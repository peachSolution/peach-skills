# 백엔드 규칙 (api/)

> peach-setup-harness Source of Truth — 대상 프로젝트 AGENTS.md의 백엔드 규칙 섹션 기준
> Elysia 전용 규칙은 `backend-elysia-rules.md` 참조

## 기술 스택 (공통)
- **런타임**: Bun
- **DAO**: bunqldb (Bun Native SQL)
- **린트**: Biome
- **테스트**: bun:test

## Controller 프레임워크 자동 감지

```bash
head -3 api/src/modules/test-data/controller/test-data.controller.ts
```

- `routing-controllers` → Koa 모드 (데코레이터 패턴, class-validator)
- `elysia` / `createElysia` → Elysia 모드 (체이닝 패턴, TypeBox, docs/)

## 모듈 구조
→ `api/src/modules/test-data/` 참조
- controller/ · service/ · dao/ · type/ · test/
- Elysia 추가: docs/

## 표준 메서드명
→ `api/src/modules/test-data/` 참조
- Controller: getAll, getOne, insert, update, updateUse, softDelete, hardDelete
- Service/DAO: findPaging, findList, findOne, insert, update, updateUse, softDelete, hardDelete

## DB 규칙
- Boolean: `CHAR(1)` Y/N
- 금액: `DECIMAL(14,0)`
- PK: `int` 자동증가, 접미사 `seq` (예: `member_seq`, `board_seq`)
- 감사 칼럼: `is_use`, `is_delete`, `insert_seq`, `insert_date`, `update_seq`, `update_date`
- 스키마 확인: `db/schema/[테이블명].sql`
- 표준 타입: `[테이블명]`, `[테이블명]PagingDto`, `[테이블명]InsertDto`, `[테이블명]UpdateDto`

## 핵심 원칙
- Service: **static 메서드**
- DAO: **bunqldb** 재할당 방식 조건부 쿼리
  - `api/src/modules/test-data/dao/test-data.dao.ts` 참조
  - `DB.many<T>()`, `DB.paginateSql<T>()` 사용
- 파일 업로드: `_common/file` 사용
- 에러: 기능오류 → HTTP 200 + `{success:false}` | 시스템예외 → `ErrorHandler`

## 품질 검증
```bash
bun start && bun test && bun run lint:fixed
```
