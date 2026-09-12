# QZ Tray vs web-print-pdf vs JSPrintManager

**로컬 사일런트 프린트 브리지**를 고르는 팀을 위한 실용 비교입니다. 숫자와 제품 표면은 변합니다 — 구매 전 항상 벤더 사이트에서 확인하세요.

## 스냅샷

| 차원 | [QZ Tray](https://qz.io/) | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
|---|---|---|---|
| 포지셔닝 | 성숙한 POS / raw + 픽셀 브리지 | 로컬 에이전트 + npm SDK | 상용 JS + 클라이언트, 인쇄 및 스캔 |
| Raw ESC/POS / ZPL | 강함 | 보통 HTML/PDF 경로 | 강함 |
| HTML/CSS 비즈니스 문서 | 지원 | 지원 | 지원 |
| npm / async DX | Script + WS 중심 | `npm` + Promise/`async` | Script 중심 |
| 영어 문서 | 있음 | 있음 | 있음 |
| 온라인 데모 | [demo.qz.io](https://demo.qz.io/) | [demos](https://webprintpdf.com/en/docs/demos/) | [azure demo](https://jsprintmanager.azurewebsites.net/) |
| 사일런트 마찰 | 서명 / 라이선스 흔함 | 클라이언트 설치 | 라이선스 + 클라이언트 |
| 자주 선택되는 경우 | Raw 방언 + 글로벌 POS 실적 | HTML/CSS 템플릿과 npm 스타일 SPA 호출 | 넓은 파일 타입 / 상용 지원 |

## 더 깊은 메모

### QZ Tray

- 강점: raw 인쇄 문화, 픽셀 인쇄, POS/라벨 커뮤니티에서 오래된 존재.
- 프로덕션 사일런트 모드가 필요하면 인증서 / 서명 워크플로 계획.
- 프론트엔드 연동은 보통 script + WebSocket; 팀이 자체 Promise 헬퍼로 감싸는 경우 많음.

### web-print-pdf (Web Print Expert)

- 강점: 이미 HTML/CSS와 npm으로 생각하는 SPA 팀.
- Raw 방언은 보통 주 경로가 아님 — ESC/POS/ZPL이 핵심 워크로드면 신중히 평가.
- 작업대 플릿에 대해 Linux/macOS/Windows 에이전트 지원 확인.

### JSPrintManager

- 강점: 상용 기능 폭(에디션에 따라 인쇄 + 관련 장치 워크플로).
- 롤아웃 비용의 일부로 라이선스 + 클라이언트 설치 예상.
- 조달이 넓은 파일 타입 스토리가 있는 단일 상용 벤더를 원할 때 좋은 후보.

## 경험 법칙

- **장치 방언 우선** → QZ / JSPM이 일반적인 숏리스트.
- **SPA에서 HTML/CSS** → 셋 다 가능; 파일럿 작업대에서 데모 + 설치 마찰 비교.
- **여러 사이트로 클라우드 라우팅** → PrintNode도 평가.

## 파일럿 체크리스트 (셋 모두 동일)

- [ ] 깨끗한 PC에 에이전트 설치 (백신 켜짐)
- [ ] HTML A4 하나와 라벨/티켓 하나 인쇄
- [ ] 현재 Chrome에서 HTTPS 사이트 → localhost 확인
- [ ] 50건 배치 측정
- [ ] 법무/IT와 라이선스 / 서명 요구사항 검토

## 관련

- [스택 선택](choose-silent-print-stack.ko.md)
- [메인 목록의 API 비교](../README.ko.md#api-friendliness-frontend-dx)
- [QZ Tray 대안](qz-tray-alternatives.ko.md)
- [JSPrintManager 대안](jsprintmanager-alternatives.ko.md)
