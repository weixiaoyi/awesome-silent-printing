# Chrome Local Network Access & 127.0.0.1

## 증상

개발에서는 됩니다. 프로덕션에서는 이런 오류가 납니다.

```text
WebSocket connection to 'ws://127.0.0.1:…' failed
Failed to connect to print agent
ERR_CONNECTION_REFUSED / net::ERR_FAILED
```

로컬 프린트 클라이언트는 실행 중입니다. 방화벽도 괜찮아 보입니다. **배포된 웹사이트만** 루프백에 연결할 수 없습니다.

모든 localhost 브리지에서 "배포 후 사일런트 프린트가 깨졌다"는 가장 흔한 사고 중 하나입니다.

## 원인

Chromium의 **Local Network Access (LNA)**는 공개 페이지가 사용자 로컬 네트워크 / 루프백과 대화하는 것을 제한합니다. `127.0.0.1`에서 수신하는 프린트 에이전트는 정확히 그 제한 구역에 있습니다.

현장에서 팀이 마주치는 실용 규칙:

| 컨텍스트 | 일반적 결과 |
|---|---|
| 개발에서 `http://localhost` 앱 | 종종 동작 (특수 컨텍스트) |
| 프로덕션 **HTTP** 앱 | 종종 **조용히 거부** |
| 신뢰할 수 있는 인증서가 있는 **HTTPS** 앱 | Local Network 권한 프롬프트 가능 |
| 최신 Chrome + 루프백 WebSocket | `ws://127.0.0.1…`에도 동일 LNA 규칙 |
| Edge / 기타 Chromium | 유사 정책 |

에이전트가 "켜져 있다"는 것은 필요하지만 충분하지 않습니다. **브라우저 origin**이 루프백과 대화할 수 있어야 합니다.

## 결정 흐름도

```text
Can the page reach ws://127.0.0.1 / http://127.0.0.1?
│
├─ No, and site is HTTP
│     → Put the site on HTTPS first. Stop here until that ships.
│
├─ No, and site is HTTPS
│     → Check Local Network permission / prompt
│     → Confirm agent port + process
│     → Test from the same machine with a tiny WS client
│
└─ Yes, but print still fails
      → Printer name, driver, spooler, template — not LNA
```

## 해결 방법 (체크리스트)

1. 워크스테이션이 신뢰하는 인증서(공개 CA 또는 내부 PKI)로 **웹 앱을 HTTPS**로 제공하세요. 사용자가 신뢰하지 않은 자체 서명 인증서는 계속 실패합니다.
2. 첫 인쇄 시 브라우저 **Local Network** 권한 프롬프트를 확인하고 origin에 허용하세요.
3. 데스크톱 프린트 에이전트가 `127.0.0.1`(및 SDK가 기대하는 포트)에서 수신 중인지 확인하세요.
4. DevTools → Network로 재현: WS/HTTP 호출이 차단, 거부, 리셋 중 무엇인지 확인하세요.
5. **관리 플릿만**: 브라우저 플래그 / 엔터프라이즈 정책으로 검사 완화 가능. 공개 SaaS 최종 사용자 전략은 아닙니다.
6. 설치 런북에 권한 단계를 문서화하세요. 그렇지 않으면 헬프데스크가 에이전트만 계속 재설치합니다.

## LNA와 "에이전트 다운" 구분

| 확인 | 에이전트 다운 | LNA / origin 문제 |
|---|---|---|
| 로컬 도구에서 `127.0.0.1:port` | 실패 | 성공 |
| 같은 PC, HTTP 사이트 | 실패할 수 있음 | 종종 실패 |
| 같은 PC, HTTPS + 권한 | 에이전트 up이면 동작 | 동작 |
| Chrome 다른 사용자 프로필 | 동일 | 권한 누락 가능 |

## 영향 받는 대상

모든 localhost 프린트 브리지: QZ Tray, web-print-pdf, JSPrintManager, Lodop 스타일 로컬 서비스, 커스텀 에이전트. 단일 벤더 버그가 아닙니다.

## 더 자세한 안내

- English: [WebSocket to 127.0.0.1 failed after deploy](https://webprintpdf.com/en/docs/production-print-troubleshoot/)
- 中文: [上线后连接 127.0.0.1 失败](https://webprintpdf.com/docs/production-print-troubleshoot/)

## 관련

- [사일런트 프린트 동작 원리](how-silent-printing-works.ko.md)
- [Vue / React 사일런트 프린트](vue-react-silent-print.ko.md)
- [스택 선택](choose-silent-print-stack.ko.md)
