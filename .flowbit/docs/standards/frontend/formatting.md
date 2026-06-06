# Frontend Formatting Standards

## Prettier Configuration
All frontend TypeScript and JavaScript files are formatted with Prettier. Do not manually override Prettier's output.

### Single Quotes
Use single quotes for all TypeScript/JavaScript strings.

**Preferred:** `const label = 'Lion';`  
**Avoid:** `const label = "Lion";`

### Print Width
Wrap lines at 100 characters. Break long method chains, object literals, or template expressions before they reach 100 characters.

### Angular HTML Templates
Format component HTML templates using Prettier with the `angular` parser. This is enforced via `.prettierrc` overrides for `*.html` files. Do not format HTML templates with generic HTML tools.

## Formatting Source of Truth
The `.prettierrc` file in `animal-catalog-front/` is the canonical formatting configuration. When in doubt, run Prettier rather than hand-formatting.
