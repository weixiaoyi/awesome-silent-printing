# Vue / React 사일런트 프린트

## 목표

`window.print()` 없이 SPA에서 사일런트 프린트를 호출하는 것.

## 일반적인 연동

1. 사용자가 로컬 프린트 에이전트를 한 번 설치합니다.
2. 프론트엔드가 npm SDK 또는 벤더 JS(QZ, web-print-pdf, JSPM 등)에 의존합니다.
3. 페이지가 HTML, PDF 또는 raw 페이로드를 `127.0.0.1`로 보냅니다.
4. 에이전트가 렌더링 / OS 프린터로 전달합니다.

호출 형태 (API는 벤더마다 다름):

```js
// 의사코드 — 브리지 SDK 문서 확인
await printAgent.printHtml(
  '<div class="label">Order #1001</div>',
  { printer: 'LabelPrinter' }
);
```

### 권장 모듈 경계

Vue/React 컴포넌트를 단순하게 유지하려면 작은 서비스 뒤에 프린트를 두세요.

```js
// printService.js
export async function printLabel(html, printer) {
  await ensureAgent();
  return printAgent.printHtml(html, { printer });
}
```

- 앱 부팅 또는 첫 인쇄 화면 전에 `ensureAgent()` 호출.
- 에이전트가 없으면 설치 안내 표시.
- 열 개 컴포넌트에서 서로 다른 options 객체로 직접 print 호출하지 않기.

## SPA 함정

| 함정 | 해결 |
|---|---|
| 에이전트 준비 전 print 호출 | 사전 연결 / 설치 안내 표시 |
| 하드코딩된 프린터 이름 | 프린터 목록 조회; 스테이션별 저장 |
| HTTP 프로덕션 origin | Local Network Access를 위해 HTTPS로 이동 |
| 화면과 스타일 다름 | Chromium 기반 HTML→print 에이전트 선호; 폰트 고정 |
| UI 크롬이 가득한 Vue/React 트리 인쇄 | 전용 print root / 오프스크린 템플릿 렌더링 |
| 거대한 base64 인라인 이미지 | 에이전트가 fetch할 URL 또는 압축 선호 |
| Promise rejection 무시 | toast + 재시도로 매핑; job id 로깅 |

## Vue 참고

- 전체 페이지 레이아웃이 아닌 인쇄 전용 SFC(`LabelTicket.vue`)에 템플릿 배치.
- 마운트된 print root의 `ref` + `innerHTML` / `outerHTML`, 또는 데이터에서 HTML 문자열 생성 선호.
- `<Transition>` 또는 가상 리스트 업데이트 중에는 인쇄 피하기.

## React 참고

- 같은 아이디어: 숨겨진 컨테이너에 렌더링한 `PrintTicket` 컴포넌트, HTML 직렬화.
- portal과 concurrent rendering 주의 — 데이터가 안정적일 때 스냅샷.
- `useEffect` 안의 `window.print()`를 "임시" 사일런트 경로로 쓰지 마세요. 잘못된 습관을 만듭니다.

## 연결 수명 주기

```text
App start
  → ping agent
  → if down: banner + install link
  → if up: cache printer list
User clicks Print
  → re-ping (cheap)
  → submit job
  → show job result / error code
```

## 관련

- [HTML/CSS 사일런트 프린트](html-css-silent-print.ko.md)
- [Chrome Local Network Access](chrome-local-network-access.ko.md)
- [스택 선택](choose-silent-print-stack.ko.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.ko.md)
