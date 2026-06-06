# Frontend Testing Standards

## Test Runner: Vitest
All frontend tests use **Vitest** (`^4.0.8`) with **jsdom** as the DOM environment. Do not use Jest or Karma.

Run tests with: `npm test` (which invokes `ng test`)

## Test File Naming
Test files use the `.spec.ts` suffix, co-located with the file they test:
- `animal-card.component.ts` → `animal-card.component.spec.ts`
- `catalog-api.service.ts` → `catalog-api.service.spec.ts`

## What to Test
- **Components**: Verify rendered output, input/output behavior, and signal state changes
- **Services**: Verify HTTP calls with `HttpClientTestingModule` or Vitest mocks
- **Models**: Verify pure transformation functions and label maps

## Angular Testing Utilities
Use Angular's `TestBed` for component tests that require dependency injection. Use plain Vitest `describe`/`it`/`expect` for pure functions and models.
