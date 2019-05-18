# Employee Producer

Il progetto `Employee Producer` è una guida a come implementare un microservizio SpringBoot per acedere alla risorsa d'esempio Employee.
per questo microservizio verranno utilizati i moduli Spring Boot `Web`, `JPA` e `H2`

## Creazione del progetto

una volta creato un nuovo progetto Maven all'interno del vostro IDE preferito:
1. aprite il file `pom.xml`
2. nella sezione `<dependencies>...</dependencies>` includere i seguenti riferimenti
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