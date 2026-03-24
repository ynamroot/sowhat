# Status Transitions

Valid status values and transition rules for sowhat sections.

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
- Any status → `needs-revision`: challenge/revise finds a problem
- Any status → `invalidated`: upstream section was revised, cascading effect
- `needs-revision` / `invalidated` → `discussing`: expand restarts development

### Gates
- `settled` requires: Claim + Grounds + Warrant + Qualifier + Rebuttal all non-empty
- `finalize-planning` requires: all planning sections (01-03) settled
- `finalize` requires: all spec sections (04-09) settled

## Propagation (Cascading Invalidation)

When section N is degraded to `needs-revision` or `invalidated`:
1. Check all downstream sections that depend on section N
2. If any downstream sections are `settled` or `discussing`:
   - Show confirmation gate: "[1] 자동 invalidate [2] 건너뜀"
   - On [1]: change downstream sections to `invalidated`
3. Update config.json for all affected sections
4. Append to argument-log.md

## config.json Tracking

```json
{
  "sections": {
    "01-problem": { "status": "settled", "file": "planning/01-problem.md" },
    "02-solution": { "status": "discussing", "file": "planning/02-solution.md" }
  }
}
```
