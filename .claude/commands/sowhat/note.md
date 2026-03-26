---
name: sowhat:note
description: 작업 중 아이디어를 즉시 메모한다. append로 추가, list로 목록 확인, promote로 섹션 Open Question에 승격. "메모", "note", "아이디어", "나중에", "잊지 말고", "기록" 등 작업 흐름을 끊지 않고 아이디어를 캡처할 때 사용.
argument-hint: "[<text>|list|promote <N> <section>]"
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
---
<objective>
작업 흐름을 끊지 않고 아이디어를 즉시 캡처하거나, 누적된 노트를 관리한다.
</objective>

<execution_context>
@.claude/sowhat-core/references/ux-standards.md
@.claude/sowhat-core/workflows/note.md
</execution_context>

<context>
Arguments: $ARGUMENTS
</context>

<process>
CRITICAL: Do NOT use AskUserQuestion tool. Present choices as text, then wait for user free-text input.
CRITICAL: Choices must be numbered [1] [2] [3] — NEVER use A/B/C/D. NEVER use tables for choices. Follow workflow templates exactly as written.
Execute the note workflow end-to-end.
</process>
</output>
