# Suite pratique 02



1. Event.java
```java
package com.example.eventmanager.model;

import jakarta.persistence.*;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

@Entity
@Table(name = "events")
public class Event {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @ElementCollection
    @CollectionTable(name = "event_date_ranges", joinColumns = @JoinColumn(name = "event_id"))
    private List<DateRange> dateRanges = new ArrayList<>();

    private String description;

    // Constructeurs, getters et setters

    public Event() {}

    public Event(String name, String description) {
        this.name = name;
        this.description = description;
    }

    // Getters et setters

    public void addDateRange(LocalDateTime start, LocalDateTime end) {
        this.dateRanges.add(new DateRange(start, end));
    }

    public void removeDateRange(LocalDateTime start, LocalDateTime end) {
        this.dateRanges.removeIf(range -> range.getStart().equals(start) && range.getEnd().equals(end));
    }

    @Embeddable
    public static class DateRange {
        private LocalDateTime start;
        private LocalDateTime end;

        public DateRange() {}

        public DateRange(LocalDateTime start, LocalDateTime end) {
            this.start = start;
            this.end = end;
        }

        // Getters et setters
    }
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
    @Query("SELECT e FROM Event e JOIN e.dateRanges dr WHERE dr.start <= :end AND dr.end >= :start")
    List<Event> findEventsBetweenDates(LocalDateTime start, LocalDateTime end);
}
```

3. EventService.java
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
        return eventRepository.findEventsBetweenDates(start, end);
    }

    public Event updateEvent(Long id, Event eventDetails) {
        Event event = eventRepository.findById(id).orElse(null);
        if (event != null) {
            event.setName(eventDetails.getName());
            event.setDescription(eventDetails.getDescription());
            return eventRepository.save(event);
        }
        return null;
    }

    public Event addDateRangeToEvent(Long id, LocalDateTime start, LocalDateTime end) {
        Event event = eventRepository.findById(id).orElse(null);
        if (event != null) {
            event.addDateRange(start, end);
            return eventRepository.save(event);
        }
        return null;
    }

    public Event removeDateRangeFromEvent(Long id, LocalDateTime start, LocalDateTime end) {
        Event event = eventRepository.findById(id).orElse(null);
        if (event != null) {
            event.removeDateRange(start, end);
            return eventRepository.save(event);
        }
        return null;
    }

    public void deleteEvent(Long id) {
        eventRepository.deleteById(id);
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

    @GetMapping("/between")
    public ResponseEntity<List<Event>> getEventsBetweenDates(
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) LocalDateTime start,
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) LocalDateTime end) {
        List<Event> events = eventService.getEventsBetweenDates(start, end);
        return ResponseEntity.ok(events);
    }

    @PostMapping("/{id}/add-date-range")
    public ResponseEntity<Event> addDateRange(
            @PathVariable Long id,
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) LocalDateTime start,
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) LocalDateTime end) {
        Event updatedEvent = eventService.addDateRangeToEvent(id, start, end);
        return updatedEvent != null ? ResponseEntity.ok(updatedEvent) : ResponseEntity.notFound().build();
    }

    @DeleteMapping("/{id}/remove-date-range")
    public ResponseEntity<Event> removeDateRange(
            @PathVariable Long id,
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) LocalDateTime start,
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME) LocalDateTime end) {
        Event updatedEvent = eventService.removeDateRangeFromEvent(id, start, end);
        return updatedEvent != null ? ResponseEntity.ok(updatedEvent) : ResponseEntity.notFound().build();
    }
}
```

5. EventManagerApplication.java
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

6. application.properties
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

7. pom.xml
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

Cette implémentation offre une gestion plus flexible des dates d'événements, permettant à chaque événement d'avoir plusieurs plages de dates. Les principales modifications incluent :

1. Modification de l'entité `Event` pour utiliser une liste de `DateRange`.
2. Ajout de méthodes dans `EventRepository` pour rechercher des événements entre deux dates.
3. Mise à jour de `EventService` avec des méthodes pour ajouter et supprimer des plages de dates.
4. Ajout de nouveaux endpoints dans `EventController` pour gérer les plages de dates.

Ces changements permettent une gestion plus robuste des événements avec des dates multiples ou récurrentes.
