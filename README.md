# Seu primeiro projeto Java web no Spring Boot - Aulão #005
###### DevSuperior - sua carreira dev com fundamento de ensino superior

**Comunidade no Discord**:
https://discord.gg/SbjpsFv

Não perca as novidades:
- https://instagram.com/devsuperior.ig
- https://facebook.com/devsuperior.fb
- https://youtube.com/devsuperior
- https://twitter.com/devsuperior

Assista o vídeo desta aula:

[![Image](https://img.youtube.com/vi/nQr_X62vq-k/mqdefault.jpg "Vídeo no Youtube")](https://youtu.be/nQr_X62vq-k)

## Sumário
- [O que você vai aprender](#O-que-você-vai-aprender)
- [Pré-requisitos](#Pré-requisitos)
- [Links sobre REST](#Links-sobre-REST)
- [Passos](#Passos)
- [Diagramas](#Diagramas)

## O que você vai aprender
- Criar um simples projeto Java web no Spring Boot e Maven
- Introdução prática a injeção de dependência no Spring Boot
- Introdução prática a REST / web services
- Introdução prática ao Spring Data JPA com banco H2

## Links sobre REST
- https://restfulapi.net/
- https://martinfowler.com/articles/richardsonMaturityModel.html
- https://pt.stackoverflow.com/questions/45783/o-que-%C3%A9-rest-e-restful/45787

## Pré-requisitos

- Lógica de programação
- OO básica
  - Composição de objetos (veja: https://github.com/devsuperior/aulao004)
- BD básico

## Passos

- Criar projeto Maven usando Spring Initializr e importar no STS

- Sugestão: acrescentar no .gitignore:

```yml
.DS_Store
.vscode
.metadata
.mvn

mvnw
mvnw.cmd
```

- Criar a entidade Category

- Criar CategoryResource

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

- Criar CategoryRepository

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

- Implementar CommandLineRunner para instanciar categorias no startup da aplicação. Usar o mecanismo de **injeção de dependência** do Spring Boot para obter uma instância de CategoryRepository.

```java
Category cat1 = new Category(1L, "Electronics");
Category cat2 = new Category(2L, "Books");
```

- Opcional: **salvar um commit**

- Acrescentar entidade Product ao projeto

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

- Opcional: **salvar um commit**

- Acrescentar as dependências Maven de Spring Data JPA e banco H2

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

- Acrescentar as configurações do H2 em application.properties

```yml
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.username=sa
spring.datasource.password=

spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

- Fazer os mapeamentos objeto-relacional

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

- Retirar os ID's das instâncias em CommandLineRunner (deixar como null)

- Trocar o código dos repositories: usar JpaRepository<Entity, ID>

- Fim: **salvar um commit**

## Diagramas

### Modelo conceitual

![myImage](https://github.com/devsuperior/aulao005/raw/master/domain-model.png)

### Instância

![myImage](https://github.com/devsuperior/aulao005/raw/master/domain-instance.png)
