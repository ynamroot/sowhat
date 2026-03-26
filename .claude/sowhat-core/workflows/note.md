# /sowhat:note — 제로 마찰 아이디어 캡처

<!--
@metadata
checkpoints: []
config_reads: [project]
config_writes: []
continuation:
  primary: "(이전 작업으로 복귀)"
  alternatives: ["/sowhat:progress"]
status_transitions: []
-->

작업 흐름을 끊지 않고 아이디어를 즉시 캡처한다. expand/debate 같은 집중 워크플로 중에도 사용 가능.

## 인자 파싱

```
/sowhat:note <text>            → 노트 추가 (append)
/sowhat:note list              → 노트 목록 확인
/sowhat:note promote <N> <section>  → N번 노트를 섹션의 Open Questions에 승격
/sowhat:note done <N>          → N번 노트를 완료 처리
/sowhat:note clear             → 완료된 노트 정리
```

---

## Append 모드 (기본)

`$ARGUMENTS`가 `list`, `promote`, `done`, `clear`가 아닌 텍스트이면 append 모드.

### 1. 프로젝트 확인

`planning/config.json` 로드.
- 파일 없으면: `❌ sowhat 프로젝트가 아닙니다.`

### 2. datetime 취득

```bash
date -u +"%Y-%m-%dT%H:%M:%SZ"
```

### 3. 노트 파일에 추가

`logs/notes.md` 파일에 append한다 (없으면 생성).

파일 형식:
```markdown
# Notes

- [ ] [1] {text} ({YYYY-MM-DD})
- [ ] [2] {text} ({YYYY-MM-DD})
- [x] [3] {text} ({YYYY-MM-DD}) → done
```

새 노트 추가 시:
1. 기존 노트의 마지막 번호를 파악
2. 다음 번호로 `- [ ] [{N}] {text} ({YYYY-MM-DD})` 추가

### 4. 확인 출력 (최소한으로)

```
📝 #{N}: {text}
```

한 줄만 출력하고 종료. 이전 작업으로 즉시 복귀할 수 있도록 최소 출력.

---

## List 모드

```
/sowhat:note list
```

`logs/notes.md`를 읽고 모든 노트를 출력한다.

```
----------------------------------------
📝 노트 ({미처리}건 미처리 / {전체}건 전체)

  [ ] [1] 05에서 규제 리스크 다룰 것 (2026-03-25)
  [ ] [2] market 섹션에 동남아 데이터 추가 (2026-03-24)
  [x] [3] draft할 때 투자자 버전 따로 (2026-03-23) → done

다음:
  [1] /sowhat:note promote <N> <section>  — Open Question으로 승격
  [2] /sowhat:note done <N>               — 완료 처리
  [3] /sowhat:note clear                  — 완료된 노트 정리
----------------------------------------
```

파일 없으면: `📝 노트가 없습니다.`

---

## Promote 모드

```
/sowhat:note promote <N> <section>
```

N번 노트를 지정 섹션의 `## Open Questions`에 추가한다.

### 1. 노트 확인

`logs/notes.md`에서 N번 노트를 찾는다.
- 없으면: `❌ #{N} 노트를 찾을 수 없습니다.`
- 이미 done이면: `❌ #{N}은 이미 완료된 노트입니다.`

### 2. 섹션 확인

대상 섹션 파일을 확인한다.
- 숫자면 → `{N}-*.md` 패턴
- 이름이면 → `*-{name}.md` 패턴
- 없으면 → `❌ 섹션을 찾을 수 없습니다: {section}`

### 3. Open Questions에 추가

섹션 파일의 `## Open Questions` 섹션에 체크박스 항목으로 추가:

```markdown
- [ ] {note text} (note #{N}에서 승격)
```

### 4. 노트 완료 처리

`logs/notes.md`에서 해당 노트를 `[x]`로 마킹하고 `→ promoted to {section}`을 추가.

### 5. 커밋

```bash
git add logs/notes.md planning/{section}.md
git commit -m "note: promote #{N} to {section} Open Questions"
```

### 6. 출력

```
✅ #{N} → {section} Open Questions에 추가됨
```

---

## Done 모드

```
/sowhat:note done <N>
```

N번 노트를 완료 처리한다.

1. `logs/notes.md`에서 `- [ ] [{N}]`을 `- [x] [{N}]`으로 변경하고 `→ done` 추가
2. 출력: `✅ #{N} done`

---

## Clear 모드

```
/sowhat:note clear
```

완료된 노트(`[x]`)를 제거한다.

1. `logs/notes.md`에서 `- [x]` 행을 모두 삭제
2. 남은 노트 번호를 1부터 재번호 부여
3. 출력: `✅ {N}건 정리됨, {M}건 남음`

---

## 엣지 케이스

- `logs/notes.md` 없음: append 시 자동 생성, list/promote/done/clear 시 "노트가 없습니다"
- 인자 없음: `❌ 노트 내용을 입력하세요. 예: /sowhat:note 05에서 규제 리스크 다룰 것`
- promote 시 섹션에 `## Open Questions`가 없으면: 섹션 파일 끝에 추가

---

## 핵심 원칙

- **최소 출력** — append는 한 줄만. 작업 흐름 방해 금지
- **번호 기반** — 모든 노트는 고유 번호로 관리
- **promote는 추적 가능** — 어떤 노트에서 왔는지 Open Question에 기록
- **session.md 저장 안 함** — read-only 성격의 가벼운 커맨드
