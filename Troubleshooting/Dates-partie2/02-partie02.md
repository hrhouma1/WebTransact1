------------------------------
# Un projet Spring boot avec les dates
------------------------------



1. Event.java (Modèle)

```java
package com.example.eventmanager.model;

import jakarta.persistence.*;
import java.time.*;

@Entity
@Table(name = "events")
public class Event {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String description;

    private LocalDate localDate;
    private LocalTime localTime;
    private LocalDateTime localDateTime;
    private ZonedDateTime zonedDateTime;
    private Instant instant;
    private OffsetDateTime offsetDateTime;
    private YearMonth yearMonth;
    private MonthDay monthDay;

    // Constructeurs, getters et setters

    public Event() {}

    public Event(String name, String description) {
        this.name = name;
        this.description = description;
    }

    // Getters et setters pour tous les champs
}
```

2. EventRepository.java

```java
package com.example.eventmanager.repository;

import com.example.eventmanager.model.Event;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;

import java.time.LocalDateTime;
import java.util.List;

@Repository
public interface EventRepository extends JpaRepository<Event, Long> {
    List<Event> findByLocalDateBetween(LocalDate start, LocalDate end);
    List<Event> findByLocalDateTimeBetween(LocalDateTime start, LocalDateTime end);
    List<Event> findByInstantBetween(Instant start, Instant end);
}
```

3. EventService.java

```java
package com.example.eventmanager.service;

import com.example.eventmanager.model.Event;
import com.example.eventmanager.repository.EventRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.time.*;
import java.util.List;

@Service
public class EventService {

    private final EventRepository eventRepository;

    @Autowired
    public EventService(EventRepository eventRepository) {
        this.eventRepository = eventRepository;
    }

    public Event createEvent(Event event) {
        return eventRepository.save(event);
    }

    public List<Event> getAllEvents() {
        return eventRepository.findAll();
    }

    public Event getEventById(Long id) {
        return eventRepository.findById(id).orElse(null);
    }

    public Event updateEvent(Long id, Event eventDetails) {
        Event event = eventRepository.findById(id).orElse(null);
        if (event != null) {
            event.setName(eventDetails.getName());
            event.setDescription(eventDetails.getDescription());
            event.setLocalDate(eventDetails.getLocalDate());
            event.setLocalTime(eventDetails.getLocalTime());
            event.setLocalDateTime(eventDetails.getLocalDateTime());
            event.setZonedDateTime(eventDetails.getZonedDateTime());
            event.setInstant(eventDetails.getInstant());
            event.setOffsetDateTime(eventDetails.getOffsetDateTime());
            event.setYearMonth(eventDetails.getYearMonth());
            event.setMonthDay(eventDetails.getMonthDay());
            return eventRepository.save(event);
        }
        return null;
    }

    public void deleteEvent(Long id) {
        eventRepository.deleteById(id);
    }

    public List<Event> getEventsByLocalDateRange(LocalDate start, LocalDate end) {
        return eventRepository.findByLocalDateBetween(start, end);
    }

    public List<Event> getEventsByLocalDateTimeRange(LocalDateTime start, LocalDateTime end) {
        return eventRepository.findByLocalDateTimeBetween(start, end);
    }

    public List<Event> getEventsByInstantRange(Instant start, Instant end) {
        return eventRepository.findByInstantBetween(start, end);
    }
}
```

4. EventController.java

```java
package com.example.eventmanager.controller;

import com.example.eventmanager.model.Event;
import com.example.eventmanager.service.EventService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.time.*;
import java.util.List;

@RestController
@RequestMapping("/api/events")
public class EventController {

    private final EventService eventService;

    @Autowired
    public EventController(EventService eventService) {
        this.eventService = eventService;
    }

    @PostMapping
    public ResponseEntity<Event> createEvent(@RequestBody Event event) {
        Event createdEvent = eventService.createEvent(event);
        return ResponseEntity.ok(createdEvent);
    }

    @GetMapping
    public ResponseEntity<List<Event>> getAllEvents() {
        List<Event> events = eventService.getAllEvents();
        return ResponseEntity.ok(events);
    }

    @GetMapping("/{id}")
    public ResponseEntity<Event> getEventById(@PathVariable Long id) {
        Event event = eventService.getEventById(id);
        return event != null ? ResponseEntity.ok(event) : ResponseEntity.notFound().build();
    }

    @PutMapping("/{id}")
    public ResponseEntity<Event> updateEvent(@PathVariable Long id, @RequestBody Event eventDetails) {
        Event updatedEvent = eventService.updateEvent(id, eventDetails);
        return updatedEvent != null ? ResponseEntity.ok(updatedEvent) : ResponseEntity.notFound().build();
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteEvent(@PathVariable Long id) {
        eventService.deleteEvent(id);
        return ResponseEntity.noContent().build();
    }

    @GetMapping("/by-local-date")
    public ResponseEntity<List<Event>> getEventsByLocalDateRange(
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE) LocalDate start,
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE) LocalDate end) {
        List<Event> events = eventService.getEventsByLocalDateRange(start, end);
        return ResponseEntity.ok(events);
    }

    @GetMapping("/by-local-date-time")
    public ResponseEntity<List<Event>> getEventsByLocalDateTimeRange(
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) LocalDateTime start,
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) LocalDateTime end) {
        List<Event> events = eventService.getEventsByLocalDateTimeRange(start, end);
        return ResponseEntity.ok(events);
    }

    @GetMapping("/by-instant")
    public ResponseEntity<List<Event>> getEventsByInstantRange(
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) Instant start,
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) Instant end) {
        List<Event> events = eventService.getEventsByInstantRange(start, end);
        return ResponseEntity.ok(events);
    }
}
```

