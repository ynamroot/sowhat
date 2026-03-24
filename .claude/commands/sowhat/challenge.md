---
name: sowhat:challenge
description: 논증 트리를 7단계 논리 검증으로 공격한다(Toulmin + Walton + Pragma-Dialectics). 섹션 지정 시 해당 섹션만 부분 검증, 미지정 시 전체 트리 검증. "전체 검증", "논리 점검", "challenge", "논증 공격", "최종 검토", "품질 검사" 등 settled 전이나 finalize 전 최종 검증 시 사용하라.
argument-hint: "[<section>] [--force]"
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - Task
---
<objective>
전체 논증 트리를 7단계(MECE→Toulmin→Walton→Pragma→Qualify→Scope→Synthesis) 검증으로 공격한다. 각 스테이지를 sowhat-challenge-agent Task로 순차 실행한다.
</objective>

<execution_context>
@.claude/sowhat-core/workflows/challenge.md
@.claude/sowhat-core/references/session-protocol.md
@.claude/sowhat-core/references/continuation-format.md
@.claude/sowhat-core/references/toulmin-model.md
@.claude/sowhat-core/references/challenge-algorithm.md
@.claude/sowhat-core/references/checkpoints.md
@.claude/sowhat-core/references/source-credibility.md
</execution_context>

<context>
Arguments: $ARGUMENTS
</context>

<process>
Execute the challenge workflow end-to-end.
Preserve all workflow gates.
</process>
