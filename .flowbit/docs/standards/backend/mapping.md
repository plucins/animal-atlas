# Backend Mapping Standards

## MapStruct for Entity-to-DTO Mapping
Use **MapStruct** interfaces for all entity-to-DTO conversions. MapStruct generates type-safe mapping code at compile time with zero runtime overhead.

**Preferred:**
```java
@Mapper(componentModel = "spring")
public interface AnimalMapper {
  AnimalCardResponse toCardResponse(Animal animal);
}
```
**Avoid:** Manual mapping methods in services or constructors that accept entity parameters in record DTOs.

## Annotation Processor
MapStruct requires the `mapstruct-processor` annotation processor configured in the Maven compiler plugin. Do not remove it from `catalog/pom.xml`.

## Mapping Location
Place all mapper interfaces in the `service/` package of the relevant module (alongside the service that uses them).
