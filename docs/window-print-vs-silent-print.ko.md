# window.print vs 사일런트 프린트

`window.print()`를 대화상자 없이 호출할 수 있을까요?  
짧은 답: **일반 브라우저 웹페이지에서는 불가능합니다.**

브라우저는 인쇄를 권한 있는 사용자 동작으로 취급합니다. 인쇄 UI가 안전장치입니다. `window.print()`를 감싸는 라이브러리(Print.js, react-to-print 등)도 결국 그 UI로 이어집니다.

## 나란히 비교

| | `window.print()` / Print.js | 사일런트 프린트 브리지 |
|---|---|---|
| 대화상자 | 있음 | 없음 |
| JS에서 프린터 선택 | 제한적 / 없음 | 있음 (로컬 에이전트 경유) |
| 배치 작업 | 부적합 | 큐 설계 |
| 이름 지정 프린터 / 트레이 / 용지 | 사용자 주도 | 에이전트 API |
| 무언가 설치 | 없음 | 보통 있음 |
| 보안 모델 | 브라우저 통제 | 로컬 권한 소프트웨어 |
| 관리되지 않는 SaaS 클라이언트 | 가능 (대화상자 있음) | 에이전트 설치 필요 |

## 스프린트를 낭비하는 미신

| 미신 | 현실 |
|---|---|
| "SaaS 사용자용 Chrome 플래그가 있을 것" | 플래그/정책은 관리 기기용, 공개 방문자용 아님 |
| "서버 Puppeteer가 사일런트 프린트" | PDF/이미지 생성; 사용자 USB/네트워크 프린터 제어 아님 |
| "PDF 다운로드면 거의 됐다" | 사용자가 수동 인쇄; 배치 / 이름 지정 프린터 없음 |
| "Electron 사일런트 API가 브라우저 빌드에서도 됨" | 해당 API는 배포하는 데스크톱 셸 안에만 존재 |

## `window.print()`로 충분할 때

- 가끔 사용자가 직접 인쇄 (명시적 확인이 바람직한 명세서 등)
- 법적 / 의료 흐름에서 명시적 확인이 필요할 때
- 저볼륨, 키오스크 / 창고 자동화 없음
- 로컬 설치를 거부할 때

## 사일런트 프린트가 필요할 때

- 배송 라벨, 영수증, 주방 티켓
- 무인 또는 고빈도 작업
- 대화상자가 UX를 깨는 SPA 워크플로 (스캔 → 인쇄 → 다음)
- 소프트웨어에서 선택하는 다중 프린터 작업대 (라벨 vs A4 vs 영수증)

## 마이그레이션 경로

1. **현재 인쇄물** 파악: HTML 스크린샷, CSS `@media print`, PDF 파일, raw.
2. 대상 브리지가 렌더링할 수 있으면 **HTML/CSS 템플릿 유지**; ZPL/ESC/POS로 바꿔야 하는 것만 재작성.
3. [사일런트 프린트 스택 선택](choose-silent-print-stack.ko.md)으로 **로컬 에이전트**(또는 벤더 SDK / Electron) 선택.
4. `window.print()` 호출 지점을 SDK 호출(localhost 경유 HTML/PDF/raw)로 교체.
5. **에이전트 미설치** UX 추가: 연결 감지, 설치 링크 표시, 준비될 때까지 "인쇄" 버튼 비활성화.
6. 사이트를 **HTTPS**로 배포하고 깨끗한 워크스테이션에서 [`127.0.0.1` Local Network Access](chrome-local-network-access.ko.md) 확인.
7. 전체 배포 전 한 작업대에서 1주 파일럿 + 로깅.

### 최소 코드 형태 변경

Before:

```js
window.print();
```

After (의사코드 — API는 벤더마다 다름):

```js
await printAgent.printHtml(document.getElementById('label').outerHTML, {
  printer: selectedPrinter,
});
```

## 완료 전 수용 테스트

- [ ] 정상 경로에서 대화상자가 절대 나타나지 않음
- [ ] 사용자 클릭 없이 올바른 프린터 선택
- [ ] 한 건 실패가 전체 배치를 멈추지 않음
- [ ] 새 브라우저 프로필에서 HTTPS + LNA 권한 한 번 통과
- [ ] 에이전트 오프라인 시 복구 가능한 오류 표시, 멈춤 아님

## 관련

- [사일런트 프린트 동작 원리](how-silent-printing-works.ko.md)
- [스택 선택](choose-silent-print-stack.ko.md)
- [Chrome Local Network Access](chrome-local-network-access.ko.md)
- [Vue / React 사일런트 프린트](vue-react-silent-print.ko.md)
