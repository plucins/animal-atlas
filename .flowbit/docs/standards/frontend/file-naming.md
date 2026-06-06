# Frontend File Naming Conventions

## Kebab-Case with Role Suffix
All multi-word Angular source files use kebab-case and append a role suffix that matches the Angular artifact type.

| Artifact | Suffix | Example |
|----------|--------|---------|
| Component | `.component.ts` | `animal-card.component.ts` |
| Service | `.service.ts` | `catalog-api.service.ts` |
| Route definition | `.routes.ts` | `catalog.routes.ts` |
| Model / interface file | `.model.ts` | `animal-card.model.ts` |
| Injection token | `.token.ts` | `api-base-url.token.ts` |
| Test file | `.spec.ts` | `animal-card.component.spec.ts` |

## Class Names
The TypeScript class name mirrors the filename in PascalCase: `animal-card.component.ts` → `AnimalCardComponent`.

## Directory Names
Feature directories also use kebab-case: `catalog-list-page/`, `animal-card/`, `filter-sidebar/`.
