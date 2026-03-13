# 배포 구조

> 작성일: 2026-03-14
> 의사결정: 플러그인(앱) 방식 채택, 마켓플레이스(앱스토어) 방식 불채택

## 결정 사항

peach-harness-plugin은 **플러그인(앱)** 으로 배포한다.

마켓플레이스(앱스토어) 방식은 채택하지 않는다.

## 근거

### 플러그인 vs 마켓플레이스

| 구분 | 플러그인(앱) | 마켓플레이스(앱스토어) |
|------|------------|-------------------|
| 저장소 구성 | 플러그인 1개 = 저장소 1개 | 플러그인 여러 개 = 저장소 1개 |
| 필요 파일 | `.claude-plugin/plugin.json` | `.claude-plugin/marketplace.json` + 각 플러그인의 `plugin.json` |
| 설치 방법 | `/plugin install owner/repo` | `/plugin marketplace add owner/repo` → 안에서 선택 설치 |
| 자동 업데이트 | 지원됨 | 지원됨 |

### 플러그인 방식을 선택한 이유

1. **peach-harness-plugin은 단일 플러그인이다** — 여러 플러그인을 묶을 필요가 없다
2. **최종안(11-피치-하네스-시스템-최종안.md) 섹션 7이 plugin.json만 정의한다** — 설계 의도와 일치
3. **자동 업데이트는 플러그인 방식에서도 동작한다** — 마켓플레이스 없이도 GitHub push → 자동 최신화 가능
4. **구조가 단순하다** — `.claude-plugin/` 안에 `plugin.json` 하나만 관리

### 마켓플레이스가 필요한 경우 (향후 참고)

나중에 peach-analytics-plugin, peach-deploy-plugin 등 여러 플러그인을 만들어 한 저장소에서 관리하고 싶다면, 별도의 마켓플레이스 저장소를 만들어 배포한다.

```
peach-marketplace/                    ← 별도 저장소
├── .claude-plugin/marketplace.json
└── plugins/
    ├── peach-harness-plugin/
    ├── peach-analytics-plugin/
    └── peach-deploy-plugin/
```

## 현재 배포 구조

```
peach-harness-plugin/
├── .claude-plugin/
│   └── plugin.json              ← 유일한 매니페스트
├── skills/                      ← 스킬 (오케스트레이터 + 생성)
├── agents/                      ← 서브에이전트 (역할 실행자)
├── hooks/                       ← Git hooks
├── templates/                   ← 템플릿
└── docs/                        ← 문서
```

## 설치 방법

```bash
# Claude Code 플러그인 설치 (권장)
/plugin install peachSolution/peach-harness-plugin

# skills.sh 호환 설치
npx skills add peachSolution/peach-harness-plugin --skill '*' -a claude-code
```

## 업데이트

- GitHub에 push하면 설치한 사용자가 업데이트 가능
- `/plugin` → Installed 탭 → 해당 플러그인 선택 → Update
- "Enable auto update" 설정 시 Claude Code 실행마다 자동 최신화
