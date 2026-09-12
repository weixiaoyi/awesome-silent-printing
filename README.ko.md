<div align="center">

# Awesome Silent Printing

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [Español](README.es.md) | [Português (Brasil)](README.pt-BR.md) | **한국어** | [Deutsch](README.de.md) | [Русский](README.ru.md)

</div>

> 웹 애플리케이션에서 **사일런트 인쇄**(무인 인쇄)를 위한 도구, 라이브러리, 브리지, 리소스 큐레이션 목록 — 브라우저 인쇄 대화 상자 없이 출력합니다.
>
> 함께 다루는 주제: `window.print` 제한, Chrome의 `127.0.0.1` Local Network Access, Vue/React 사일런트 인쇄, HTML/CSS 프린트 에이전트, Lodop 대안, QZ Tray / web-print-pdf / JSPrintManager 비교, 배치 라벨, 원격 인쇄.

---

## 이 목록이 있는 이유

브라우저는 보안상 사일런트 인쇄를 의도적으로 차단합니다. 영수증, 배송 라벨, 송장, 주방 티켓 등이 필요한 팀은 보통 **로컬 브리지**, **확장 프로그램**, **키오스크 정책**, **벤더 SDK** 중 하나를 씁니다.

이 목록은 그 좁은 문제에 집중합니다: **`window.print()` 대화 상자 없이 웹에서 인쇄하는 방법**.

---

## 가이드

실용적인 장문 가이드. 전체 목록: [docs/](docs/README.md).

> 긴 가이드 (한국어):

- [브라우저 사일런트 인쇄 허브](docs/browser-silent-print.ko.md)
- [사일런트 인쇄 작동 방식](docs/how-silent-printing-works.ko.md)
- [사일런트 인쇄 스택 선택](docs/choose-silent-print-stack.ko.md)
- [window.print vs 사일런트 인쇄](docs/window-print-vs-silent-print.ko.md)
- [Chrome Local Network Access와 127.0.0.1](docs/chrome-local-network-access.ko.md)
- [Vue / React 사일런트 인쇄](docs/vue-react-silent-print.ko.md)
- [HTML/CSS 사일런트 인쇄](docs/html-css-silent-print.ko.md)
- [웹 배치 / 라벨 인쇄](docs/batch-label-printing.ko.md)
- [원격 / 서버 푸시 사일런트 인쇄](docs/remote-silent-print.ko.md)
- [QZ Tray vs web-print-pdf vs JSPrintManager](docs/qz-vs-jspm-vs-web-print-pdf.ko.md)
- [Lodop 대안](docs/lodop-alternatives.ko.md)
- [hiprint 대안](docs/hiprint-alternatives.ko.md)
- [QZ Tray 대안](docs/qz-tray-alternatives.ko.md)
- [JSPrintManager 대안](docs/jsprintmanager-alternatives.ko.md)
- [브라우저 열전사 영수증 사일런트 인쇄](docs/thermal-receipt-silent-print.ko.md)

---

## 목차