5. application.properties

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

spring.jackson.serialization.write-dates-as-timestamps=false
```

6. pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.1.0</version>
        <relativePath/>
    </parent>
    <groupId>com.example</groupId>
    <artifactId>eventmanager</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>eventmanager</name>
    <description>Event Manager with Various Date Types</description>

    <properties>
        <java.version>17</java.version>
    </properties>

    <dependencies>
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
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

Ce projet inclut tous les types de dates mentionnés dans le modèle Event et fournit des opérations CRUD de base. Il offre également des méthodes pour rechercher des événements par plages de dates pour LocalDate, LocalDateTime et Instant. Vous pouvez étendre ces fonctionnalités pour inclure des recherches spécifiques pour d'autres types de dates si nécessaire.

N'oubliez pas d'ajouter la classe principale Spring Boot (EventManagerApplication.java) pour exécuter l'application.


--------------------------------
# Annexes :
--------------------------------

1. test1.http

```http
### Créer un nouvel événement
POST http://localhost:8080/api/events
Content-Type: application/json

{
  "name": "Conférence Spring Boot",
  "description": "Une conférence sur les meilleures pratiques Spring Boot"
}

### Récupérer tous les événements
GET http://localhost:8080/api/events

### Récupérer un événement par ID
GET http://localhost:8080/api/events/1

### Mettre à jour un événement
PUT http://localhost:8080/api/events/1
Content-Type: application/json

{
  "name": "Conférence Spring Boot 2023",
  "description": "Mise à jour de la conférence sur les meilleures pratiques Spring Boot"
}

### Ajouter une plage de dates à un événement
POST http://localhost:8080/api/events/1/add-date-range?start=2023-10-15T09:00:00&end=2023-10-15T17:00:00

### Supprimer une plage de dates d'un événement
DELETE http://localhost:8080/api/events/1/remove-date-range?start=2023-10-15T09:00:00&end=2023-10-15T17:00:00

### Récupérer les événements entre deux dates
GET http://localhost:8080/api/events/between?start=2023-10-01T00:00:00&end=2023-10-31T23:59:59

### Supprimer un événement
DELETE http://localhost:8080/api/events/1
```

2. README.md

```markdown
# Event Manager Application

Cette application Spring Boot permet de gérer des événements avec des plages de dates multiples.

## Prérequis

- Java 17
- Maven

## Configuration

Le projet utilise une base de données H2 en mémoire pour le développement. Vous pouvez modifier la configuration dans le fichier `application.properties` si nécessaire.

## Exécution

Pour exécuter l'application :

1. Clonez le dépôt
2. Naviguez vers le dossier du projet
3. Exécutez `mvn spring-boot:run`

L'application sera accessible à l'adresse `http://localhost:8080`.

## Endpoints API

- POST /api/events : Créer un nouvel événement
- GET /api/events : Récupérer tous les événements
- GET /api/events/{id} : Récupérer un événement par ID
- PUT /api/events/{id} : Mettre à jour un événement
- DELETE /api/events/{id} : Supprimer un événement
- GET /api/events/between : Récupérer les événements entre deux dates
- POST /api/events/{id}/add-date-range : Ajouter une plage de dates à un événement
- DELETE /api/events/{id}/remove-date-range : Supprimer une plage de dates d'un événement

## Tests

Vous pouvez utiliser le fichier `test1.http` pour tester les endpoints de l'API si votre IDE supporte les fichiers HTTP.
```

3. .gitignore

```
HELP.md
target/
!.mvn/wrapper/maven-wrapper.jar
!**/src/main/**/target/
!**/src/test/**/target/

### STS ###
.apt_generated
.classpath
.factorypath
.project
.settings
.springBeans
.sts4-cache

### IntelliJ IDEA ###
.idea
*.iws
*.iml
*.ipr

### NetBeans ###
/nbproject/private/
/nbbuild/
/dist/
/nbdist/
/.nb-gradle/
build/
!**/src/main/**/build/
!**/src/test/**/build/

### VS Code ###
.vscode/

### H2 Database ###
*.db
```

4. Schéma ASCII de l'organisation du projet

```
eventmanager/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── eventmanager/
│   │   │               ├── controller/
│   │   │               │   └── EventController.java
│   │   │               ├── model/
│   │   │               │   └── Event.java
│   │   │               ├── repository/
│   │   │               │   └── EventRepository.java
│   │   │               ├── service/
│   │   │               │   └── EventService.java
│   │   │               └── EventManagerApplication.java
│   │   │
│   │   └── resources/
│   │       └── application.properties
│   │
│   └── test/
│       └── java/
│           └── com/
│               └── example/
│                   └── eventmanager/
│                       └── EventManagerApplicationTests.java
│
├── pom.xml
├── README.md
├── .gitignore
└── test1.http
```

Ces fichiers supplémentaires complètent la structure du projet :

- Le fichier `test1.http` permet de tester facilement les endpoints de l'API.
- Le `README.md` fournit des informations sur le projet et comment l'exécuter.
- Le `.gitignore` est utile si vous utilisez Git pour le contrôle de version.
- Le schéma ASCII donne une vue d'ensemble de la structure du projet.

Avec ces ajouts, vous avez maintenant une structure de projet complète et des outils pour tester et documenter votre application Spring Boot de gestion d'événements.



