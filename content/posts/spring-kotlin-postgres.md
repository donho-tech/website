+++ 
date = 2021-10-11
title = "Create a Kotlin Spring Boot application with PostgreSQL"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

Learn how to bootstrap a Spring Boot app with Kotlin, PostgreSQL, Hibernate and Flyway

Go to the [Spring Initializr website](https://start.spring.io/#!type=maven-project&language=kotlin&dependencies=web,data-jpa,flyway,postgresql) to create a new Spring project with the following dependencies
* org.springframework.boot:spring-boot-starter-web
* org.springframework.boot:spring-boot-starter-data-jpa
* org.postgresql:postgresql
* org.flywaydb:flyway-core

Start a local Postgres database
```bash
docker run --name postgres -e POSTGRES_PASSWORD=test -p 5432:5432 postgres
```

Replace application.properties with application.yaml
```yaml
# Database credentials
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/
    username: postgres
    password: test
  
# Hibernate logging
logging.level.org.hibernate:
  SQL: DEBUG
  type.descriptor.sql.BasicBinder: TRACE
```

Create a Flyway migration script in `src/resources/db.migration/V1__Create_schema.sql`
```SQL
create table users
(
    id   integer primary key generated always as identity,
    name varchar(100)
);
```

Create an entity, a repository and a controller
```kotlin
@Entity
@Table(name = "users")
class User(
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY) val id: Long? = null,
    val name: String,
)

@Repository
interface UserRepository : CrudRepository<User, Long> {
}

@RestController
@RequestMapping("user")
class UserController @Autowired constructor(
    val repository: UserRepository,
) {

    @GetMapping("/{id}")
    fun getCurrent(@PathVariable id: Long): User {
        return repository.findById(id).get()
    }

    @GetMapping("create")
    fun create(): String {
        val user = User(null, "peter")
        repository.save(user)
        return "created"
    }
}
```

Start the application and test it using the above endpoints.
