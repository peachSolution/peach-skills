# Validator / 타입 규칙

> peach-setup-harness Source of Truth — 대상 프로젝트 AGENTS.md의 Validator/타입 규칙 섹션 기준

## Validator 선택 (프레임워크별)

| 프레임워크 | Validator | 감지 방법 |
|-----------|-----------|----------|
| Koa (routing-controllers) | class-validator | `head -3 controller/test-data.controller.ts` → `routing-controllers` |
| Elysia | TypeBox (`t`) | `head -3 controller/test-data.controller.ts` → `elysia` |

---

## class-validator 작성 규칙 (Koa)

→ `api/src/modules/test-data/controller/test-data.validator.ts` 참조

- DB 컬럼 null 허용 시 `@IsOptional` 적용
- 주석은 필드 왼쪽에 배치

주요 데코레이터: `@IsNotEmpty()`, `@IsString()`, `@IsNumber()`, `@IsOptional()`, `@IsEmail()`, `@MinLength(n)`

---

## TypeBox 작성 규칙 (Elysia)

→ `api/src/modules/test-data/controller/test-data.validator.ts` 참조

```typescript
import { t } from 'elysia';

export const exampleInsertValidator = t.Object({
  name: t.String({ minLength: 1 }),
  amount: t.Number(),
});
```

---

## 타입 규칙 (공통)
- 옵셔널(`?`) 금지 · `null` 타입 금지 · `undefined` 타입 금지

## 표준 인터페이스 명명
- `[테이블명]` — 기본 조회 타입
- `[테이블명]PagingDto` — 페이징 파라미터
- `[테이블명]InsertDto` — 등록 DTO
- `[테이블명]UpdateDto` — 수정 DTO

## 필드 배치 순서
1. 비즈니스 필드
2. 감사 필드 (`isView`, `isUse`, `isDelete`, `insertSeq`, `insertDate`, `updateSeq`, `updateDate`)
3. 파일 필드 (`fileList`, `imageList`)
