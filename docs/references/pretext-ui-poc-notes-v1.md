# Pretext UI PoC Notes v1

작성일: 2026-05-07
문서 상태: 초안 v1
목적: Pretext.js를 AdMate core dependency로 바로 도입하지 않고, 긴 텍스트 UI의 layout stability 개선 후보로 검토하기 위한 PoC 기준을 정의한다.

---

## 1. Position

Pretext.js는 현재 AdMate 제품군에 바로 도입할 dependency가 아니다.

이 문서에서 Pretext는 다음으로 취급한다.

```text
긴 텍스트 UI의 layout shift와 overflow를 줄일 수 있는 실험 후보
```

최종 기준:

- production dependency 도입 전 별도 Gate로 검토한다.
- UI 안정성 개선 효과가 명확할 때만 제품별로 제한 적용한다.
- creative typography보다 운영 텍스트 안정성이 우선이다.

---

## 2. Candidate Surfaces

| Surface | 적용 아이디어 | Priority |
|---|---|---|
| Compass RAG answer/source | 긴 답변과 source excerpt 높이 예측, source card layout 안정화 | 중 |
| Intelligence Library/Wiki | 긴 한국어 summary, tags/chips와 본문 inline layout 안정화 | 중 |
| Sentinel alert log | 긴 alert reason/operator note가 포함된 virtualized log list | 낮음-중 |
| Foresight report comments | 개인화 보고서 코멘트 preview, 긴 문장 높이 예측 | 중 |
| Creative Studio | storyboard/presenter script text layout 실험 | 낮음, 실험용 |
| Homepage | hero 주변 creative text flow | 낮음 |

우선 PoC:

```text
Gate UI-Pretext-1
= Compass 또는 Intelligence Library의 긴 한국어 텍스트 card에서
  layout shift 감소와 모바일 text overflow 방지 효과만 검증한다.
```

---

## 3. Non-Goals

Pretext PoC에서 하지 않는다.

- 제품 로직 변경
- RAG/search/DB/API/auth 변경
- capture output 변경
- Sentinel critical alert 핵심 정보 영역 변경
- Command Center KPI layout 변경
- animation-heavy text effect 적용
- production dependency 즉시 추가

---

## 4. PoC Test Matrix

PoC는 최소 아래 케이스를 확인한다.

| Case | Check |
|---|---|
| 긴 한국어 정책 답변 | line count, card height, mobile overflow |
| 한국어/영어/숫자 혼합 source | token break, URL handling, badge collision |
| tags/chips + 본문 | inline wrapping, row height stability |
| streaming-like update | height jump 감소 여부 |
| fallback CSS only | Pretext 없이도 acceptable한지 비교 |
| browser support | Canvas/Intl.Segmenter availability |

---

## 5. Evaluation Criteria

채택 기준:

- mobile text overflow가 실제로 줄어든다.
- layout shift가 screenshot/measurement로 확인 가능하게 줄어든다.
- font stack, line-height, word-break 설정과 불일치가 없다.
- CSS line-clamp/overflow 전략보다 명확한 이점이 있다.
- bundle/runtime cost가 업무 UI에 비해 과하지 않다.
- failure fallback이 단순하다.

거절 기준:

- 계산 결과가 실제 렌더링과 자주 다르다.
- 한국어 `word-break: keep-all`과 충돌한다.
- source card나 alert log의 안정성보다 복잡도가 더 커진다.
- 제품 Agent가 core dependency를 넓은 범위에 추가해야 한다.

---

## 6. Product-Specific Guidance

### Compass

가장 적합한 후보. RAG answer/source card에서 긴 텍스트가 많고, source excerpt 높이 안정성이 중요하다.

금지:

- retrieval/generation logic 변경
- source ranking 변경
- confidence calculation 변경

### Intelligence Library

knowledge detail, candidate summary, source/provenance panel에서 검토 가능하다.

금지:

- approved/candidate state logic 변경
- reviewer/admin permission 변경

### Sentinel

alert log의 긴 reason에만 낮은 우선순위로 검토한다. critical action 영역에는 적용하지 않는다.

금지:

- critical alert visibility 변경
- operator action flow 변경
- monitoring/suppression logic 변경

### Foresight

report comment preview와 basis 설명에 제한적으로 검토한다.

금지:

- prediction model 변경
- raw campaign-level data 노출

### Lens

operator UI의 failure reason 정도에만 참고 가능하다. capture output에는 적용하지 않는다.

---

## 7. Recommended Prompt

제품 Agent에게 PoC를 지시할 때는 아래처럼 제한한다.

```text
Run a UI-only Pretext.js PoC for long Korean text layout stability.
Target only Compass source cards or Intelligence Library knowledge summaries.
Do not modify API, DB, auth, RAG logic, product logic, environment, or capture output.
Compare against CSS-only line-clamp/overflow behavior.
Report screenshot evidence, mobile overflow behavior, browser support, bundle/runtime risk, and rollback plan.
Do not add Pretext as a production dependency unless the Design Gate approves it.
```

---

## 8. Current Recommendation

Pretext.js는 바로 도입하지 않는다.

권장:

```text
Compass 또는 Intelligence Library에서 non-production UI PoC
→ CSS-only 전략과 비교
→ screenshot/measurement로 효과 확인
→ 제품별 제한 적용 여부 결정
```
