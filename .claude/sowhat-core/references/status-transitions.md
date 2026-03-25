# Status Transitions

Valid status values, transition rules, and cascading invalidation algorithm for sowhat sections.

## Status Values

| Status | Meaning | Can be changed to |
|--------|---------|-------------------|
| `draft` | Initial state. Structure exists but content not developed. | `discussing` (via expand) |
| `discussing` | Actively being developed via expand/debate. | `settled`, `needs-revision` |
| `settled` | Argument complete and confirmed. | `needs-revision`, `invalidated` (via challenge/revise) |
| `needs-revision` | Was settled but a problem was found. Must be reworked. | `discussing`, `settled` |
| `invalidated` | Upstream section changed; this section's argument is now invalid. | `discussing` (after upstream fixed) |

## Transition Rules

### Forward (progress)
- `draft` → `discussing`: expand command begins section development
- `discussing` → `settled`: settle command after validation passes

### Backward (degradation)
- Any status → `needs-revision`: challenge/revise finds a problem in this section directly
- Any status → `invalidated`: upstream section was revised, cascading effect
- `needs-revision` / `invalidated` → `discussing`: expand restarts development

### Gates
- `settled` requires: Claim + Grounds + Warrant + Qualifier + Rebuttal + Scheme all non-empty
- `finalize-planning` requires: all planning sections (01-03) settled
- `finalize` requires: all spec sections (04-09) settled

---

## Dependency Graph (섹션 간 참조 관계)

### 의존성 정의

섹션 B가 섹션 A에 **의존**한다는 것은 다음 중 하나를 의미한다:

1. **thesis_argument 의존**: B의 thesis_argument가 A의 Claim을 전제로 함
2. **Grounds 인용**: B의 Grounds가 A의 Claim 또는 Grounds를 직접 인용함
3. **Warrant 참조**: B의 Warrant가 A의 결론을 논리적 전제로 사용함
4. **암묵적 순서**: 번호 순서상 A < B이고, B가 A의 결론 위에 논증을 구축함

### 의존성 추출 알고리즘

```
FOR EACH section B:
  dependencies[B] = []

  1. B의 Grounds에서 다른 섹션을 명시적으로 언급하는가?
     - "01-problem에서 확인한 바와 같이..." → dependencies[B].append("01-problem")
     - 파일명, 섹션 번호, 섹션 이름 패턴 매칭

  2. B의 Warrant가 다른 섹션의 Claim을 전제하는가?
     - B.Warrant에 A.Claim의 핵심 키워드가 포함 → dependencies[B].append(A)

  3. B의 thesis_argument가 A의 thesis_argument와 논리적 선후 관계인가?
     - 같은 Key Argument를 지지하는 섹션들 중 번호가 앞선 것 → 잠재적 의존

  결과: dependencies[B] = [A1, A2, ...]  (B가 의존하는 섹션 목록)
```

### 의존성 그래프 예시

```
thesis (00)
  ├── 01-problem (thesis 의존)
  │     └── 02-solution (01 의존: "문제가 존재하므로 솔루션이 필요")
  │           └── 03-market (02 의존: "솔루션이 있으므로 시장이 존재")
  └── 04-actors (01, 02 의존 — spec layer)
```

---

## Cascading Invalidation Algorithm

### 트리거

섹션 A의 status가 `needs-revision` 또는 `invalidated`로 변경될 때 실행.

트리거 소스: challenge, revise, debate (broken), sync

### 알고리즘

```
FUNCTION cascade(section_A, reason):

  1. downstream = find_dependents(section_A)
     # section_A에 의존하는 모든 섹션 (직접 + 간접)

  2. IF downstream is empty:
     RETURN  # 전파 대상 없음

  3. 영향 범위 표시:
     **[decision]** 역전파 범위

     > 수정 대상: {section_A}
     > 이유: {reason}
     > 직접 영향: {direct_dependents 목록}
     > 간접 영향: {indirect_dependents 목록}

     [1] 전체 역전파 (위 섹션 모두 invalidated)
     [2] 직접 영향만 (간접은 유지)
     [3] 해당 섹션만 (역전파 없음)
     [4] 취소

  4. 인간의 선택에 따라:
     [1] → invalidate(direct + indirect)
     [2] → invalidate(direct only)
     [3] → 역전파 없음 (section_A만 변경)
     [4] → 전체 취소

  5. invalidate(sections):
     FOR EACH section IN sections:
       IF section.status IN [settled, discussing]:
         section.status = invalidated
         section.updated = current_datetime
         config.json 업데이트
         GitHub Issue reopen + label 변경
```

### find_dependents (의존 섹션 탐색)

```
FUNCTION find_dependents(section_A):
  direct = []
  indirect = []

  # 직접 의존: A를 직접 참조하는 섹션
  FOR EACH section B IN all_sections:
    IF A IN dependencies[B]:
      direct.append(B)

  # 간접 의존: 직접 의존 섹션에 다시 의존하는 섹션 (BFS)
  queue = copy(direct)
  WHILE queue is not empty:
    current = queue.pop()
    FOR EACH section C IN all_sections:
      IF current IN dependencies[C] AND C NOT IN direct AND C NOT IN indirect:
        indirect.append(C)
        queue.append(C)

  RETURN { direct, indirect }
```

### 순환 의존 방지

```
IF section A IN find_dependents(A):
  # 순환 감지
  ⚠️ 순환 의존 감지: {cycle_path}
  역전파를 중단하고 인간에게 보고
```

---

## 전파 순서

역전파 실행 시 반드시 **의존 방향 순서**로 실행한다:

```
직접 의존 → 간접 의존 (BFS 순서)

예: A가 변경되면
  1. B (A 직접 의존) invalidated
  2. C (B 직접 의존 = A 간접 의존) invalidated
  3. D (C 직접 의존 = A 간접 의존) invalidated
```

역순서로 실행하면 안 됨 (D를 먼저 invalidate하면 C 검사 시 D 상태가 이미 변경됨).

---

## config.json Tracking

```json
{
  "sections": {
    "01-problem": { "issue": 1, "status": "settled" },
    "02-solution": { "issue": 2, "status": "discussing" }
  }
}
```

섹션 파일 frontmatter의 status와 config.json의 status는 항상 동기화되어야 한다.
불일치 감지 시 `/sowhat:resume`의 Fallback Recovery가 수정을 제안한다 (`references/checkpoints.md` 참조).
