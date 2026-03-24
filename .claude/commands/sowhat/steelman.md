---
name: sowhat:steelman
description: 현재 논증의 최강 반대 논증 트리를 자동 생성한다. "steelman", "반대 논증", "counter-narrative", "최강 반론", "반대 입장", "스트레스 테스트" 등 논증의 근본적 강도를 시험할 때 사용.
argument-hint: "[--section <section>]"
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - WebSearch
  - WebFetch
  - Task
---
<objective>
현재 thesis에 대한 최강 반대 논증 트리(Anti-Thesis + 섹션별 Counter-Argument)를 자동 생성하고, 원본과 비교하여 취약점을 식별한다.
</objective>

<execution_context>
@.claude/sowhat-core/workflows/steelman.md
@.claude/sowhat-core/references/toulmin-model.md
@.claude/sowhat-core/references/strength-scoring.md
@.claude/sowhat-core/references/source-credibility.md
@.claude/sowhat-core/references/challenge-algorithm.md
</execution_context>

<context>
Arguments: $ARGUMENTS
</context>

<process>
Execute the steelman workflow end-to-end.
Generate Anti-Thesis, section-level Counter-Arguments, compare original vs counter, and produce STEELMAN-REPORT.md.
</process>
