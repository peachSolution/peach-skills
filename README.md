# peach-skills

PeachSolution 아키텍처 기반의 공개 스킬 모음입니다.

## 문서

- **[AGENTS.md](AGENTS.md)** - 아키텍처 가이드 (공통 원칙, 백엔드/프론트엔드 패턴, 스킬 규칙)
- **[CLAUDE.md](CLAUDE.md)** - Claude Code 진입점

이 저장소는 `skills.sh` 기준의 공용 배포 저장소이며, 에이전트 전용 `.claude/agents` 대신 `skills/` 아래의 스킬 정본만 관리합니다.

## 설치

```bash
# 특정 스킬 설치
npx skills add peachSolution/peach-skills --skill peach-gen-backend -a claude-code

# 여러 에이전트에 설치
npx skills add peachSolution/peach-skills --skill peach-agent-team -a claude-code -a codex -a openclaw

# 전체 설치
npx skills add peachSolution/peach-skills --skill '*' -a claude-code -a codex
```

## 스킬 목록

- `peach-add-api`
- `peach-add-cron`
- `peach-add-print`
- `peach-gen-backend`
- `peach-gen-db`
- `peach-gen-design`
- `peach-gen-feature-docs`
- `peach-gen-prd`
- `peach-gen-store`
- `peach-gen-ui`
- `peach-refactor-backend`
- `peach-refactor-frontend`
- `peach-agent-team`
- `peach-agent-team-refactor`

## 마이그레이션

| 기존 이름 | 새 이름 |
| --- | --- |
| `add-api` | `peach-add-api` |
| `add-cron` | `peach-add-cron` |
| `add-print` | `peach-add-print` |
| `gen-backend` | `peach-gen-backend` |
| `gen-db` | `peach-gen-db` |
| `gen-design` | `peach-gen-design` |
| `gen-feature-docs` | `peach-gen-feature-docs` |
| `gen-prd` | `peach-gen-prd` |
| `gen-store` | `peach-gen-store` |
| `gen-ui` | `peach-gen-ui` |
| `refactor-backend` | `peach-refactor-backend` |
| `refactor-frontend` | `peach-refactor-frontend` |
| `team-backend`, `team-ui`, `team-fullstack` | `peach-agent-team` |
| `team-refactor` | `peach-agent-team-refactor` |

## 팀 스킬 정책

- `peach-agent-team`은 생성 계열 팀 조율 스킬입니다.
- `mode=backend|ui|fullstack` 분기로 기존 `team-backend`, `team-ui`, `team-fullstack`를 대체합니다.
- `peach-agent-team-refactor`는 리팩토링 전용 팀 조율 스킬입니다.
- Claude 전용 `.claude/agents` 파일은 이 저장소에 복사하지 않고, 역할 정의와 체크리스트를 팀 스킬 내부에 흡수합니다.
