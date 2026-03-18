---
model: claude-haiku-4-5-20251001
---
# /sowhat:map — 논증 트리 시각화

논증 구조를 Excalidraw JSON 파일로 시각화한다. `$ARGUMENTS`에 따라 전체 트리(global) 또는 특정 섹션의 로컬 뷰를 생성한다. 생성된 파일은 Obsidian Excalidraw 플러그인에서 열 수 있다.

## 인자 파싱

```
/sowhat:map [section] [--snapshot] [--local | --global]
```

| 인자 | 의미 |
|------|------|
| 인자 없음 또는 `--global` | 전체 논증 트리 |
| `{section}` (번호 또는 이름) | 해당 섹션 로컬 뷰 |
| `--snapshot` | 현재 상태를 스냅샷으로도 저장 |

모드 결정: `--global` > `{section}` 존재 > global

## 사전 준비

1. `planning/config.json` 로드
2. `00-thesis.md` 로드
3. 디렉터리 생성:
   ```bash
   mkdir -p maps/snapshots maps/local maps/debate
   ```
4. 현재 datetime:
   ```bash
   date -u +"%Y-%m-%dT%H:%M:%SZ"
   ```

---

## Excalidraw JSON 기본 구조

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [...],
  "appState": {
    "gridSize": null,
    "viewBackgroundColor": "#f8f9fa"
  },
  "files": {}
}
```

---

## 카드 설계 — 최적화 버전

### 섹션 카드 (width: 200, height: 140)

카드는 **rectangle 1개 + text 1개**로 구성한다. **세로 확장에 최적화.**

**rectangle:**
```json
{
  "id": "rect-{section}",
  "type": "rectangle",
  "x": {계산값},
  "y": {계산값},
  "width": 200,
  "height": 140,
  "strokeColor": "#495057",
  "backgroundColor": "{status 색상}",
  "fillStyle": "solid",
  "strokeWidth": 1,
  "roughness": 0,
  "roundness": {"type": 3},
  "link": "[[{섹션 파일명 확장자 없이}]]"
}
```

**text (카드 내부, 절대좌표로 배치):**
```json
{
  "id": "text-{section}",
  "type": "text",
  "x": {rect.x + 6},
  "y": {rect.y + 4},
  "width": 188,
  "height": 132,
  "text": "{N} {name}\n\n{scheme} {qualifier}\n────\nClaim:\n{claim 첫 2줄 또는 35자}\n\nWarrant:\n{warrant 첫 1줄 또는 20자}\n{rebuttal 있으면}\nRebuttal:\n{rebuttal 첫 1줄 또는 20자}",
  "fontSize": 11,
  "fontFamily": 1,
  "textAlign": "left",
  "verticalAlign": "top",
  "strokeColor": "#212529"
}
```

**텍스트 콘텐츠 원칙:**
- 첫 2줄까지 보여주거나, 실제 문장 끝까지 표시
- 50자 제한 제거 → **맥락 있는 문장 우선**
- 비어있는 필드는 생략 (표시 안 함)
- 줄 바꿈은 자연스럽게 (단어 경계에서만)

### Thesis 카드 (width: 240, height: 70)

```
text: "Thesis\n{Answer 전체 내용}"
width: 240, height: 70
link: "[[00-thesis]]"
backgroundColor: "#91caff"
fontSize: 11
```

### Key Argument 카드 (width: 200, height: 50)

```
text: "{Key Argument 한 줄}"
width: 200, height: 50
backgroundColor: "#d3adf7"
fontSize: 11
```

### 노드 색상 (status별)

| status | backgroundColor |
|--------|----------------|
| `settled` | `#b7eb8f` |
| `discussing` | `#ffe58f` |
| `draft` | `#f0f0f0` |
| `needs-revision` | `#ffccc7` |
| `invalidated` | `#d9d9d9` |

---

## Global 모드 — 세로 트리 (개선됨)

### 1. 데이터 수집

- `00-thesis.md`: Answer, Key Arguments 목록
- 각 섹션 파일: `status`, `scheme`, `qualifier`, Claim 내용, Warrant 내용, Rebuttal 내용, `thesis_argument`

### 2. 레이아웃 계산 (위→아래, Key Arg별 세로 그룹핑)

```
구조:
  [Thesis 중앙]
       ↓
  [Key Arg 1]  [Key Arg 2]  [Key Arg 3]
       ↓            ↓            ↓
  [1-*]        [2-*]        [3-*]
  [1-*]        [2-*]        [3-*]

파라미터:
  Thesis 카드: width=240, height=70
  Key Arg 카드: width=200, height=50
  섹션 카드: width=200, height=140

  세로 간격(y): 20 (카드 아래끝 ~ 다음 카드 위끝)
  가로 간격(x): 40 (열 사이)

레이아웃 절차:
  1. Key Argument 개수 = K
  2. 각 Key Arg 내 최대 섹션 수 = max_sections
  3. 각 열의 높이 = 50 + (max_sections × 140) + ((max_sections-1) × 20) + 20 (아래 여유)

  x 좌표 계산:
    전체 폭 = K × 200 + (K-1) × 40 + 40 (좌우 여유)
    캔버스 중앙 = 400
    시작 x = 400 - (전체 폭 / 2)

    각 Key Arg x = 시작 x + (i × (200 + 40)) (i: 0부터)

  y 좌표 계산:
    Thesis y = 30
    첫 Key Arg y = Thesis.y + 70 + 20 = 120

    각 Key Arg 내 첫 섹션 y = Key Arg.y + 50 + 20 = Key Arg.y + 70
    각 섹션 y = 이전_섹션_y + 140 + 20 (Key Arg 열 내에서 순차)
```

