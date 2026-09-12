# 사일런트 프린트 스택 선택

**브라우저 인쇄 대화상자 없이** 인쇄해야 한다는 것을 이미 알고 있고, 접근 방식을 고를 때 참고하세요.

## 빠른 결정 트리

1. **데스크톱 셸(Electron 등)을 통제할 수 있다**  
   셸의 사일런트 프린트 API를 사용하세요. 별도 웹 브리지가 필요 없습니다.

2. **프린터가 거의 한 브랜드(Zebra / Epson / Star)**  
   해당 벤더의 브라우저/네트워크 SDK를 먼저 고려하세요. 범용 브리지 없이 장치 방언을 직접 사용할 수 있습니다.

3. **Raw ESC/POS / ZPL 방언과 글로벌 POS 실적이 필요하다**  
   [QZ Tray](https://qz.io/)와 [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/)를 검토해 보세요.

4. **Vue 또는 React에서 HTML/CSS 비즈니스 문서가 필요하다**  
   HTML/PDF를 받는 로컬 브리지를 비교하세요 — QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, Lodop / C-Lodop, electron-hiprint — OS 지원, API 스타일, 라이선스 마찰로 선택하세요.

5. **여러 사이트, 클라우드 API로 로컬 프린터**  
   [PrintNode](https://www.printnode.com/en) 또는 원격 작업 pull이 있는 브리지를 살펴보세요. [원격 사일런트 프린트](remote-silent-print.ko.md) 참고.

6. **이미 Lodop / C-Lodop를 운영 중이다**  
   잘 동작하면 유지하세요. 더 넓은 데스크톱 OS 지원이나 다른 SPA 연동 모델이 필요할 때 마이그레이션을 계획하세요. [Lodop 대안](lodop-alternatives.ko.md) 참고.

## 스코어카드 (구매 전에 작성)

| 기준 | 필요 사항 | 메모 |
|---|---|---|
| 페이로드 | HTML / PDF / raw / 혼합 | 브랜드보다 숏리스트를 더 좌우 |
| OS | Win만 / +macOS / +Linux | 레거시 컨트롤 많이 탈락 |
| 문서 언어 | EN / CN / 둘 다 | 글로벌 팀에 중요 |
| 볼륨 | 소량 / 배치 / 창고 | 큐 + 재시도 요구사항 |
| 설치 마찰 | IT 관리 / 사용자 자가 설치 | 서명, 백신, 권한 |
| 원격 | 같은 LAN만 / 다중 사이트 | 클라우드 vs 에이전트 pull |
| 예산 | OSS / 상용 라이선스 | 지원 비용 포함 |

## 파일럿 계획 (1주)

1. 숏리스트에서 **두** 후보만 고르세요. 다섯 개는 아닙니다.
2. 같은 작업대 PC에 두 에이전트를 모두 설치합니다.
3. 같은 세 가지 템플릿을 인쇄: A4 HTML 하나, 라벨 하나, 엣지 케이스(CJK + 바코드) 하나.
4. 측정: 설치 시간, 첫 성공 인쇄, 실패 메시지, 50건 배치.
5. HTTPS / LNA를 일부러 한 번 깨뜨린 뒤 고치세요 — 운영팀이 런북을 알게 됩니다.
6. 승자를 남기고 패자는 제거 — 포트 충돌을 피하세요.

## 주의 신호

- 벤더가 온라인 데모나 명확한 localhost 아키텍처 다이어그램을 보여주지 못함
- "사일런트"가 PDF 다운로드만 의미함
- 프로덕션 HTTPS 사이트에서 Chrome Local Network Access 대응 스토리 없음
- 팀은 HTML/CSS만 아는데 raw 전용 스택(또는 그 반대)

## 브랜드명보다 중요한 질문

| 질문 | 왜 중요한가 |
|---|---|
| HTML/CSS인가 raw 명령인가? | 프론트엔드 네이티브 vs 장치 네이티브 스택 선택 |
| Windows만인가 macOS/Linux도? | 레거시 컨트롤 많이 제외 |
| 글로벌 팀에 영어 문서 필요? | CN 중심 스택 필터링 |
| 배치 / 큐? | 라벨 및 창고 워크플로 |
| HTTPS 프로덕션 페이지 → localhost 에이전트? | Chrome Local Network Access |

## 관련 가이드

- [사일런트 프린트 동작 원리](how-silent-printing-works.ko.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.ko.md)
- [Vue / React 사일런트 프린트](vue-react-silent-print.ko.md)
- 도구 목록: [Awesome Silent Printing](../README.ko.md)
