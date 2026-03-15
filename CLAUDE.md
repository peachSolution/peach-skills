# peach-harness

PeachSolution 하네스 시스템 플러그인입니다.

## 아키텍처 가이드

→ **@AGENTS.md** 참조 (공통 원칙, 스킬 규칙, 에이전트 구조)

## 배포 구조

**단일 플러그인 + 자체 마켓플레이스** 방식으로 배포한다. `.claude-plugin/` 안에 marketplace.json과 plugin.json이 공존한다.

→ **@docs/04-배포구조.md** 참조 (의사결정 근거, 설치/업데이트 방법)

```
peach-harness/
├── .claude-plugin/
│   ├── marketplace.json         # 마켓플레이스 정의 (source: "./")
│   └── plugin.json              # 플러그인 정의 (컴포넌트)
├── skills/                      # 스킬 (오케스트레이터 + 생성)
├── agents/                      # 서브에이전트 (역할 실행자)
└── README.md
```

## 설치

```bash
# skills.sh (권고 - 14+ AI 도구 지원)
npx skills add peachSolution/peach-harness --skill '*' -a claude-code

# Claude Code 플러그인 (호환)
# 1. 마켓플레이스 등록
/plugin marketplace add peachSolution/peach-harness

# 2. 플러그인 설치
/plugin install peach-harness-plugin
```
