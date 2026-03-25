# Continuation Format

Standard format for presenting next steps after any sowhat command completes.

## Core Structure

```
----------------------------------------
다음 액션:

[1] {description} ({command hint})
[2] {description} ({command hint})
[3] {description} ({command hint})
----------------------------------------
```

## Rules

1. 번호 선택지로 제시 — 유저가 커맨드를 타이핑할 필요 없음
2. 괄호 안에 커맨드 힌트 — 유저가 원하면 직접 실행 가능
3. 유저가 번호를 입력하면 Claude가 해당 커맨드 실행
4. 구분선은 `----------------------------------------` (40자)
5. `<sub>` 태그, `## ▶ 다음`, `**또한 가능:**` 패턴 금지

## Variants

### After expand (section just developed):
```
----------------------------------------
다음 액션:

[1] 논증 검토 후 확정 (settle {section})
[2] 변증법 검증으로 강화 (debate {section})
[3] 전체 트리 검증 (challenge)
----------------------------------------
```

### After settle (section settled):
```
----------------------------------------
다음 액션:

[1] 다음 섹션 전개 (expand {next-section})
[2] 전체 현황 확인 (progress)
[3] 전체 트리 검증 (challenge) — 모든 섹션 settled 후
----------------------------------------
```

### After challenge (issues found):
```
----------------------------------------
다음 액션:

[1] {section} 수정 (revise {section}) — {issue summary}
[2] {section} 재전개 (expand {section}) — {issue summary}
----------------------------------------
```

### After challenge (no issues):
```
----------------------------------------
다음 액션:

[1] 최종 완료 (finalize)
[2] 문서 생성 (draft)
[3] 최강 반론 테스트 (steelman)
----------------------------------------
```

### After inject:
```
----------------------------------------
다음 액션:

[1] 주입 반영 후 전개 계속 (expand {section})
[2] 논증 확정 (settle {section})
[3] 주입 후 부분 검증 (challenge {section})
----------------------------------------
```

### After autonomous:
```
----------------------------------------
다음 액션:

[1] 문서 생성 (draft)
[2] 최강 반론 테스트 (steelman)
[3] 전체 상태 확인 (progress)
----------------------------------------
```

### After steelman:
```
----------------------------------------
다음 액션:

[1] 가장 약한 섹션 수정 (revise {weakest_section})
[2] 변증법 강화 (debate {section})
[3] 근거 보강 (inject {section})
----------------------------------------
```

### After branch create:
```
----------------------------------------
다음 액션:

[1] 대안 논증 전개 (expand {section})
[2] 분기 비교 (branch compare {section})
----------------------------------------
```

### After branch merge:
```
----------------------------------------
다음 액션:

[1] 채택된 논증 확정 (settle {section})
[2] 전체 상태 확인 (progress)
----------------------------------------
```
