# Lodop 대안

Lodop / C-Lodop는 여전히 많은 Windows 비즈니스 시스템에서 흔합니다. 팀이 대안을 찾는 경우는 보통 다음이 필요할 때입니다.

- 현대 SPA 연동
- 더 넓은 데스크톱 OS 지원 (macOS / Linux 작업대)
- 혼합 팀을 위한 영어 중심 문서
- 현재 Chromium 규칙 하에서 더 명확한 HTTPS + localhost 동작

## 대체 방향

| 필요 | 후보 |
|---|---|
| HTML/CSS 비즈니스 문서 | QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, hiprint + electron-hiprint |
| Raw POS / 라벨 | [QZ Tray](https://qz.io/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| 여러 프린터로 클라우드 | [PrintNode](https://www.printnode.com/en) |
| Lodop 유지 | Lodop7 / C-Lodop (스택이 여전히 맞을 때) |

## 마이그레이션이 멈추는 이유

- 템플릿이 독점 Lodop 명령과 HTML 조각이 섞여 있음
- 프린터 이름과 용지함이 오래된 스크립트에 인코딩됨
- 병원 / ERP가 성수기에 동작하는 인쇄 경로 변경을 두려워함

## 마이그레이션 팁

1. 템플릿 파악: HTML vs 독점 명령. "이미 HTML만"인 것이 몇 개인지 세기.
2. 가능하면 중요 문서를 HTML/CSS로 재구성; 특수 raw는 2차 웨이브로.
3. 파일럿 작업대에서 구/신 에이전트 병렬 실행 (다른 포트).
4. 전국 배포 전 HTTPS / Local Network Access 수정.
5. 헬프데스크에 새 "에이전트 미실행" 증상 교육 — 예전 ActiveX 시대 오류를 대체.

## 관련

- [스택 선택](choose-silent-print-stack.ko.md)
- [window.print vs 사일런트 프린트](window-print-vs-silent-print.ko.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.ko.md)
