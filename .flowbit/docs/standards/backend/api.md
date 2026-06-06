## API Design

### RESTful Principles
Use resource-based URLs with appropriate HTTP methods (GET, POST, PUT, PATCH, DELETE).

### Consistent Naming
Use lowercase, hyphenated or underscored names consistently across endpoints.

### Versioning
Implement versioning (URL path or headers) to manage breaking changes.

### Plural Nouns
Use plural nouns for resources (`/users`, `/products`).

### Limited Nesting
Keep URL nesting to 2-3 levels maximum for readability.

### Query Parameters
Use query parameters for filtering, sorting, and pagination.

### Proper Status Codes
Return appropriate HTTP status codes (200, 201, 400, 404, 500).

### Rate Limit Headers
Include rate limit information in response headers.

### Consistent API Response Envelope
Always wrap backend responses in the shared envelope types from the `common` module:
- `ApiResponse<T>` — for all responses (success and error)
- `PageResponse<T>` — for paginated collections, nested inside `ApiResponse`

**Preferred:**
```java
return ResponseEntity.ok(ApiResponse.success(PageResponse.from(page)));
```
**Avoid:** Returning bare DTOs or collections directly from controllers.

### REST Path Conventions
Use lowercase plural resource names under an `/api/` prefix. Filter, sort, and pagination parameters belong as query parameters, not in path segments.

**Preferred:** `GET /api/catalog/animals?query=lion&diets=CARNIVORE&page=0&size=12`  
**Avoid:** `/api/catalog/animal/search` or `/api/Catalog/Animals`
