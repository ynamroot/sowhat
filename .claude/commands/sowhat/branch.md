---
name: sowhat:branch
description: 섹션의 대안적 논증 경로를 생성하고 비교한다. "분기", "branch", "대안", "비교", "다른 방향", "A vs B", "두 가지 방향" 등 하나의 섹션에서 여러 논증 방향을 탐색하고 싶을 때 사용.
argument-hint: "[<section>] | [compare <section>] | [merge <section> <branch>] | [delete <section> <branch>]"
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
---
<objective>
지정된 섹션에서 대안적 논증 경로(branch)를 생성·비교·병합·삭제한다. 현재 논증을 보존하면서 다른 방향을 탐색할 수 있게 한다.
</objective>

<execution_context>
@.claude/sowhat-core/references/ux-standards.md
@.claude/sowhat-core/workflows/branch.md
@.claude/sowhat-core/references/strength-scoring.md
@.claude/sowhat-core/references/toulmin-model.md
@.claude/sowhat-core/references/session-protocol.md
@.claude/sowhat-core/references/continuation-format.md
</execution_context>

<context>
Arguments: $ARGUMENTS
</context>

<process>
Execute the branch workflow end-to-end.
Preserve all workflow gates and checkpoints.
</process>