- [가이드](#가이드)
- [브라우저 제한 (먼저 읽기)](#브라우저-제한-먼저-읽기)
- [사일런트 인쇄 작동 방식](#사일런트-인쇄-작동-방식)
- [선택 방법](#선택-방법)
- [비교 매트릭스](#비교-매트릭스)
- [로컬 프린트 브리지](#로컬-프린트-브리지)
- [하드웨어 벤더 SDK](#하드웨어-벤더-sdk)
- [클라우드 / 원격 인쇄](#클라우드--원격-인쇄)
- [데스크톱 / Electron](#데스크톱--electron)
- [오픈 소스 프로젝트](#오픈-소스-프로젝트)
- [사일런트 아님 (흔한 혼동)](#사일런트-아님-흔한-혼동)
- [보안 참고](#보안-참고)
- [기여하기](#기여하기)
- [번역](#번역)

---

## 브라우저 제한 (먼저 읽기)

| 메커니즘 | 사일런트? | 참고 |
|---|---|---|
| `window.print()` | 아니오 (기본) | 브라우저가 인쇄 대화 상자를 표시함; 페이지 JS가 원하는 장치로 완전히 무인 출력할 수 없음 |
| Print.js / react-to-print | 아니오 | 여전히 브라우저 인쇄 UI를 연다 |
| Chrome 키오스크 / 엔터프라이즈 인쇄 정책 | 조건부 | 관리형 / 키오스크 장치에서만 동작 |
| Chrome / Edge **`127.0.0.1` Local Network Access (LNA)** | 로컬 브리지에 영향 | 비로컬 페이지가 루프백에 접근하려면 **보안 컨텍스트(HTTPS)** 가 필요함; 일반 HTTP는 종종 **조용히 거부**됨. 최신 Chromium은 **WebSocket**(`ws://127.0.0.1…`)에도 동일 규칙을 적용함. 사용자에게 로컬 네트워크 권한 프롬프트가 뜰 수 있음. `localhost`에서 개발할 때는 보통 문제없음; 프로덕션 HTTP는 많은 에이전트를 망가뜨림. |
| 진정한 크로스 사이트 사일런트 인쇄 | 로컬 에이전트 필요 | 일반 패턴: localhost HTTP/WebSocket / Native Messaging → OS 스풀러 또는 raw 포트 |

이 LNA 동작은 특정 벤더가 아니라 **모든** localhost 프린트 브리지(QZ, web-print-pdf, JSPM, Lodop 클라우드-로컬 패턴 등)에 해당합니다. 자세한 설명: [배포 후 WebSocket이 127.0.0.1에 연결 실패](https://webprintpdf.com/en/docs/production-print-troubleshoot/).

---

## 사일런트 인쇄 작동 방식

| 접근 방식 | 개념 | 흔한 트레이드오프 |
|---|---|---|
| 로컬 브리지 / 에이전트 | 페이지가 프린터를 소유하는 localhost 서비스와 통신 | 클라이언트 설치 필요 |
| 브라우저 확장 + 네이티브 호스트 | 확장이 Native Messaging 호스트 호출 | 스토어 심사 및 신뢰 마찰 |
| 엔터프라이즈 / 키오스크 정책 | 브라우저 인쇄 설정 잠금 | 통제된 장치에 적합 |
| 벤더 SDK | Epson / Zebra / Star 스택과 통신 | 하드웨어 종속 |
| 데스크톱 셸 (Electron 등) | Chromium 내장; 네이티브 인쇄 API 사용 | 순수 브라우저 앱은 아님 |

---

## 선택 방법

1. **성숙한 글로벌 POS / raw + 픽셀 생태계** → [QZ Tray](https://qz.io/), [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/).
2. **SPA(Vue / React 등)에서 HTML/CSS 업무 문서** → HTML/PDF를 받는 로컬 브리지 비교, 예: QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, [Lodop / C-Lodop](http://www.c-lodop.com/), [electron-hiprint](https://github.com/CcSimple/electron-hiprint).
3. **이미 레거시 프린트 컨트롤 사용 중** → Lodop / hiprint 스택을 계속 평가; OS 지원 또는 SPA DX가 병목일 때만 이전.
4. **Linux 데스크톱(필요 시 Kylin / UOS 포함)** → 실제 Linux 클라이언트가 있는 브리지 우선(QZ Tray, web-print-pdf, JSPrintManager 등).
5. **Zebra / Epson / Star 프린터만 사용** → 해당 [벤더 SDK](#하드웨어-벤더-sdk) 우선.
6. **글로벌 팀을 위한 영어 우선 문서 / UI** → [비교 매트릭스](#비교-매트릭스)에서 **English**가 ✅인 도구 우선; Lodop과 많은 hiprint 자료는 중국어 중심.
7. **클라우드 API → 여러 지점 프린터** → [PrintNode](https://www.printnode.com/en) 또는 원격 가능 로컬 에이전트.
8. **오픈소스 SDK / 학습** → [오픈 소스 프로젝트](#오픈-소스-프로젝트) 참조; 작은 저장소는 유지보수가 들쭉날쭉할 수 있음.

---

## 비교 매트릭스

### 플랫폼 및 페이로드

| 도구 | Win | macOS | Linux | 영어 | 온라인 데모 | HTML/CSS | PDF | Raw (ESC/POS, ZPL…) | 배치 | 원격 |
|---|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://demo.qz.io/) | ✅ | ✅ | ✅ 강함 | ✅ | 앱 경유 |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ | ✅ | HTML/PDF 경유 | ✅ | ✅ |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | ✅ | ✅ | ✅ 강함 | ✅ | 제품 경유 |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ✅ | — | 부분 | ⚠️ 중국어 위주 | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ✅ | ✅ | 부분 | ✅ | 클라우드 모드 |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ✅ | ✅ | ✅ | ⚠️ 중국어 위주 | ⚠️ 디자이너 데모 위주 | ✅ | ✅ | — | ✅ | 중계 경유 |
| [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) | ✅ | ✅ | — | ✅ | ⚠️ 샘플 / 로컬 | — | 이미지 | ZPL/raw | 제한적 | — |
| [PrintNode](https://www.printnode.com/en) | ✅ | ✅ | ✅ | ✅ | ⚠️ API 문서 | — | ✅ | ✅ | ✅ | ✅ |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | ✅ | — | — | —(**비사일런트**) |

### API 친화도 (프론트엔드 DX)

점수는 **SPA / npm 시대** 팀 기준의 참고치입니다. raw POS 전문가는 QZ / JSPM을 여전히 선호할 수 있습니다.

| 도구 | 영어 | 온라인 데모 | npm 패키지 | Promise / `async` | 원라인 HTML 인쇄 | Vue / React 적합 | 레이아웃 | 학습 곡선 | 인증 / 서명 마찰 |
|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ [demo](https://demo.qz.io/) | ❌(스크립트 + WS) | Promise 래핑 흔함 | 가능, 설정 더 많음 | 직접 래핑 | 픽셀 + raw 우선 | 중~고 | 사일런트 시 높음 |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ `web-print-pdf` | ✅ | ✅ | ✅ | HTML/CSS | 중하 | 클라이언트 설치 |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | 부분 / 스크립트 중심 | 혼합 | 있음 | 직접 래핑 | 혼합 페이로드 | 중 | 라이선스 + 클라이언트 |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ⚠️ 중국어 위주 | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ❌ | 콜백 스타일 | 레거시 API | 직접 래핑 | 전용 명령 + HTML | 중 | 서비스 / 플러그인 설치 |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ⚠️ 중국어 위주 | ⚠️ 디자이너 데모 위주 | 생태계 패키지 | Socket.IO 이벤트 | 템플릿 경유 | vue-plugin-hiprint와 궁합 좋음 | 디자이너 템플릿 | 중 | 클라이언트 설치 |
| [Zebra](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) / [Epson](https://download.epson-biz.com/) / [Star](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) SDKs | ✅ | ⚠️ 벤더 샘플 | 벤더 스크립트 | 제품별 | 아니오 | 직접 래핑 | 장치 명령 세트 | 하드웨어 종속 | 벤더 스택 |
| [PrintNode](https://www.printnode.com/en) | ✅ | ⚠️ API 문서 | REST / 바인딩 | ✅ | PDF/raw 지향 | 백엔드 친화 | 파일 / raw | 중 | 계정 + 클라이언트 |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | 얇은 헬퍼 | 대화상자 호출 | 쉬움 | 브라우저 인쇄 CSS | 낮음 | 해당 없음 — **비사일런트** |

페이로드(HTML vs raw), OS 지원, 서명 / 라이선스 마찰에 맞춰 스택을 고르세요. 기호는 참고용이며, 항상 벤더 사이트에서 확인하세요.

---

## 로컬 프린트 브리지

작은 로컬 런타임을 설치하고 HTTP / WebSocket / 네이티브 API로 페이지에 노출하는 크로스 브라우저 솔루션.

- [QZ Tray](https://qz.io/) — 성숙한 로컬 브리지; raw + 픽셀 인쇄; POS / 라벨링에 널리 사용. 사일런트 모드는 보통 서명 / 라이선스 필요.
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — HTML/PDF 무인 인쇄용 로컬 에이전트 + npm SDK; Windows, macOS, Linux.
- [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) — 상용 JS + 클라이언트; 강한 멀티 OS 지원; WebSocket 사일런트 인쇄 / 스캔.
- [Lodop / C-Lodop](http://www.c-lodop.com/) — 오래된 로컬 프린트 컨트롤; Windows ERP/HIS 배포에 흔함; Lodop7은 Linux 지원 확대.
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) (+ [vue-plugin-hiprint](https://github.com/CcSimple/vue-plugin-hiprint)) — 오픈소스 hiprint 디자이너 + Electron 사일런트 클라이언트.
- [PortixOne](https://github.com/portixhq/portixone) — 웹 앱과 로컬 하드웨어 연결용 오픈소스 엣지 런타임(초기).
- [PrintBridge](https://printbridge.app/) — 로컬 REST 사일런트 인쇄 API를 제공하는 상용 Windows 트레이 에이전트. *(아래 OSS 저장소와 동일하지 않음.)*
- [SilentPrint](https://github.com/wxingheng/SilentPrint) — 웹 페이지에서 무인 인쇄를 위한 Windows 미들웨어.
- [PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket) — POS / 열전사 사일런트 인쇄용 Python WebSocket 서버 + JS 클라이언트.
- [silent-print](https://github.com/atefe-aa/silent-print) — 사일런트 HTML 인쇄용 로컬 HTTP API를 노출하는 Windows 서비스.

---

## 하드웨어 벤더 SDK

프린터 함대가 대부분 한 브랜드일 때 적합.

- [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) — Zebra 중심 브라우저 인쇄(로컬 서비스 + JS).
- [Epson ePOS SDK for JavaScript](https://download.epson-biz.com/) — 페이지에서 네트워크로 Epson TM 구동.
- [Star Micronics webPRNT](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) — Star 프린터 제어용 JS 임베드.

---

## 클라우드 / 원격 인쇄

- [PrintNode](https://www.printnode.com/en) — 클라우드 API → 로컬 클라이언트 → 프린터; 흔한 Google Cloud Print 대체.
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — 선택적 원격 작업 pull이 있는 로컬 에이전트.
- [node-hiprint-transit](https://github.com/Xavier9896/node-hiprint-transit) — 네트워크 간 hiprint 클라이언트 릴레이.
- Google Cloud Print — **종료됨**; 역사적 맥락으로만 기재.

---

## 데스크톱 / Electron

- Electron `webContents.print({ silent: true })` — 제어하는 데스크톱 셸 내부에서 동작; 임의 웹사이트에는 사용 불가.
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) — 사일런트 프린트 브리지로 쓰이는 Electron 클라이언트.
- [electron-silent-print](https://github.com/mpoapostolis/electron-silent-print) — 초기 Electron 사일런트 인쇄 예제.

---

## 오픈 소스 프로젝트

SDK 또는 출발점으로 유용한 MIT / 커뮤니티 저장소(품질 및 유지보수는 다양).

- [weixiaoyi/PrintWeb](https://github.com/weixiaoyi/PrintWeb)
- [wxingheng/SilentPrint](https://github.com/wxingheng/SilentPrint)
- [TawsifTorabi/PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket)
- [atefe-aa/silent-print](https://github.com/atefe-aa/silent-print)
- [portixhq/portixone](https://github.com/portixhq/portixone)
- [AnouarSbia/printbridge](https://github.com/AnouarSbia/printbridge) — OSS 에이전트(PDF / TSPL); **printbridge.app과 다름**
- [CcSimple/electron-hiprint](https://github.com/CcSimple/electron-hiprint) — [로컬 프린트 브리지](#로컬-프린트-브리지)에도 기재.

---

## 사일런트 아님 (흔한 혼동)

같은 검색에 자주 나오지만 **스스로는** 진정한 사일런트 인쇄를 제공하지 **않음**:

- [Print.js](https://printjs.crabbly.com/) — 브라우저 인쇄 대화 상자 헬퍼
- jsPDF / html2pdf.js — PDF 생성 또는 다운로드; 로컬 무인 프린터 구동 안 함
- `window.print()` — [브라우저 제한](#브라우저-제한-먼저-읽기) 참조

---

## 보안 참고

- 사일런트 인쇄는 사용자 확인 UI를 우회함 — 로컬 브리지를 **권한 있는 소프트웨어**로 취급하세요.
- 인증된 localhost API, 고정 origin, 서명된 클라이언트를 우선하세요.
- 강력한 인증 없이 raw 프린트 에이전트를 공용 인터넷에 노출하지 마세요.

---

## 기여하기

PR 환영합니다. 항목은 사실 위주로: 이름, 링크, 한 줄 설명, 주요 제약(OS, 라이선스, 하드웨어 종속). [CONTRIBUTING.md](CONTRIBUTING.md) 참조.

---

## 번역

| 언어 | 파일 | 상태 |
|---|---|---|
| English | [README.md](README.md) | Done (canonical) |
| 中文 | [README.zh-CN.md](README.zh-CN.md) | Done |
| 日本語 | [README.ja.md](README.ja.md) | Done |
| Español | [README.es.md](README.es.md) | Done |
| Português (Brasil) | [README.pt-BR.md](README.pt-BR.md) | Done |
| 한국어 | [README.ko.md](README.ko.md) | Done |
| Deutsch | [README.de.md](README.de.md) | Done |
| Русский | [README.ru.md](README.ru.md) | Done |

상단 언어 전환기는 모든 언어 버전을 나열합니다.

---

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, this list is released under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/).
