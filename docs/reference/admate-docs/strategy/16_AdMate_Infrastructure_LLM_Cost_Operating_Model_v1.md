# AdMate Infrastructure / LLM Cost Operating Model v1

작성일: 2026-05-06

목적: AdMate가 내부 사용자 최대 100명 규모로 확장될 때 필요한 서버, LLM, 오픈 모델, GPU, 비용 통제 구조를 정리한다. 이 문서는 향후 인프라 증설, OpenRouter credit 충전, 오픈 LLM PoC, Agent orchestration 운영 비용 판단 시 참고한다.

## 1. 최상위 판단

지금 단계에서는 고정 GPU 서버를 바로 운영하지 않는다.

권장 구조:

```text
앱/DB/권한/저장소: Vercel + Supabase + Hetzner CPU worker
상업용 LLM: OpenRouter + 필요 시 OpenAI/Claude/Gemini direct
오픈 LLM: 초기에는 API 또는 on-demand GPU PoC
상시 GPU 서버: 사용량과 보안 요구가 증명된 뒤 검토
```

이유:

- 현재 AdMate는 제품 기능, 권한, 데이터 흐름, RAG 품질 안정화가 우선이다.
- 100명 사용자가 있어도 동시 GPU inference가 계속 필요한 구조는 아직 아니다.
- 상시 GPU는 고정비가 크고, 모델/가격/성능 변화가 빠르다.
- 초기에는 credit 기반 LLM과 on-demand GPU가 비용 통제에 유리하다.

## 2. 권장 인프라 역할 분리

### Hetzner CPU 서버

용도:

- Openclaw worker
- n8n 또는 workflow runner
- Slack webhook receiver
- lightweight scheduler / cron
- background worker
- log shipping helper

비권장:

- 대형 LLM inference 상시 실행
- GPU workload
- 장기 모델 학습

### Vercel

용도:

- Homepage
- Compass
- Sentinel
- Foresight
- Lens
- lightweight API
- serverless frontend/backend boundary

주의:

- 오래 걸리는 작업은 Vercel function에 두지 말고 Openclaw worker/queue로 넘긴다.
- LLM streaming과 file processing은 timeout/cost 정책을 둔다.

### Supabase Admate-Vision

용도:

- 통합 DB
- Auth
- Storage
- RLS
- audit/operator/action log
- product schema 분리

권장 schema:

```text
openclaw
compass
foresight
sentinel_prelaunch
lens
```

### OpenRouter / 상업용 LLM

용도:

- multi-model routing
- provider fallback
- 모델 비교
- 제품별 budget cap

운영 원칙:

- Compass는 cheap/fast model 우선
- Sentinel은 알림 요약 cheap, 사고 분석 premium
- Lens는 텍스트 요약 중심 cheap
- Foresight는 분석/보고서 premium
- Creative Studio는 storyboard/script premium, 초안 cheap
- Hermes/Library는 후보 요약 cheap, 승인 판단 premium

## 3. 월 예산 초안

### MVP / 내부 테스트

```text
LLM credit: $100-$200 / month
GPU PoC: $50-$150 / month
Hetzner worker: EUR 10-EUR 50 / month
Supabase/Vercel: 현재 사용량 기준 유지 또는 소폭 증액
총 추가 예산: 약 20만-60만원 / month
```

### 100명 내부 사용 초기

```text
LLM credit: $300-$500 / month
GPU PoC/on-demand: $100-$300 / month
worker/monitoring/backup: EUR 50-EUR 150 / month
총 추가 예산: 약 70만-150만원 / month
```

### 본격 운영 / 보고서 / Creative / Slack Agent 확장

```text
LLM credit: $1,000+ / month
GPU: $300-$1,500+ / month
필요 시 direct provider 계약, invoice, internal cost center 운영
```

## 4. OpenRouter Credit 운영안

초기 충전:

```text
1차: $100
관찰 기간: 2-3주
이후 월 cap: $300
```

확장 조건:

- 제품별 token/cost log가 쌓였을 때
- 실패 요청 비용과 cache 절감액이 보일 때
- premium model 사용 이유가 feature별로 분리됐을 때

주의:

- credit 구매 수수료와 모델별 가격 변동을 고려한다.
- provider별 direct API가 더 유리한 경우는 별도 비교한다.
- 월 cap 없이 전 제품을 premium model로 열지 않는다.

## 5. 오픈 LLM / GPU 전략

초기에는 상시 GPU 서버를 운영하지 않는다.

권장 단계:

1. OpenRouter 또는 managed inference의 open model로 기능 검증
2. RunPod/Vast/Hetzner GPU 등 on-demand GPU로 단기 PoC
3. 내부 데이터/비용/지연시간 요구가 확인되면 dedicated GPU 검토
4. 월 비용이 꾸준히 높아지면 direct hosting 또는 GPU reservation 검토

상시 GPU 검토 조건:

```text
월 LLM 비용이 지속적으로 $1,000-$2,000 이상
내부 데이터가 외부 모델 전송에 부담
Slack/Agent 응답 지연을 직접 통제해야 함
반복 작업이 많아 small local model로 충분
야간 batch/Hermes 요약 작업이 많음
```

비권장:

- 첫날부터 H100급 GPU 상시 운영
- proof 없이 자체 모델 서버를 production dependency로 묶기
- 오픈 모델을 보안 검토 없이 raw campaign data와 연결

## 6. Cost Center 구조

AdMate는 처음부터 비용을 기능별로 나눠야 한다.

추적 단위:

```text
product
feature
user
campaign
model
provider
input_tokens
output_tokens
request_cost
success/failure
cached 여부
latency
trace_id
```

제품별 cost center:

```text
Compass
Sentinel
Lens
Foresight
AdMate Engine
Intelligence Library
Creative Studio
```

월간 보고 항목:

- 제품별 총 비용
- 모델별 총 비용
- 사용자별 과다 사용
- 실패 요청 비용
- cache 절감액
- premium model 사용 비율
- 이상 급증 알림

## 7. 운영 정책

초기 정책:

- product별 budget cap을 둔다.
- premium model은 feature flag로 제어한다.
- Slack agent는 high-cost action에 rate limit을 둔다.
- batch job은 야간 실행, cheap model 우선.
- raw campaign-level data는 LLM에 직접 전달하지 않는다.
- 외부 LLM 전달 데이터는 익명화/집계/요약한다.

권장 guardrail:

```text
cheap model default
premium model explicit reason
monthly cap
per-user/day cap
per-product cap
retry limit
cache first
audit log
```

## 8. 다음 Gate 후보

### Infra-Cost-1

현재 Vercel/Supabase/Hetzner/OpenRouter 사용량 inventory.

### Infra-Cost-2

AdMate cost_events table 또는 Openclaw cost logging schema 설계.

### Infra-Cost-3

제품별 LLM router policy 설계.

### Infra-Cost-4

Open model PoC 대상 선정.

### Infra-Cost-5

GPU on-demand PoC 비용/성능 benchmark.

## 9. 결론

AdMate는 지금 단계에서 고정 GPU 서버보다 하이브리드 과금 구조가 맞다.

```text
Hetzner는 worker와 orchestration.
Vercel은 제품 surface.
Supabase는 통합 데이터/권한.
OpenRouter는 초기 LLM routing.
GPU는 on-demand PoC.
상시 GPU는 비용과 사용량이 증명된 뒤.
```

첫 운영 예산은 OpenRouter $100-$300/month와 on-demand GPU $50-$150/month 수준에서 시작하고, 실제 사용량을 측정한 뒤 단계적으로 늘린다.
