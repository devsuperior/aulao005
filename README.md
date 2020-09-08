# Spring Boot - First Project

Lesson video:<br/>
[![Image](https://img.youtube.com/vi/nQr_X62vq-k/mqdefault.jpg "Vídeo no Youtube")](https://youtu.be/nQr_X62vq-k)

## Sumário
- [What let's learn?](#O-que-você-vai-aprender)
- [Links to Learn REST/Web Services](#Links to Learn REST/Web Services)
- [What you need to know](#What you need to know)
- [Steps](#Steps)
- [Diagrams](#Diagramas)

## What let's learn?
- Building an simple Java Application using Spring Boot and Maven
- Intro Spring Dependency Injection
- ntroduction to RESTful Web Services
- Intro to Spring Data JPA with H2 database

## Links to Learn REST/Web Services
- https://restfulapi.net/
- https://martinfowler.com/articles/richardsonMaturityModel.html
- https://pt.stackoverflow.com/questions/45783/o-que-%C3%A9-rest-e-restful/45787

## What you neeed to know?

- Logic programming
- Basic of Object-Oriented Programming
  - Object composition (watch this: https://github.com/devsuperior/aulao004)
- Basic Database Concepts 

## Steps

- Create an Maven project using Spring Initializr and import it in STS(Spring Tools Suite)

- Some suggestions to add in .gitignore:

```yml
.DS_Store
.vscode
.metadata
.mvn

mvnw
mvnw.cmd
```

- Create Category entity

- Create CategoryResource

```java
@RestController
@RequestMapping(value = "/categories")
public class CategoryResource {

	@GetMapping
	public ResponseEntity<...> findAll() {
		...
		return ResponseEntity.ok().body(...);
	}

	@GetMapping(value = "/{id}")
	public ResponseEntity<...> findById(@PathVariable Long id) {
		...
		return ResponseEntity.ok().body(...);
	}
}
```

- Create CategoryRepository

```java
@Component
public class CategoryRepository {

	public void save(Category obj) {
		...
	}

	public Category findById(Long id) {
		...
	}
	
	public List<Category> findAll() {
		...
	}
}
```

- Implement CommandLineRunner to instantiate categories at the startup application. Use Spring Boot's ** dependency injection ** mechanism to get an instance of CategoryRepository.
```java
Category cat1 = new Category(1L, "Electronics");
Category cat2 = new Category(2L, "Books");
```

- Optional: **save an commit**

- Add Product entity in project

```java
Category cat1 = new Category(1L, "Electronics");
Category cat2 = new Category(2L, "Books");

Product p1 = new Product(1L, "TV", 2200.00, cat1);
Product p2 = new Product(2L, "Domain Driven Design", 120.00, cat2);
Product p3 = new Product(3L, "PS5", 2800.00, cat1);
Product p4 = new Product(4L, "Docker", 100.00, cat2);

cat1.getProducts().addAll(Arrays.asList(p1, p3));
cat2.getProducts().addAll(Arrays.asList(p2, p4));
```

- Optional: **save an commit**

- Add Maven dependencies Spring Data JPA and H2 database

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
	<groupId>com.h2database</groupId>
	<artifactId>h2</artifactId>
	<scope>runtime</scope>
</dependency>
```

- Add H2 settings in application.properties file

```yml
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.username=sa
spring.datasource.password=

spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

- To make objetc-realtional mapping
```java
@Entity
public class Product implements Serializable {
	private static final long serialVersionUID = 1L;

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
	private String name;
	private Double price;
	
	@ManyToOne
	@JoinColumn(name = "category_id")
	private Category category;
```

- Remove ID's from CommandLineRunner (null) instantiates

- Change repositories code: use JpaRepository<Entity, ID>

- End: **save an commit**

## Diagrams

### Conceptual Model

![myImage](https://github.com/devsuperior/aulao005/raw/master/domain-model.png)

### Instantiate

![myImage](https://github.com/devsuperior/aulao005/raw/master/domain-instance.png)
