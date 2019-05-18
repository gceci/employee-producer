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
```java
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
2.	aggiungere la sezione di settings di H2  
```java
h2:
    console:
      enabled: true
      path: /console
      settings:
        trace: false
        web-allow-others: false
```
in particolare la property `path: /console` permette di definire l'endpoint con cui accedere al pannello web di controllo del DB appena configurato.
Se non avete dichiarato un particolare numero di porta nel file `application.properties` l'applicazione verrà avviata all'indirizzo `localhost:8080`, quindi all'indirizzo `localhost:8080/console`.  
Cliccando su `Connect` si accederà al contenuto del DB (ancora completamente vuoto)

## Definizione del modello entità Employee

Prima di popolare il DB, abbiamo bisogno di definire il modello dati dell'entità governata dal microservizio Employee Producer.  
Creiamo una nuova classe `Employee.java` al'interno del package `com.example.springcloud.model`.  
La classe conterrà la definizione della risorsa Employee e i riferimenti alla tabella el DB H2 che conterrà i record che andremo a inserire.  
1.	annotare la classe con i moduli `@Entity` e `@Table`
```java
@Entity
@Table(name = "tbl_employee")
public class Employee {...}
```
2.	definire chiavi e attributi dell'entità Employee, ad esempio
```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
private String name;
private String surname;
private String level;
private Double salary;
```
3.	creare `getter` e `setter`, `costruttori` e metodo `toString()`
```java
//constructor
public Employee(String name, String surname, String level, Double salary) {
	this.name = name;
	this.surname = surname;
	this.level = level;
	this.salary = salary;
}

//toString	
@Override
public String toString() {
	return "Employee [id=" + id + ", name=" + name + ", surname=" + surname + ", level=" + level + ", salary="
			+ salary + "]";
}
```

## Inizializzare DB H2

nela definizione del modello entità abbiamo dichiarato il nome della tabella `tbl_employee` che ora inizializzeremo mediante l'utilizzo del file `data.sql` da salvare nelle risorse del progetto. 
Occorrerà creare il file e riportare al suo interno il sql necessario a fare l'insert dei record che vogliamo siano sempre presenti allo startup del nostro progetto.  
ad esempio:  
```sql
INSERT INTO tbl_employee (id, name, surname, level, salary) VALUES (1, 'John', 'Doe', 'Manager', 1000.0);
INSERT INTO tbl_employee (id, name, surname, level, salary) VALUES (2, 'Jim', 'Curtney', 'CIO', 2000.0);
INSERT INTO tbl_employee (id, name, surname, level, salary) VALUES (3, 'Mark', 'Pillow', 'CFO', 3000.0);
INSERT INTO tbl_employee (id, name, surname, level, salary) VALUES (4, 'Ella', 'Star', 'CEO', 4000.0)
```
Attenzione a rispettare il modello dati dichiarato in precedenza.
  
Quando accederemo a `localhost:8080/console` (dopo aver compilato ed eseguito il nostro progetto) troveremo all'interno del DB la tabella definita e i record riportati sopra.

## Servizi di accesso al DB - Logica applicativa

### Repository Setup
`SpringBoot JPA` semplifica molto l'implementazione delle interfacce necessarie per l'accesso alle informazioni contenute all'interno del notro DB.
Per accessi basilari, come negli esempi di questo tutorial, sarà necessario semplicemente definire una nuova interfaccia `EmployeeRepository.java` all'interno del package `com.example.springcloud.repository`.  
1.	annotare l'interfaccia con `@Repository`
2.	estendere l'interfacia con `JpaRepository<class, T>`
```java
public interface EmployeeRepository extends JpaRepository<Employee, Long> {}
```
Con questa implementazione sarà possibile utilizzare primitive come `findAll()`, `findOne()` e `save()` per effettuare la ricerca di liste, singoli record e insermenti reispettivamente.

### Service Setup
1.	creare la classe `EmployeeService.java` all'interno del package `com.example.springcloud.service`
2.	annotare la clase con `@Service`
3.	referenziare il repository creato in precedenza inizializzandone una variabile
```java
@Autowired
private EmployeeRepository employeeRepository;
```
4.	definire la funzione che permette di recuperare la lista di Employee
```java
public List<Employee> findAllEmployee(){
	return employeeRepository.findAll();
}
```
5.	definire la funzione che permette di recuperare il singolo oggetto Employee dato comenpu ili suo ID
```java
public Employee findById(Long id){
	return employeeRepository.findOne(id);
}
```
6.	definire la funzione che permette di aggiungere un record di tipo Employee
```java
public void addEmployee(Employee employee) {
	employeeRepository.save(employee);
	System.out.println(employee.toString());
}
```

### Controller Setup
...

## Build e Run
...

