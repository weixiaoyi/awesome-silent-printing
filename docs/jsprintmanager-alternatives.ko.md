# JSPrintManager 대안

**JSPrintManager 대안**은 보통 가격, API 스타일, 또는 사일런트 프린트를 유지하면서 다른 HTML/CSS 워크플로가 필요할 때 찾습니다.

## 유지할 때

- JSPM의 넓은 파일/인쇄/스캔 기능 세트에 의존
- 상용 지원과 다중 OS 클라이언트 지원이 가장 중요
- 조달이 이미 Neodynamic 라이선스로 표준화됨

## 필요별 대안

| 필요 | 옵션 |
|---|---|
| Raw 중심 POS 생태계 | [QZ Tray](https://qz.io/) |
| SPA에서 npm 스타일 HTML/CSS | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), 기타 HTML 가능 브리지 |
| 클라우드 인쇄 라우팅 | [PrintNode](https://www.printnode.com/en) |
| 오픈 디자이너 + Electron 클라이언트 | hiprint + electron-hiprint |
| 단일 하드웨어 브랜드 | Zebra / Epson / Star 벤더 SDK |

## 마이그레이션 참고

1. 실제로 호출하는 JSPM API 목록 (인쇄 vs 스캔 vs 파일 타입).
2. 각각을 후보의 가장 가까운 API에 매핑 — glue code 예상.
3. 라이선스 + 지원 재예산; "더 싼 SDK"가 헬프데스크 시간에서 질 수 있음.
4. 백신 켜진 한 스테이션에서 파일럿; 상용 에이전트는 한 번씩 알림을 유발하는 경우 많음.

## 관련

- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.ko.md)
- [사일런트 프린트 스택 선택](choose-silent-print-stack.ko.md)
- [Vue / React 사일런트 프린트](vue-react-silent-print.ko.md)
