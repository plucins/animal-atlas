# Backend Framework Standards

## Spring MVC (Not WebFlux)
This project uses **Spring MVC** (servlet-based, blocking I/O). Do not introduce Spring WebFlux (`Mono`, `Flux`, reactive repositories) — the stack is intentionally synchronous.

## Spring Data JPA + Specification Pattern
Use Spring Data JPA repositories with `JpaSpecificationExecutor<T>` for complex query logic. Encapsulate all filtering criteria in a dedicated `*Specification` class using the JPA Criteria API, keeping services thin.

**Preferred:**
```java
Specification<Animal> spec = AnimalSpecification.from(request);
return repository.findAll(spec, pageable);
```
**Avoid:** Building JPQL query strings in services or adding custom query methods for every filter combination.

## Database Schema Management
`ddl-auto: create-drop` is only acceptable during early development. Before any environment with persistent data:
1. Switch `spring.jpa.hibernate.ddl-auto` to `validate`
2. Introduce **Flyway** or **Liquibase** for versioned schema migrations

Do not commit `create-drop` to a production configuration.

## Module Responsibilities
- `common` module: shared API envelope types (`ApiResponse`, `PageResponse`), auditing infrastructure (`AuditableEntity`)
- `catalog` module: domain entities, services, repositories, specifications, controllers
- `application` module: Spring Boot entry point and environment-specific configuration only
