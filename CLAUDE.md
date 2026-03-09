# peach-skills

PeachSolution 아키텍처 기반 공개 스킬 저장소입니다.

## 아키텍처 가이드

→ **@AGENTS.md** 참조 (공통 원칙, 백엔드/프론트엔드 패턴, 스킬 규칙)

## 스킬 위치

모든 스킬은 `skills/` 디렉토리에 위치합니다.

```
skills/
├── peach-gen-backend/SKILL.md
├── peach-gen-ui/SKILL.md
├── peach-agent-team/SKILL.md
└── ...
```

## 설치

```bash
npx skills add peachSolution/peach-skills --skill peach-gen-backend -a claude-code
```
