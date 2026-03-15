# 프론트엔드 규칙 (front/)

> peach-setup-harness Source of Truth — 대상 프로젝트 AGENTS.md의 프론트엔드 규칙 섹션 기준

## 기술 스택
- Vue 3 + Vite + Pinia · **NuxtUI v4** + TailwindCSS v4 · Vitest · **ESLint**

## 모듈 구조
→ `front/src/modules/test-data/` 참조
- pages/ · components/ · store/ · modals/ · types/

계층: `{module}/` · `{domain}/{module}/` · `{domain}/{category}/{module}/`

## Store 표준 (Pinia Option API)
→ `front/src/modules/test-data/store/test-data.store.ts` 참조

**금지:** isLoading, error 상태 · try-catch · 구분선 주석

표준 상태: `listParams`, `listData`, `listTotalRow`, `detailData`
표준 액션: `paging()`, `list()`, `detail()`, `insert()`, `update()`, `updateUse()`, `softDelete()`

## 핵심 원칙 (추론 불가)
- Composition API: `<script setup lang="ts">` 필수
- 모든 API는 Store 통해 호출
- Store 값은 반드시 `computed()`로 래핑
- 파일 업로드: `_common/components/file/` 사용

## TailwindCSS 클래스 그룹핑
5개 이상 클래스 → 배열 그룹화 (Layout/Typography/Pseudo 기준)
→ `front/src/modules/test-data/` 컴포넌트 참조

## _common 래퍼 컴포넌트
`front/src/modules/_common/components/`에 래퍼가 있으면 NuxtUI 직접 사용 대신 우선 사용
```bash
ls front/src/modules/_common/components/
```

## 파일명 규칙
- 페이지: `{module-name}-{page-type}.vue`
- 모달: `{module-name}-{action}.modal.vue`
- 컴포넌트: `{module-name}-{name}.component.vue`
- 스토어: `{module-name}.store.ts` · 타입: `{module-name}.type.ts` · 라우트: `_{module-name}.routes.ts`

## 주석 금지
구분선/섹션/자명한 코드 설명 주석 금지

## 품질 검증
```bash
bun run dev && bun run test && npx vue-tsc --noEmit && bun run lint:fix && bun run build
```
