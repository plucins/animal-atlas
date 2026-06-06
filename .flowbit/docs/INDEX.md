# Documentation Index

**IMPORTANT**: Read this file at the beginning of any development task to understand available documentation and standards.

## Quick Reference

### Project Documentation
Project-level documentation covering architecture, technology choices, and system design.

| File | Purpose |
|---|---|
| `project/tech-stack.md` | Full technology stack — languages, frameworks, versions, build tools, dev tooling |
| `project/architecture.md` | System architecture — C4 container diagram, module breakdown, data flow, domain model |

### Technical Standards
Coding standards, conventions, and best practices organized by domain.

| File | Purpose |
|---|---|
| `standards/global/coding-style.md` | Naming, formatting, indentation (2 spaces), file encoding (UTF-8) |
| `standards/global/commenting.md` | Commenting philosophy — let code speak |
| `standards/global/conventions.md` | Structural conventions + Flowbit workflow integration |
| `standards/global/error-handling.md` | Error handling + backend `GlobalExceptionHandler` pattern |
| `standards/global/minimal-implementation.md` | Build only what's needed |
| `standards/global/validation.md` | Server- and client-side validation rules |
| `standards/backend/api.md` | REST design + `ApiResponse`/`PageResponse` envelope + path conventions |
| `standards/backend/framework.md` | Spring MVC, JPA Specification pattern, module responsibilities |
| `standards/backend/java.md` | Java 25, records, no Lombok, UUID PKs, modern idioms |
| `standards/backend/mapping.md` | MapStruct interfaces, annotation processor, placement |
| `standards/backend/migrations.md` | Schema migration rules |
| `standards/backend/models.md` | Entity naming, constraints, relationships |
| `standards/backend/queries.md` | Parameterized queries, N+1 avoidance, indexing |
| `standards/frontend/accessibility.md` | Semantic HTML, keyboard nav, ARIA |
| `standards/frontend/components.md` | Angular Signals, standalone, smart/dumb, lazy routes, method style, PrimeNG as first choice |
| `standards/frontend/css.md` | Tailwind methodology, design tokens, minimal custom CSS |
| `standards/frontend/file-naming.md` | Kebab-case + role suffix; PascalCase classes; kebab-case dirs |
| `standards/frontend/formatting.md` | Prettier config, single quotes, 100-char width, Angular HTML parser |
| `standards/frontend/responsive.md` | Mobile-first, breakpoints, fluid layouts |
| `standards/frontend/tooling.md` | npm scripts as entry points; Angular CLI only |
| `standards/frontend/typescript.md` | ES2022, strict options, interface vs type, relative imports |
| `standards/testing/backend.md` | `@DataJpaTest`, `@WebMvcTest`, test priority, file location |
| `standards/testing/frontend.md` | Vitest + jsdom, `.spec.ts`, TestBed, co-location |
| `standards/testing/test-writing.md` | Test behavior, descriptive names, mocking strategy |

---

## Project Documentation

Located in `.flowbit/docs/project/`

#### Technology Stack (`project/tech-stack.md`)
Animal Atlas is a tech demo showcasing **Spring Boot 4 / Java 25** (backend) + **Angular 22 / TypeScript 6** (frontend) + **PostgreSQL**. Key highlights:
- **Backend**: Spring Boot 4, Java 25 (Records, no Lombok), MapStruct 1.6.3, Spring Data JPA + Hibernate
- **Frontend**: Angular 22 (Signals, standalone components, `@if`/`@for`), PrimeNG 21, Tailwind CSS 4, Vitest 4
- **Build**: Maven multi-module (`mvnw`) + npm 11 (`ng build`)
- **DB**: PostgreSQL, schema `animal_atlas`, currently `ddl-auto: create-drop` (dev only)
- ⚠️ No Docker/CI configured; no real tests written yet; credentials hardcoded in `application.yml`

#### System Architecture (`project/architecture.md`)
Full-stack umbrella: two independent Git repos under a single parent directory.
- **Pattern**: Layered REST API (Controller → Service → Repository → PostgreSQL) + Angular SPA (feature-based, smart/dumb separation)
- **Modules**: `common` (audit, shared DTOs), `catalog` (domain logic, JPA Specification search), `application` (bootstrap)
- **Frontend features**: `core` (HTTP), `shell` (layout), `features/catalog` (list page, animal card, filter sidebar, toolbar)
- **Data Flow**: `AnimalSpecification` (JPA Criteria API) drives server-side filtering; Angular Signals + IntersectionObserver drive infinite scroll
- ⚠️ Hardcoded `localhost:8080`; `create-drop`; no Flyway/Liquibase

