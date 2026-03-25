---
name: sowhat:resume
description: 중단된 작업 세션을 재개하고 컨텍스트를 복원한다. "계속", "이어서", "세션 재개", "어디까지 했지", "작업 복원", "resume" 등 이전 작업을 이어서 하고 싶을 때 사용. session.md, git log, 미완료 섹션, 활성 debate 브랜치를 자동 감지해 재진입 경로를 제시한다.
argument-hint: "(no arguments)"
allowed-tools:
  - Read
  - Bash
  - Glob
  - Grep
---
<objective>
logs/session.md, git log, 섹션 상태, debate 브랜치를 분석하여 중단된 작업 컨텍스트를 복원하고 다음 실행할 커맨드를 제시한다.
</objective>

<execution_context>
@.claude/sowhat-core/references/ux-standards.md
@.claude/sowhat-core/workflows/resume.md
@.claude/sowhat-core/references/session-protocol.md
</execution_context>

<context>
Arguments: $ARGUMENTS
</context>

<process>
Execute the resume workflow end-to-end.
Preserve all workflow gates.
</process>
