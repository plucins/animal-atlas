## Components

### Single Responsibility
Each component should do one thing well.

### Reusability
Design components to work across different contexts with configurable props.

### Composability
Build complex UIs by combining smaller components rather than creating monoliths.

### Clear Interface
Define explicit, documented props with sensible defaults.

### Encapsulation
Keep implementation details private; expose only what's necessary.

### Consistent Naming
Use descriptive names that indicate purpose and follow team conventions.

### Local State
Keep state as close to where it's used as possible; lift only when needed.

### Minimal Props
If a component needs many props, consider composition or splitting it.

### Documentation
Document usage, props, and examples to help team adoption.

### Angular Signals and Reactivity
Use Angular Signals (`signal()`, `computed()`, `input()`, `output()`, `viewChild()`) for all component state and data binding. Do not use legacy `@Input()`/`@Output()` decorators in new components.

**Preferred:**
```typescript
readonly animals = signal<AnimalCard[]>([]);
readonly isLoading = signal(false);
readonly query = input<string>('');
```
**Avoid:** `@Input() query: string;` or `@Output() queryChange = new EventEmitter<string>();`

### Standalone Components
All Angular components must declare `standalone: true`. Do not create NgModules.

**Preferred:**
```typescript
@Component({ selector: 'app-animal-card', standalone: true, imports: [...] })
export class AnimalCardComponent {}
```

### Smart / Dumb Component Separation
Page components (in `pages/`) own state, data fetching, and business logic. Display components (in `components/`) receive data only via inputs and emit events via outputs. Data-access logic belongs in services under `data-access/`.

### Lazy-Loaded Routes
All routes at every level (shell and feature) must be lazy-loaded.

**Preferred:**
```typescript
{ path: 'catalog', loadChildren: () => import('./features/catalog/catalog.routes') }
```

### Method Declarations in Classes
Angular component methods must be written as named method declarations, not arrow function class fields. Use arrow functions only for inline callbacks in RxJS operators.

**Preferred:**
```typescript
onSortChange(option: SortOption): void { this.sortChange.emit(option.value); }
```
**Avoid:** `onSortChange = (option: SortOption) => { ... };`

### PrimeNG as First Choice
Always prefer **PrimeNG v21** library components over custom-built implementations. Before building any UI element (buttons, inputs, tables, dialogs, dropdowns, calendars, chips, tags, etc.), check the PrimeNG v21 component catalogue first. Build a custom component only when PrimeNG v21 has no suitable component for the use case. Import PrimeNG components directly — do not wrap them in additional app-level wrapper components.

**Preferred:**
```typescript
import { ButtonModule } from 'primeng/button';
import { TableModule } from 'primeng/table';
import { DialogModule } from 'primeng/dialog';
import { ChipModule } from 'primeng/chip';
import { TagModule } from 'primeng/tag';
```
**Avoid:** Building custom `AppButtonComponent`, `AppTableComponent`, etc. when PrimeNG v21 equivalents exist.

Use PrimeNG v21 `severity` values for semantic coloring (`'success' | 'info' | 'warn' | 'danger' | 'secondary' | 'contrast'`). Override visual appearance via `styleClass` and `[style]` bindings or global CSS token overrides in `styles.css`.
