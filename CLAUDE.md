# peach-harness

PeachSolution 하네스 시스템 마켓플레이스입니다.

## 아키텍처 가이드

→ **@AGENTS.md** 참조 (공통 원칙, 백엔드/프론트엔드 패턴, 스킬 규칙)

## 배포 구조

**마켓플레이스** 방식으로 배포한다. 루트가 마켓플레이스이고, `plugins/` 안에 플러그인을 포함한다.

→ **@docs/DISTRIBUTION.md** 참조 (의사결정 근거, 설치/업데이트 방법)

```
peach-harness/
├── .claude-plugin/
│   └── marketplace.json                    # 마켓플레이스 정의
└── plugins/
    └── peach-harness-plugin/
        ├── .claude-plugin/plugin.json      # 플러그인 정의
        ├── skills/                         # 스킬 (오케스트레이터 + 생성)
        └── agents/                         # 서브에이전트 (역할 실행자)
```

## 설치

```bash
# 1. 마켓플레이스 등록
/plugin marketplace add peachSolution/peach-harness

# 2. 플러그인 설치
/plugin install peach-harness-plugin

# skills.sh 설치 (호환)
npx skills add peachSolution/peach-harness --skill '*' -a claude-code
```
