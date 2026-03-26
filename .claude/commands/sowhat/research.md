---
name: sowhat:research
description: 외부 리서치를 수행하고 결과를 섹션에 반영한다. "리서치", "데이터 조사", "근거 수집", "URL 분석", "파일 분석", "폴더 분석", "시장 조사", "외부 자료 찾기", "open question 해결", "딥 리서치", "deep research", "심층 조사" 등 논증에 필요한 외부 근거나 데이터가 필요할 때 사용. URL 직접 분석, 로컬 파일/폴더 분석, 토픽 검색, 자율 리서치 모드 지원. --deep 플래그로 Perplexity Deep Research 활성화 (PERPLEXITY_API_KEY 필요, 선택적).
argument-hint: "[--deep] [<url>|file:<path>|dir:<path> [--glob <pattern>]|<topic>|review|review <section>|accept <N>|reject <N>]"
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
  - WebSearch
  - WebFetch
---
<objective>
URL 분석, 로컬 파일/폴더 분석, 토픽 검색, 자율 리서치 모드로 외부 근거를 수집하고 research/ 파인딩 파일로 저장한다. 파일/폴더 모드는 로컬 자료를 분석하여 섹션별 제안을 생성한다. 결정은 항상 인간이 accept/reject한다.
</objective>

<execution_context>
@.claude/sowhat-core/references/ux-standards.md
@.claude/sowhat-core/workflows/research.md
@.claude/sowhat-core/references/session-protocol.md
@.claude/sowhat-core/references/continuation-format.md
@.claude/sowhat-core/references/toulmin-model.md
@.claude/sowhat-core/references/source-credibility.md
</execution_context>

<context>
Arguments: $ARGUMENTS
</context>

<process>
CRITICAL: Do NOT use AskUserQuestion tool. Present choices as text, then wait for user free-text input.
CRITICAL: Choices must be numbered [1] [2] [3] — NEVER use A/B/C/D. NEVER use tables for choices. Follow workflow templates exactly as written.
Execute the research workflow end-to-end.
Preserve all workflow gates.
</process>
