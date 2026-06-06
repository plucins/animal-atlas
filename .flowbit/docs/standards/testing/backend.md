# Backend Testing Standards

## Spring Boot Test Slices
Use Spring Boot's test slice annotations rather than loading the full application context:

| What to test | Annotation | Scope |
|---|---|---|
| JPA repositories, Specifications | `@DataJpaTest` | Persistence layer only |
| REST controllers | `@WebMvcTest` | MVC layer only |
| Full integration | `@SpringBootTest` | Only when slice tests are insufficient |

**Preferred:**
```java
@DataJpaTest
class AnimalRepositoryTest {
  @Autowired AnimalRepository repository;
  // Tests AnimalSpecification filtering logic
}

@WebMvcTest(AnimalController.class)
class AnimalControllerTest {
  @MockBean AnimalCatalogService service;
  // Tests HTTP layer only
}
```

## Test Priority
Given the current state of the project (no tests written), prioritize:
1. `@DataJpaTest` for `AnimalRepository` + `AnimalSpecification` — covers the filter/search logic
2. `@WebMvcTest` for `AnimalController` — covers the HTTP contract
3. Service unit tests — pure unit tests with mocked repositories

## Test File Location
Test files go in `src/test/java/` mirroring the main source structure.
