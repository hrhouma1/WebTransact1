------------------------------
# Exemple à réalsier
------------------------------

## Structure du projet

```
src/main/java/com/example/eventmanager
├── EventManagerApplication.java
├── controller
│   └── EventController.java
├── model
│   └── Event.java
├── repository
│   └── EventRepository.java
└── service
    └── EventService.java
```

## Entité (Event.java)

```java
package com.example.eventmanager.model;

import jakarta.persistence.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "events")
public class Event {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(nullable = false)
    private LocalDateTime startDateTime;

    @Column(nullable = false)
    private LocalDateTime endDateTime;

    private String description;

    // Constructeurs, getters et setters

    public Event() {}

    public Event(String name, LocalDateTime startDateTime, LocalDateTime endDateTime, String description) {
        this.name = name;
        this.startDateTime = startDateTime;
        this.endDateTime = endDateTime;
        this.description = description;
    }

    // Getters et setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public LocalDateTime getStartDateTime() {
        return startDateTime;
    }

    public void setStartDateTime(LocalDateTime startDateTime) {
        this.startDateTime = startDateTime;
    }

    public LocalDateTime getEndDateTime() {
        return endDateTime;
    }

    public void setEndDateTime(LocalDateTime endDateTime) {
        this.endDateTime = endDateTime;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }
}
```

## Repository (EventRepository.java)

```java
package com.example.eventmanager.repository;

import com.example.eventmanager.model.Event;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.time.LocalDateTime;
import java.util.List;

@Repository
public interface EventRepository extends JpaRepository<Event, Long> {
    List<Event> findByStartDateTimeBetween(LocalDateTime start, LocalDateTime end);
}
```

## Service (EventService.java)

```java
package com.example.eventmanager.service;

import com.example.eventmanager.model.Event;
import com.example.eventmanager.repository.EventRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.time.LocalDateTime;
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

    public List<Event> getEventsBetweenDates(LocalDateTime start, LocalDateTime end) {
        return eventRepository.findByStartDateTimeBetween(start, end);
    }

    public Event updateEvent(Long id, Event eventDetails) {
        Event event = eventRepository.findById(id).orElse(null);
        if (event != null) {
            event.setName(eventDetails.getName());
            event.setStartDateTime(eventDetails.getStartDateTime());
            event.setEndDateTime(eventDetails.getEndDateTime());
            event.setDescription(eventDetails.getDescription());
            return eventRepository.save(event);
        }
        return null;
    }

    public void deleteEvent(Long id) {
        eventRepository.deleteById(id);
    }
}
```

## Contrôleur (EventController.java)

```java
package com.example.eventmanager.controller;

import com.example.eventmanager.model.Event;
import com.example.eventmanager.service.EventService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.time.LocalDateTime;
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
        if (event != null) {
            return ResponseEntity.ok(event);
        }
        return ResponseEntity.notFound().build();
    }

    @GetMapping("/between")
    public ResponseEntity<List<Event>> getEventsBetweenDates(
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) LocalDateTime start,
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) LocalDateTime end) {
        List<Event> events = eventService.getEventsBetweenDates(start, end);
        return ResponseEntity.ok(events);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Event> updateEvent(@PathVariable Long id, @RequestBody Event eventDetails) {
        Event updatedEvent = eventService.updateEvent(id, eventDetails);
        if (updatedEvent != null) {
            return ResponseEntity.ok(updatedEvent);
        }
        return ResponseEntity.notFound().build();
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteEvent(@PathVariable Long id) {
        eventService.deleteEvent(id);
        return ResponseEntity.noContent().build();
    }
}
```

## Diagramme de l'entité Event

```
+-------------------+
|       Event       |
+-------------------+
| - id: Long        |
| - name: String    |
| - startDateTime:  |
|   LocalDateTime   |
| - endDateTime:    |
|   LocalDateTime   |
| - description:    |
|   String          |
+-------------------+
| + getId()         |
| + setId()         |
| + getName()       |
| + setName()       |
| + getStartDateTime|
| + setStartDateTime|
| + getEndDateTime()|
| + setEndDateTime()|
| + getDescription()|
| + setDescription()|
+-------------------+
```

Ce code complet fournit une implémentation de base pour gérer des événements avec des dates dans une application Spring Boot. Voici quelques points importants :

1. L'entité `Event` utilise `LocalDateTime` pour les dates de début et de fin.
2. Le repository `EventRepository` étend `JpaRepository` et inclut une méthode personnalisée pour rechercher des événements entre deux dates.
3. Le service `EventService` encapsule la logique métier et interagit avec le repository.
4. Le contrôleur `EventController` expose des endpoints RESTful pour créer, lire, mettre à jour et supprimer des événements, ainsi que pour rechercher des événements entre deux dates.


----------------------------------------------
# Annexe : fichiers de configurations
----------------------------------------------



## Application principale (EventManagerApplication.java)

```java
package com.example.eventmanager;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class EventManagerApplication {

    public static void main(String[] args) {
        SpringApplication.run(EventManagerApplication.class, args);
    }
}
```

## Configuration (application.properties)

```properties
# Configuration du serveur
server.port=8080

# Configuration de la base de données (utilisant H2 pour cet exemple)
spring.datasource.url=jdbc:h2:mem:eventdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

# Configuration de JPA/Hibernate
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# Configuration du fuseau horaire pour Hibernate
spring.jpa.properties.hibernate.jdbc.time_zone=UTC

# Activation de la console H2 (utile pour le développement)
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

# Configuration du logging
logging.level.org.springframework=INFO
logging.level.com.example.eventmanager=DEBUG

# Configuration de Jackson pour la sérialisation des dates
spring.jackson.serialization.write-dates-as-timestamps=false
spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
```

## Fichier pom.xml (pour Maven)

Voici un exemple de fichier `pom.xml` qui inclut toutes les dépendances nécessaires :

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
    <description>Event Manager Application</description>

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

## Explications supplémentaires

1. **Base de données** : Dans cet exemple, nous utilisons H2, une base de données en mémoire, ce qui est pratique pour le développement et les tests. Pour un environnement de production, vous devriez configurer une base de données persistante comme MySQL ou PostgreSQL.

2. **Configuration JPA/Hibernate** : 
   - `spring.jpa.hibernate.ddl-auto=update` permet à Hibernate de mettre à jour automatiquement le schéma de la base de données.
   - `spring.jpa.show-sql=true` affiche les requêtes SQL générées par Hibernate.

3. **Fuseau horaire** : `spring.jpa.properties.hibernate.jdbc.time_zone=UTC` configure Hibernate pour utiliser UTC, ce qui est une bonne pratique pour gérer les dates.

4. **Console H2** : Activée pour le développement, vous pouvez accéder à `http://localhost:8080/h2-console` pour voir et interagir avec votre base de données.

5. **Logging** : Configuré pour afficher les logs de Spring au niveau INFO et les logs de votre application au niveau DEBUG.

6. **Configuration Jackson** : Configuré pour formater les dates correctement lors de la sérialisation/désérialisation JSON.

Pour utiliser cette configuration :

1. Assurez-vous que tous les fichiers Java sont dans le package correct (`com.example.eventmanager`).
2. Placez le fichier `application.properties` dans le dossier `src/main/resources`.
3. Si vous utilisez Maven, placez le fichier `pom.xml` à la racine de votre projet.

- Avec cette configuration, vous devriez pouvoir exécuter votre application Spring Boot et commencer à utiliser les endpoints pour gérer vos événements. 
- L'application utilisera une base de données H2 en mémoire, ce qui est parfait pour le développement et les tests.
