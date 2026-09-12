# 원격 / 서버 푸시 사일런트 프린트

## 로컬 브라우저 호출만으로 부족할 때

- 포장 스테이션이 비즈니스 SPA를 열 필요 없음
- 작업이 중앙 WMS/OMS에서 생성됨
- 여러 작업대가 같은 큐를 소비해야 함
- 야간 근무가 다른 사이트 UI에서 만든 작업을 인쇄

## 일반적인 아키텍처

```text
Business server
    → queue / webhook / websocket feed
    → desk print agent
    → local printer
```

브라우저는 설정(스테이션 ↔ 프린터 바인딩)에만 쓰일 수 있습니다. 에이전트가 지속적으로 작업을 pull하거나 수신합니다.

### 두 가지 전송 스타일

| 스타일 | 동작 방식 | 주의 |
|---|---|---|
| 클라우드 릴레이 (PrintNode류) | 서버 API → 벤더 클라우드 → 로컬 클라이언트 | 계정 보안, 사이트별 매핑 |
| 자체 호스팅 pull | 에이전트가 스테이션 토큰으로 API 폴링 | 인증, 백오프, 내구 큐 |
| 디자이너 스택용 transit/relay | hiprint 스타일 클라이언트용 추가 홉 | 운영 복잡도 |

## 설계 참고

- **에이전트 인증**; raw print 포트를 인터넷에 노출하지 않기
- 가능하면 로컬 SDK 형태와 동일한 job 페이로드 유지 (같은 HTML/PDF/raw)
- 오프라인 작업대는 내구 큐와 poison job용 dead-letter 처리
- `job id → station → printer → result` 로깅
- 템플릿 버전 관리; 잘못된 배포는 이력 재인쇄 없이 롤백 가능해야 함
- 스테이션별 rate-limit으로 한 작업대가 다른 곳을 기아 상태로 만들지 않기

## 보안 체크리스트

- [ ] 스테이션 자격 증명 교체 가능
- [ ] API에 TLS
- [ ] 브라우저 기능용 에이전트는 localhost에만 바인딩; 원격 채널은 아웃바운드
- [ ] 신뢰할 수 없는 job 필드에서 "임의 URL 인쇄" 없음
- [ ] 어떤 스테이션에 누가 enqueue할 수 있는지 감사

## 운영 런북 (최소)

1. 스테이션 오프라인 → 마지막 heartbeat 시간과 함께 온콜 페이지
2. 프린터 용지 없음 → 스테이션 UI / LED(있으면)에 표시
3. poison 템플릿 → job 격리; 템플릿 소유자에게 알림
4. 재생 → 명시적으로 실패한 id만

## 관련

- [배치 및 라벨 인쇄](batch-label-printing.ko.md)
- [스택 선택](choose-silent-print-stack.ko.md)
- 실제 사례: PrintNode, hiprint transit relay, 원격 job pull이 있는 로컬 에이전트 ([메인 목록](../README.ko.md#cloud--remote-print) 참고)
