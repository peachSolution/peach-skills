---
name: peach-release
description: |
  peach-harness 버전 업데이트 → develop 커밋/푸시 → main PR 생성 → PR 머지까지 일괄 처리하는 릴리스 스킬.
  semver 기준(patch/minor/major) 자동 판단 또는 사용자 지정.
  "릴리스", "버전 업", "release", "main 머지", "배포 준비" 키워드로 트리거.
  peach-harness 저장소에서만 사용한다.
allowed-tools:
  - Bash
  - Read
  - Edit
---

# peach-release — 릴리스 일괄 처리

peach-harness 저장소의 릴리스를 한 번에 처리한다.
두 버전 파일 동기화 → develop 커밋/푸시 → main PR 생성 → 머지까지 자동화한다.

## 전제조건

- **peach-harness 저장소 루트**에서 실행
- `develop` 브랜치에 체크아웃된 상태
- `gh` CLI 인증 완료

---

## Workflow

### 1단계: 상태 확인

```bash
git status && git branch && git log --oneline -5
```

- develop 브랜치인지 확인한다. 아니면 중단하고 사용자에게 알린다.
- 미스테이지 변경사항이 있으면 사용자에게 보여주고 계속할지 확인한다.

### 2단계: 현재 버전 파악

`.claude-plugin/marketplace.json`의 `plugins[0].version`을 읽는다.

### 3단계: 버전 타입 결정

사용자가 버전 타입을 명시하지 않은 경우, git diff로 변경 내용을 분석하여 아래 기준으로 자동 판단한다.

| 변경 유형 | 버전 타입 | 판단 기준 |
|----------|---------|---------|
| 문서 수정, 오타, 버그 수정 | **patch** (x.x.+1) | SKILL.md 내용 수정, 참조 경로 수정 |
| 스킬/에이전트 추가, 기능 개선 | **minor** (x.+1.0) | 새 스킬 파일 추가, 워크플로우 변경 |
| 하위호환 파괴, 구조 변경 | **major** (+1.0.0) | 배포 구조 변경, 스킬 인터페이스 변경 |

자동 판단한 버전 타입과 새 버전 번호를 사용자에게 제시하고 확인을 받는다.
사용자가 다른 타입을 원하면 그에 따른다.

**계산 예시**: 현재 `1.7.0`에서 minor → `1.8.0`, patch → `1.7.1`, major → `2.0.0`

### 4단계: 두 파일 동시 업데이트

반드시 두 파일을 동시에 같은 버전으로 업데이트한다. 불일치 시 auto update가 실패한다.

- `.claude-plugin/marketplace.json` → `plugins[0].version`
- `.claude-plugin/plugin.json` → `version`

### 5단계: 커밋 확인 후 실행

변경될 파일 목록과 커밋 메시지를 사용자에게 보여준다.

커밋 메시지 형식: `Release v{버전}`

사용자 승인 후 커밋한다.

```bash
git add .claude-plugin/marketplace.json .claude-plugin/plugin.json
# (스테이지되지 않은 다른 변경사항이 있다면 함께 스테이징할지 사용자에게 확인)
git commit -m "Release v{버전}"
```

### 6단계: develop 푸시 확인 후 실행

```
develop → origin/develop 푸시하시겠습니까?
```

사용자 승인 후 푸시한다.

```bash
git push origin develop
```

### 7단계: main PR 생성

PR을 생성한다. base는 `main`, head는 `develop`.

```bash
gh pr create \
  --base main \
  --head develop \
  --title "Release v{버전}" \
  --body "$(cat <<'EOF'
## Release v{버전}

### 변경 사항
{git log main..develop --oneline 결과를 기반으로 요약}

### 버전
- {이전 버전} → {새 버전}
- 변경 유형: {patch/minor/major}

🤖 Generated with [Claude Code](https://claude.com/claude-code)
EOF
)"
```

PR URL을 사용자에게 보여준다.

### 8단계: PR 머지 확인 후 실행

```
PR #{번호}를 main에 머지하시겠습니까?
```

사용자 승인 후 머지한다. `--merge` (3-way merge, --no-ff 유지)를 사용한다.

```bash
gh pr merge {PR번호} --merge --delete-branch=false
```

> `--delete-branch=false`: develop 브랜치는 삭제하지 않는다.

### 9단계: 완료 보고

```
✅ Release v{버전} 완료
- develop 커밋: {커밋 해시}
- PR: {PR URL}
- main 머지: 완료
```

---

## 규칙

- **develop 브랜치에서만** 버전을 업데이트한다. main 직접 작업 금지.
- 각 단계(커밋/푸시/머지)마다 사용자 승인을 받는다. 자동으로 진행하지 않는다.
- 두 버전 파일은 항상 동일한 버전으로 유지한다.
- PR diff 확인: `gh pr diff {번호} --stat`으로 실제 변경 내용을 확인하고 Summary에 반영한다.
