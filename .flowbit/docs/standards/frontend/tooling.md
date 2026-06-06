# Frontend Tooling Standards

## Standard npm Scripts
Always use the declared npm scripts as entry points for frontend tasks. Do not call Angular CLI (`ng`) directly in development workflows.

| Task | Command |
|------|---------|
| Development server | `npm start` (runs `ng serve`) |
| Production build | `npm run build` (runs `ng build`) |
| Watch build | `npm run watch` (runs `ng build --watch --configuration development`) |
| Tests | `npm test` (runs `ng test`) |

## Build Tool
The frontend build is managed by Angular CLI (`@angular/build`). Do not introduce alternative bundlers (Vite, Webpack config overrides) without team discussion.
