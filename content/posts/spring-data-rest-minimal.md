+++ 
date = 2021-07-01
title = "A minimal Spring Data REST example"
description = ""
slug = ""
authors = []
tags = []
categories = []
externalLink = ""
series = []
+++

What do you need to complete this tutorial?
* Maven/Gradle
* Java 17

Go to the Spring Initializr website to create a new Spring project

[Spring Initializer](https://start.spring.io/#!dependencies=h2,data-jpa,data-rest)

Create a new Book class

```java
@Entity
public class Book {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private long id;

    private String title;

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }
}
```

Create a Book repository

```java
@RepositoryRestResource(collectionResourceRel = "books", path = "books")
public interface BookRepository extends PagingAndSortingRepository<Book, Long> {
    List<Book> findByTitle(@Param("title") String title);
}
```

Start the application and add a new book via curl

```bash
curl -i --request POST --header "Content-Type:application/json" --data '{"title" : "Moby Dick"}' http://localhost:8080/books
```

Open the app in a browser to see your created book: http://localhost:8080/books


## Further reading
Learn more about SDR:
* [SDR Docs](https://docs.spring.io/spring-data/rest/docs/3.4.2/reference/html)
* [Oliver Drotbohm talks about DDD and Rest implemented by SDR](https://kentcdodds.com/blog/authentication-in-react-applications")
* [StackOverflow answer from Oliver about exposing JPA entities via REST](https://stackoverflow.com/a/38876046/561543)
<!-- /wp:list -->