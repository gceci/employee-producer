# Employee Producer

Il progetto `Employee Producer` è una guida a come implementare un microservizio SpringBoot per acedere alla risorsa d'esempio Employee.
per questo microservizio verranno utilizati i moduli Spring Boot `Web`, `JPA` e `H2`

## Creazione del progetto

una volta creato un nuovo progetto Maven all'interno del vostro IDE preferito:  
1.	aprite il file `pom.xml`  
2.	nella sezione `<dependencies>...</dependencies>` includere i seguenti riferimenti
```java
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
		
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
	<groupId>com.h2database</groupId>
	<artifactId>h2</artifactId>
	<scope>runtime</scope>
</dependency>
```

## Setup del Database H2

verificare se presente il file `applicatio.yml` all'interno della root di progetto.
nel caso non fosse già generato dalle procedure del vostro IDE, createlo. in seguito:  
1.	aggiungere la sezione dedicata aai settings del DB
```
spring:
  datasource:
    url: jdbc:h2:mem:employee-producer-app;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
    platform: h2
    username: sa
    password:
    driverClassName: org.h2.Driver
  jpa:
    database-platform: org.hibernate.dialect.H2Dialect
    hibernate:
      ddl-auto: update
    properties:
      hibernate:
        show_sql: true
        use_sql_comments: true
        format_sql: true
```