### 3. 화살표 연결

각 화살표:
```json
{
  "id": "arrow-{from}-{to}",
  "type": "arrow",
  "strokeColor": "#868e96",
  "strokeWidth": 1,
  "roughness": 0,
  "startBinding": {"elementId": "{from rect id}", "focus": 0, "gap": 4},
  "endBinding": {"elementId": "{to rect id}", "focus": 0, "gap": 4},
  "endArrowhead": "arrow",
  "startArrowhead": null
}
```

연결:
- Thesis → 각 Key Arg (아래로, 수직선)
- 각 Key Arg → 자신의 섹션들 (아래로, 수직선)

화살표 points 계산:
```
Thesis → Key Arg:
  시작점: (Thesis.x + 120, Thesis.y + 70)
  끝점:   (KeyArg.x + 100, KeyArg.y)
  points: [[0,0], [끝x-시작x, 끝y-시작y]]

Key Arg → 첫 섹션:
  시작점: (KeyArg.x + 100, KeyArg.y + 50)
  끝점:   (Section.x + 100, Section.y)
  points: [[0,0], [끝x-시작x, 끝y-시작y]]

같은 열 내 섹션들:
  이전 섹션 아래에서 다음 섹션 위로 수직선
  시작점: (Section.x + 100, Section.y + 140)
  끝점:   (NextSection.x + 100, NextSection.y)
```

### 4. 파일 저장

Write 도구로 `maps/overview.excalidraw` 저장.

`--snapshot` 플래그 있으면 동일 내용을 `maps/snapshots/overview-{YYYYMMDD-HHMM}.excalidraw`에도 저장.

### 5. 출력

```
✅ maps/overview.excalidraw ({settled}/{total} settled)
```

needs-revision이 있으면 한 줄 추가:
```
⚠️  needs-revision: {섹션 번호 목록}
```

---

## Local 모드 — 섹션 상세 뷰

특정 섹션의 Toulmin 구조를 더 넓게 시각화한다.

### 1. 섹션 파일 확인

- 숫자 → `{N}-*.md` 패턴 검색
- 이름 → `*-{name}.md` 패턴 검색
- 없으면 → `❌ 섹션을 찾을 수 없습니다: {section}`

### 2. 데이터 수집

- `00-thesis.md`: Answer, 해당 섹션의 Key Argument
- 현재 섹션: 전체 Toulmin 필드 (Claim, Grounds, Warrant, Backing, Qualifier, Rebuttal, Open Questions)

### 3. 레이아웃 (가로 트리, 더 넓은 카드)

```
[Thesis]──→[Key Arg]──→[Claim 카드]──→[Warrant 카드]
                                   ↗
                         [Grounds 카드]
                                        ↘
                                    [Rebuttal 카드]

오른쪽 사이드패널:
  [Backing]
  [Qualifier]
  [Open Questions]
```

카드 너비: 300px (내용을 더 길게)

각 카드에 `link: "[[{section}]]"` 포함.

### 4. 파일 저장

`maps/local/{section-name}.excalidraw`

### 5. 출력

```
✅ maps/local/{section-name}.excalidraw
```

---

## Debate 맵 생성 (자동 호출)

`/sowhat:debate` 라운드 완료 시 자동 호출.

저장 경로: `maps/debate/debate-{section}-r{N}.excalidraw`

이전 라운드 상태는 excalidraw 파일을 읽지 않고 git으로 가져온다:
```bash
git show HEAD~1:{section-file}.md
```
이전 커밋의 섹션 파일에서 Claim/Warrant/Rebuttal을 추출하여 왼쪽 카드(회색)로 표시.
현재 섹션 파일에서 추출한 내용을 오른쪽 카드(파랑)로 표시.
각 카드에 `link: "[[{section}]]"` 포함.

---

## 파일 명명 규칙

| 종류 | 경로 |
|------|------|
| 전체 개요 | `maps/overview.excalidraw` |
| 전체 스냅샷 | `maps/snapshots/overview-{YYYYMMDD-HHMM}.excalidraw` |
| 로컬 뷰 | `maps/local/{section-name}.excalidraw` |
| Debate 뷰 | `maps/debate/debate-{section}-r{N}.excalidraw` |

---

## 핵심 원칙 (개선됨)

- **세로 트리 (위→아래)** — Thesis 최상단, Key Arguments 2단계, 각 열에서 섹션 세로 스택
- **최소한의 신뢰** — 카드는 Claim의 처음 2줄 + Warrant의 첫 1줄을 보여주되, 전체 문맥을 link로 연결
- **카드 크기 최적화** — 섹션 200x140, Key Arg 200x50, Thesis 240x70 (세로 확장에 최적)
- **맥락 우선, 자르지 않음** — 50자 제한 폐기, 실제 문장 끝까지 표시하되 자연스럽게 줄바꿈
- **wiki 링크** — 모든 카드에 `link: "[[파일명]]"` 설정, 클릭 시 해당 md 파일로 이동
- **구조가 아닌 논리 강조** — 파일 구조를 보여주는 것이 아닌, 주장·근거·반박 같은 논증 요소를 구조화하여 맥락 전달
- **MCP 미사용** — Claude가 JSON 직접 생성, Write 도구로 저장
- **Global은 항상 덮어쓴다** — 최신 상태 유지
- **스냅샷은 누적** — 삭제하지 않는다
