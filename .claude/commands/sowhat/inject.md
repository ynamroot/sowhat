---
name: sowhat:inject
description: 외부 자료를 특정 섹션의 특정 Toulmin 필드에 직접 주입한다. "증거 추가", "자료 주입", "inject", "URL 넣기", "파일 넣기", "데이터 추가", "근거 추가", "backing 추가" 등 사용자가 가진 자료를 논증에 직접 반영할 때 사용.
argument-hint: "<section> [<url>|file:<path>]"
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - WebFetch
---
<objective>
외부 자료(URL, 로컬 파일, 직접 텍스트)를 분석하고, Tier 판정 후, 사용자가 지정한 섹션의 Toulmin 필드에 직접 주입한다. 파인딩 파일을 생성하여 출처를 추적한다.
</objective>

<execution_context>
@.claude/sowhat-core/workflows/inject.md
@.claude/sowhat-core/references/source-credibility.md
@.claude/sowhat-core/references/strength-scoring.md
@.claude/sowhat-core/references/session-protocol.md
@.claude/sowhat-core/references/continuation-format.md
@.claude/sowhat-core/references/toulmin-model.md
</execution_context>

<context>
Arguments: $ARGUMENTS
</context>

<process>
Execute the inject workflow end-to-end.
Preserve all workflow gates.
</process>
