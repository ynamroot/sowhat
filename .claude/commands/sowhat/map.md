---
name: sowhat:map
description: 논증 흐름을 Mermaid 다이어그램으로 즉시 시각화한다. --export로 ARGUMENT-MAP.md 정식 산출물 생성. "논증 맵", "시각화", "map", "다이어그램", "구조 보기", "흐름 보여줘", "어떻게 연결돼", "트리 그려줘", "argument map 생성", "논증 구조 저장" 등 현재 논증 구조를 한눈에 확인하거나 export할 때 사용.
argument-hint: "[section] [--save] [--export]"
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
---
<objective>
config.json과 모든 섹션 파일을 읽어 논증 트리의 Mermaid 다이어그램을 인라인으로 즉시 출력한다. --export 시 export/ARGUMENT-MAP.md로 Toulmin 전체 구조 스냅샷을 정식 산출물로 생성한다.
</objective>

<execution_context>
@.claude/sowhat-core/workflows/map.md
@.claude/sowhat-core/references/session-protocol.md
</execution_context>

<context>
Arguments: $ARGUMENTS
</context>

<process>
Execute the map workflow end-to-end.
Preserve all workflow gates.
</process>
