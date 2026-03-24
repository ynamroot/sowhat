---
name: sowhat:progress
description: 현재 프로젝트 상태를 대시보드로 보여주고 다음 액션을 안내한다. "진행 상황", "현재 상태", "어디까지 했어", "다음 뭐 해", "progress", "현황 확인", "상태 보기", "얼마나 됐어" 등 프로젝트 전체 현황이 궁금할 때 사용.
argument-hint: "(no arguments)"
allowed-tools:
  - Read
  - Bash
  - Glob
  - Grep
---
<objective>
config.json과 모든 섹션 파일을 읽어 전체 진행 상황 대시보드를 출력하고, 현재 상태에 맞는 다음 액션을 안내한다.
</objective>

<execution_context>
@C:/Users/Owner/.claude/sowhat-core/workflows/progress.md
@C:/Users/Owner/.claude/sowhat-core/references/session-protocol.md
</execution_context>

<context>
Arguments: $ARGUMENTS
</context>

<process>
Execute the progress workflow end-to-end.
Preserve all workflow gates.
</process>
