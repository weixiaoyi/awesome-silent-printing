# QZ Tray 대안

**QZ Tray 대안**을 검색하는 것은 보통 브라우저에서 사일런트 프린트를 원하지만 API 스타일, 가격, 서명, HTML/CSS 워크플로에서 다른 트레이드오프를 원한다는 뜻입니다.

## QZ Tray를 유지할 때

- Raw ESC/POS / ZPL이 주 워크로드
- QZ 서명과 인증서에 이미 투자함
- 글로벌 POS 실적이 필요함
- 팀이 이미 프로덕션에서 QZ WebSocket 호출을 감싸고 있음

## 대안을 검토할 때

| 필요 | 살펴볼 것 |
|---|---|
| 넓은 파일 타입의 상용 JS 클라이언트 | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| SPA에서 npm 스타일 HTML/CSS | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), 기타 HTML 가능 브리지 |
| 여러 프린터로 클라우드 API | [PrintNode](https://www.printnode.com/en) |
| 디자이너 중심 템플릿 | hiprint + electron-hiprint |
| 단일 하드웨어 브랜드 | Zebra / Epson / Star 벤더 SDK |

## 마이그레이션 참고

1. raw vs HTML/PDF 작업 파악 — raw 작업이 비용 큰 재작성.
2. 서명 / 라이선스 가정 재테스트; 다음 벤더가 "서명 없는 사일런트"라고 가정하지 마세요.
3. 대안 파일럿 중 QZ 작업대 하나 유지.
4. 새 에이전트 포트에서 Chrome Local Network Access 재검증.

## 직접 비교

- [QZ Tray vs web-print-pdf vs JSPrintManager](qz-vs-jspm-vs-web-print-pdf.ko.md)
- [사일런트 프린트 스택 선택](choose-silent-print-stack.ko.md)