---

## Technical Standards

### Global Standards

Located in `.flowbit/docs/standards/global/`

#### Coding Style (`standards/global/coding-style.md`)
Naming consistency, automatic formatting, descriptive names, focused functions, uniform indentation, no dead code, no backward compat unless required, DRY principle. **Project-specific:** 2-space indentation across TypeScript/HTML/Java; UTF-8 encoding; single trailing newline; no trailing whitespace.

#### Commenting (`standards/global/commenting.md`)
Let code speak through structure and naming, comment sparingly for non-obvious logic, no change/changelog comments.

#### Conventions (`standards/global/conventions.md`)
Predictable structure, up-to-date documentation, clean version control, environment variables, minimal dependencies, consistent reviews, testing standards, feature flags, changelog updates, build what's needed. **Flowbit integration:** always read INDEX.md first; follow `.flowbit/docs/standards/`; invoke `/flowbit-*` commands via Skill tool immediately.

#### Error Handling (`standards/global/error-handling.md`)
Clear user messages without internal details, fail fast on invalid input, typed exceptions, centralized handling at API boundaries, graceful degradation, retry with backoff, resource cleanup. **Backend:** all exceptions flow through `GlobalExceptionHandler` (`@RestControllerAdvice`); domain errors extend `CatalogException` with `ErrorCode`; responses use `ApiResponse.error()` envelope.

#### Minimal Implementation (`standards/global/minimal-implementation.md`)
Build only what's needed, clear purpose per method, delete exploration artifacts, no future stubs or placeholder functions, no speculative abstractions, review before commit, treat unused code as debt.

#### Validation (`standards/global/validation.md`)
Server-side always, client-side for UX feedback, validate early, specific field-level errors, allowlists over blocklists, type and format checks, input sanitization, business rule validation, consistent enforcement across all entry points.

---

### Backend Standards

Located in `.flowbit/docs/standards/backend/`

#### API Design (`standards/backend/api.md`)
RESTful principles, consistent naming with lowercase/hyphens, URL versioning, plural nouns for resources, max 2-3 levels of URL nesting, query parameters for filtering/sorting/pagination, proper HTTP status codes, rate limit headers. **Project rules:** always wrap responses in `ApiResponse<T>` / `PageResponse<T>` envelopes; paths use `/api/` prefix with lowercase plural nouns; filters go as query params.

#### Framework (`standards/backend/framework.md`)
Spring MVC only (no WebFlux). JPA repositories with `JpaSpecificationExecutor` + dedicated `*Specification` classes. `ddl-auto: create-drop` dev-only — use Flyway/Liquibase before persistent data. Module split: `common` (envelopes, auditing), `catalog` (domain), `application` (bootstrap).

#### Java (`standards/backend/java.md`)
Java 25 target. DTOs as records (no Lombok). UUID primary keys. Modern Java idioms (pattern matching, sealed types, no raw generics). PascalCase filenames. Fully qualified imports — no wildcards.

#### Mapping (`standards/backend/mapping.md`)
MapStruct interfaces for all entity-to-DTO mapping. `mapstruct-processor` stays in `catalog/pom.xml`. Mapper interfaces live in the `service/` package alongside the service that uses them.

#### Database Migrations (`standards/backend/migrations.md`)
Always reversible, small and focused per change, zero-downtime awareness, separate schema from data migrations, careful indexing on large tables, descriptive names, version-controlled and never modified after deployment.

#### Models (`standards/backend/models.md`)
Singular model names / plural table names, timestamps for auditing, database-level constraints (NOT NULL, UNIQUE, FK), appropriate data types, index foreign keys, multi-layer validation, clear relationships with cascade, practical normalization.

#### Database Queries (`standards/backend/queries.md`)
Parameterized queries always, avoid N+1 with eager loading/joins, select only needed columns, index strategic columns, transactions for related operations, query timeouts, cache expensive queries.

---

### Frontend Standards

