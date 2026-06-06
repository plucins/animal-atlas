## Error Handling

### Clear User Messages
Show helpful, actionable messages without exposing internal details or security-sensitive information.

### Fail Fast
Validate inputs and check preconditions early; reject invalid data before it causes deeper issues.

### Typed Exceptions
Use specific exception types instead of generic ones to enable precise error handling.

### Centralized Handling
Catch and process errors at appropriate boundaries (controllers, API layers) rather than scattering try-catch throughout.

### Graceful Degradation
When non-critical services fail, continue operating with reduced functionality rather than crashing entirely.

### Retry with Backoff
Use exponential backoff for transient failures when calling external services.

### Resource Cleanup
Always release resources (file handles, connections) in finally blocks or equivalent cleanup mechanisms.

### Backend: Centralized Exception Handling
All backend exceptions must flow through a single `@RestControllerAdvice` class (`GlobalExceptionHandler`). Domain errors must extend a custom typed runtime exception (e.g., `CatalogException`) and carry an `ErrorCode` enum value. The handler converts exceptions into `ApiResponse.error(messages)` envelopes so the REST contract is consistent.

**Preferred:**
```java
@ExceptionHandler(CatalogException.class)
public ResponseEntity<ApiResponse<Void>> handleCatalogException(CatalogException ex) {
  return ResponseEntity.badRequest().body(ApiResponse.error(List.of(message)));
}
```
**Avoid:** Letting exceptions propagate to the framework default error handler or returning raw error strings.
