# Continuation Format

Standard format for presenting next steps after any sowhat command completes.

## Core Structure

```
---

## ▶ 다음

**{section or command}: {name}** — {one-line description}

`{exact command to copy-paste}`

<sub>`/clear` 후 실행 → 컨텍스트 초기화</sub>

---

**또한 가능:**
- `{alternative 1}` — 설명
- `{alternative 2}` — 설명

---
```

## Rules

1. Always show what it is — section name + 1-line description
2. Command in inline backticks — easy to copy-paste
3. Include `/clear` note — always
4. Use "또한 가능:" not "다른 옵션"
5. `---` separators above and below

## Variants

### After expand (section just developed):
```
---

## ▶ 다음

**{N}-{section}: {section name}** — 논증 검토 후 확정

`/sowhat:settle {section}`

<sub>`/clear` 후 실행</sub>

---

**또한 가능:**
- `/sowhat:debate {section}` — 변증법 검증으로 논거 강화
- `/sowhat:challenge` — 전체 트리 검증

---
```

### After settle (section settled):
```
---

## ▶ 다음

**{next-section}: {name}** — 다음 섹션 전개

`/sowhat:expand {next-section}`

<sub>`/clear` 후 실행</sub>

---

**또한 가능:**
- `/sowhat:progress` — 전체 현황 확인
- `/sowhat:challenge` — 전체 트리 검증 (모든 섹션 settled 후)

---
```

### After challenge (issues found):
```
---

## 🔴 {N}건 발견

[1] {section} — {issue summary}
[2] {section} — {issue summary}

`/sowhat:revise {section}` 또는 `/sowhat:expand {section}`

<sub>`/clear` 후 실행</sub>

---
```

### After challenge (no issues):
```
---

## ✅ Challenge 통과

모든 섹션 검증 완료

`/sowhat:finalize`

<sub>`/clear` 후 실행</sub>

---
```