Located in `.flowbit/docs/standards/frontend/`

#### Accessibility (`standards/frontend/accessibility.md`)
Semantic HTML elements, full keyboard navigation with visible focus, 4.5:1 color contrast, alt text and form labels, screen reader testing, ARIA for complex components, proper heading hierarchy, focus management in dynamic content and SPAs.

#### Components (`standards/frontend/components.md`)
Single responsibility, reusable across contexts, composable from smaller parts, clear explicit props with defaults, encapsulation, consistent naming, local state by default, minimal props, documented usage and examples. **Angular:** use Signals (`signal()`, `computed()`, `input()`, `output()`); `standalone: true` always; no NgModules; smart/dumb separation (pages own state, components are display-only); lazy-loaded routes at every level; methods as named declarations (not arrow field assignments). **PrimeNG as first choice:** always use PrimeNG components (buttons, tables, dialogs, etc.) before building custom UI; import directly, do not wrap in app-level wrappers.

#### File Naming (`standards/frontend/file-naming.md`)
Kebab-case filenames with Angular role suffix (`.component.ts`, `.service.ts`, `.routes.ts`, `.model.ts`, `.token.ts`, `.spec.ts`). PascalCase class names mirror filename. Kebab-case directory names.

#### Formatting (`standards/frontend/formatting.md`)
Prettier for all TS/JS files. Single quotes. 100-character print width. Angular HTML templates formatted with `angular` parser via `.prettierrc` overrides. `.prettierrc` in `animal-catalog-front/` is the source of truth.

#### Tooling (`standards/frontend/tooling.md`)
Use declared npm scripts (`npm start`, `npm run build`, `npm run watch`, `npm test`). Do not call `ng` directly. Angular CLI (`@angular/build`) is the only bundler — no Vite or Webpack overrides without team discussion.

#### TypeScript (`standards/frontend/typescript.md`)
ES2022 target, `"module": "preserve"`. Strict options active: `noImplicitOverride`, `noPropertyAccessFromIndexSignature`, `noImplicitReturns`, `noFallthroughCasesInSwitch`, `isolatedModules`. `interface` for data contracts; `type` only for unions/mapped/conditional types. Relative imports only — no `@/` aliases.

#### CSS (`standards/frontend/css.md`)
Consistent methodology (Tailwind / BEM / CSS modules), work with framework patterns, design tokens for colors/spacing/typography, minimize custom CSS, production optimization via purging/tree-shaking.

#### Responsive Design (`standards/frontend/responsive.md`)
Mobile-first progressive enhancement, standard breakpoints, fluid layouts, relative units (rem/em), cross-device testing, touch-friendly tap targets (44×44px min), mobile performance optimization, readable typography, content priority on small screens.

---

### Testing Standards

Located in `.flowbit/docs/standards/testing/`

#### Test Writing (`standards/testing/test-writing.md`)
Test behavior not implementation, descriptive test names, mock external dependencies, fast unit test execution, risk-based prioritization, balance coverage with velocity, critical path focus, appropriate edge case depth.

#### Backend Testing (`standards/testing/backend.md`)
Spring Boot test slices: `@DataJpaTest` for repositories/Specifications, `@WebMvcTest` for controllers, `@SpringBootTest` only when slices are insufficient. Priority order: repository + spec tests → controller tests → service unit tests. Tests in `src/test/java/` mirroring main structure.

#### Frontend Testing (`standards/testing/frontend.md`)
Vitest `^4.0.8` with jsdom. `.spec.ts` files co-located with source. Test components (output/signals), services (HTTP with `HttpClientTestingModule`), and pure model functions. `TestBed` for DI-dependent tests; plain Vitest `describe`/`it`/`expect` for pure functions.

---

## How to Use This Documentation

1. **Start Here**: Always read this INDEX.md first to understand what documentation exists
2. **Project Context**: Read relevant project documentation before starting work
3. **Standards**: Reference appropriate standards when writing code
4. **Keep Updated**: Update documentation when making significant changes
5. **Customize**: Adapt all documentation to your project's specific needs

## Updating Documentation

- Project documentation should be updated when goals, tech stack, or architecture changes
- Technical standards should be updated when team conventions evolve
- Always update INDEX.md when adding, removing, or significantly changing documentation
