# 웹에서 배치 및 라벨 인쇄

## 문제

창고와 이커머스 작업대는 종종 **수십~수백** 장의 라벨을 인쇄 대화상자를 클릭하지 않고 필요로 합니다. 라벨당 대화상자 하나는 운영 설계가 아니라 장애입니다.

## 필요한 것

1. **이름 지정** 프린터로 가는 사일런트 경로
2. 작업 큐 / 배치 API (클라이언트 루프만으로는 규모에서 부족)
3. 안정적인 템플릿 렌더링 (HTML 또는 ZPL)
4. 배치 중간 실패 시 재시도 + 로깅
5. 프린터가 제출보다 느릴 때 백프레셔

## 패턴

| 패턴 | 메모 | 적합한 경우 |
|---|---|---|
| 브라우저 → 로컬 에이전트 배치 API | 작업대 PC에서 최저 지연 | 운영자가 SPA 안에서 작업 |
| 서버 푸시 → 작업대 에이전트 pull | 많은 포장 스테이션에 유리 | 브라우저를 열어둘 필요 없음 |
| 벤더 raw (ZPL) | 순수 라벨 프린터에 우수 | ZPL로 표준화된 플릿 |

## 권장 배치 흐름

```text
Select orders
  → render or fetch N templates
  → submit as a batch (or chunked batches of 20–50)
  → show per-job status (queued / printing / done / failed)
  → retry failed ids only
```

### 청킹

500건을 한 Promise.all로 제출하면 도움이 되기보다 해가 될 때가 많습니다. 청크를 선호하세요.

- HTML 렌더링 에이전트는 청크당 20–50건
- 작은 ZPL 문자열은 더 큰 청크 가능
- 다음 전에 청크 완료 대기(또는 동시성 제한 2–3)

## 실패 분류

| 실패 | 운영자 조치 | 시스템 조치 |
|---|---|---|
| 에이전트 오프라인 | 설치 / 에이전트 시작 | 큐 일시정지; 배너 |
| 프린터 오프라인 / 용지 없음 | 하드웨어 수리 | 작업 재시도 가능 표시 |
| 잘못된 템플릿 / 바코드 | 데이터 수정 | 해당 작업만 실패; 나머지 계속 |
| LNA / HTTPS | IT가 origin 수정 | [Chrome LNA](chrome-local-network-access.ko.md) 참고 |

## 체크리스트

- [ ] 용지 크기 / 스테이션별 프린터 선택
- [ ] 배치 제출 + 작업별 상태
- [ ] HTTPS 페이지가 localhost에 연결 가능 (LNA)
- [ ] 바코드/QR 템플릿 회귀 테스트
- [ ] 멱등 job id (재제출 안전)
- [ ] 운영팀이 실패 job id 내보내기 가능

## 관련

- [원격 사일런트 프린트](remote-silent-print.ko.md)
- [HTML/CSS 사일런트 프린트](html-css-silent-print.ko.md)
- [열전사 영수증 사일런트 프린트](thermal-receipt-silent-print.ko.md)
- [스택 선택](choose-silent-print-stack.ko.md)
