# Frontend TypeScript Standards

## Compiler Target
Compile TypeScript to **ES2022** with `"module": "preserve"` for native ESM compatibility. Do not lower the target.

## Required Compiler Options
The following TypeScript strict options are active and must not be disabled:
- `noImplicitOverride` — always use the `override` keyword when overriding a base class member
- `noPropertyAccessFromIndexSignature` — use explicit index access syntax for index-signature types
- `noImplicitReturns` — every code path in a function must return a value
- `noFallthroughCasesInSwitch` — every switch case must break or return
- `isolatedModules` — every file must be a module (has at least one import or export)

## Angular Compiler Options
- `strictInjectionParameters: true` — always declare types for injectable constructor parameters
- `strictInputAccessModifiers: true` — respect access modifiers on component inputs
- `enableI18nLegacyMessageIdFormat: false` — use the newer i18n message ID format

## Type vs Interface
Use `interface` declarations for data contracts and API shapes. Use `type` aliases only for union types, mapped types, or conditional types.

**Preferred:**
```typescript
export interface PageResponse<T> {
  items: T[];
  totalElements: number;
  totalPages: number;
}
```
**Avoid:** `export type PageResponse<T> = { items: T[]; ... };`

## Imports
Use relative paths for app-local imports. Do not introduce path aliases (`@/`). Import external packages by package name.

**Preferred:**
```typescript
import { API_BASE_URL } from '../../../core/http/api-base-url.token';
import { HttpClient } from '@angular/common/http';
```
**Avoid:** `import { API_BASE_URL } from '@/core/http/api-base-url.token';`
