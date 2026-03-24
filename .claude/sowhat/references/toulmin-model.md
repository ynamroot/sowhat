# Toulmin Model — sowhat Field Definitions

All sowhat sections use the Toulmin Model with 6 mandatory fields.

## Fields

| Field | Korean | Definition | Example |
|-------|--------|------------|---------|
| **Claim** | 주장 | The assertion being made. What this section argues. | "이 시장은 연 30% 성장 중이다" |
| **Grounds** | 근거 | Factual evidence supporting the Claim. Data, observations, facts. | "2023 IDC 보고서: TAM $4.2B" |
| **Warrant** | 논리적 연결 | The reasoning that connects Grounds to Claim. Why the evidence matters. | "시장 성장률은 진입 타이밍의 적절성을 증명한다" |
| **Backing** | 보강 | Evidence supporting the Warrant itself. Meta-justification. | "McKinsey: 성장기 시장 진입이 성숙기보다 3x 생존율" |
| **Qualifier** | 한정어 | Confidence level. How certain is the Claim. | "probably" (2등급) |
| **Rebuttal** | 반박 조건 | Conditions under which the Claim would be false. | "단, 규제 환경이 급변하지 않는 한" |

## Qualifier Scale (5-level)

Used in both expand and challenge/debate to express argument strength:

| Level | Korean | English | Meaning |
|-------|--------|---------|---------|
| 0 | 확실히 | definitely | No meaningful counterargument exists |
| 1 | 아마도 | probably | Strong evidence, minor exceptions possible |
| 2 | 추정컨대 | presumably | Reasonable inference, moderate uncertainty |
| 3 | 그럴 수 있다 | possibly | Plausible but significant uncertainty |
| 4 | 어쩌면 | conceivably | Speculative, weak evidence |

## Settled Criteria

A section is eligible for `/sowhat:settle` when ALL of the following are true:
- Claim: non-empty, clear assertion
- Grounds: at least 1 concrete evidence item
- Warrant: explicit reasoning connecting Grounds → Claim
- Qualifier: set to a specific level (not blank)
- Rebuttal: at least 1 condition identified
- Backing: optional but recommended

## MECE Check

Key Arguments in thesis should be:
- **Mutually Exclusive**: no overlap between sections
- **Collectively Exhaustive**: together they fully support the thesis Answer
