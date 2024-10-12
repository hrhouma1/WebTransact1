------------------------------
# Dates 
------------------------------

1. LocalDate
```java
LocalDate date = LocalDate.now();
LocalDate specificDate = LocalDate.of(2023, 10, 15);
```
LocalDate représente une date sans heure ni fuseau horaire.

2. LocalTime
```java
LocalTime time = LocalTime.now();
LocalTime specificTime = LocalTime.of(14, 30, 45);
```
LocalTime représente une heure sans date ni fuseau horaire.

3. LocalDateTime
```java
LocalDateTime dateTime = LocalDateTime.now();
LocalDateTime specificDateTime = LocalDateTime.of(2023, 10, 15, 14, 30, 45);
```
LocalDateTime combine LocalDate et LocalTime.

4. ZonedDateTime
```java
ZonedDateTime zonedDateTime = ZonedDateTime.now();
ZonedDateTime specificZonedDateTime = ZonedDateTime.of(2023, 10, 15, 14, 30, 45, 0, ZoneId.of("Europe/Paris"));
```
ZonedDateTime inclut la date, l'heure et le fuseau horaire.

5. Instant
```java
Instant instant = Instant.now();
```
Instant représente un point précis sur la ligne du temps, généralement en UTC.

6. OffsetDateTime
```java
OffsetDateTime offsetDateTime = OffsetDateTime.now();
OffsetDateTime specificOffsetDateTime = OffsetDateTime.of(2023, 10, 15, 14, 30, 45, 0, ZoneOffset.ofHours(2));
```
OffsetDateTime est similaire à ZonedDateTime mais utilise un décalage fixe par rapport à UTC.

7. YearMonth
```java
YearMonth yearMonth = YearMonth.now();
YearMonth specificYearMonth = YearMonth.of(2023, 10);
```
YearMonth représente un mois spécifique d'une année spécifique.

8. MonthDay
```java
MonthDay monthDay = MonthDay.now();
MonthDay specificMonthDay = MonthDay.of(10, 15);
```
MonthDay représente une combinaison d'un mois et d'un jour, sans année.

Ces classes font partie du package java.time et offrent une API plus robuste et plus facile à utiliser que les anciennes classes java.util.Date et java.util.Calendar. Elles sont immuables et thread-safe, ce qui les rend plus sûres à utiliser dans des applications concurrentes.

# Citations:

[1] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/19265956/b8bd0550-d92e-4a04-a484-bdd3eb078b58/paste.txt

[2] https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/19265956/f9d7a5cb-833a-49c9-98f7-88bd7709f662/paste-2.txt

[3] https://dev.java/learn/date-time/

[4] https://thorben-janssen.com/hibernate-jpa-date-and-time/

[5] https://www.digitalocean.com/community/tutorials/java-simpledateformat-java-date-format

[6] https://blog.mimacom.com/java-8-dates-with-postgresql/

[7] https://vladmihalcea.com/date-timestamp-jpa-hibernate/

[8] https://www.influxdata.com/blog/java-date-format-guide-influxdb/

[9] https://www.codecademy.com/resources/docs/java/date

[10] https://www.geeksforgeeks.org/date-class-java-examples/
