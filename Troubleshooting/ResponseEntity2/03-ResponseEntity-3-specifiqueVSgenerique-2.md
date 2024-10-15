### Je vous présente des exemples concrets de contrôleurs avec des cas d'utilisation réels, en montrant quand utiliser des méthodes génériques ou spécifiques de ResponseEntity.

1. Contrôleur pour une API de gestion de cartes bancaires (Card):

```java
@RestController
@RequestMapping("/api/cards")
public class CardController {

    @Autowired
    private CardService cardService;

    // Méthode spécifique pour un cas simple
    @GetMapping("/{id}")
    public ResponseEntity<Card> getCard(@PathVariable Long id) {
        Card card = cardService.findById(id);
        return ResponseEntity.ok(card);
    }

    // Méthode générique pour un cas plus complexe
    @PostMapping
    public ResponseEntity<Card> createCard(@RequestBody Card card) {
        Card createdCard = cardService.create(card);
        return ResponseEntity
                .status(HttpStatus.CREATED)
                .header("X-Card-Id", createdCard.getId().toString())
                .body(createdCard);
    }

    // Méthode générique pour gérer différents cas
    @PutMapping("/{id}/status")
    public ResponseEntity<?> updateCardStatus(@PathVariable Long id, @RequestBody CardStatus status) {
        try {
            Card updatedCard = cardService.updateStatus(id, status);
            return ResponseEntity.ok(updatedCard);
        } catch (CardNotFoundException e) {
            return ResponseEntity.notFound().build();
        } catch (InvalidStatusException e) {
            return ResponseEntity
                    .badRequest()
                    .body("Invalid status: " + e.getMessage());
        }
    }
}
```

2. Contrôleur pour une API de gestion de pharmacies:

```java
@RestController
@RequestMapping("/api/pharmacies")
public class PharmacyController {

    @Autowired
    private PharmacyService pharmacyService;

    // Méthode spécifique pour un cas simple
    @GetMapping("/nearby")
    public ResponseEntity<List<Pharmacy>> getNearbyPharmacies(
            @RequestParam double latitude,
            @RequestParam double longitude) {
        List<Pharmacy> pharmacies = pharmacyService.findNearby(latitude, longitude);
        return ResponseEntity.ok(pharmacies);
    }

    // Méthode générique pour un cas plus complexe
    @PostMapping("/{id}/inventory")
    public ResponseEntity<?> updateInventory(
            @PathVariable Long id,
            @RequestBody InventoryUpdate update) {
        try {
            Pharmacy pharmacy = pharmacyService.updateInventory(id, update);
            return ResponseEntity
                    .ok()
                    .header("X-Inventory-Updated", "true")
                    .body(pharmacy);
        } catch (PharmacyNotFoundException e) {
            return ResponseEntity.notFound().build();
        } catch (InvalidInventoryUpdateException e) {
            return ResponseEntity
                    .badRequest()
                    .body("Invalid inventory update: " + e.getMessage());
        }
    }
}
```

3. Contrôleur pour une API de gestion de médecins:

```java
@RestController
@RequestMapping("/api/doctors")
public class DoctorController {

    @Autowired
    private DoctorService doctorService;

    // Méthode spécifique pour un cas simple
    @GetMapping("/speciality/{speciality}")
    public ResponseEntity<List<Doctor>> getDoctorsBySpeciality(@PathVariable String speciality) {
        List<Doctor> doctors = doctorService.findBySpeciality(speciality);
        return ResponseEntity.ok(doctors);
    }

    // Méthode générique pour un cas plus complexe
    @PostMapping("/{id}/appointments")
    public ResponseEntity<?> bookAppointment(
            @PathVariable Long id,
            @RequestBody AppointmentRequest request) {
        try {
            Appointment appointment = doctorService.bookAppointment(id, request);
            return ResponseEntity
                    .status(HttpStatus.CREATED)
                    .header("X-Appointment-Id", appointment.getId().toString())
                    .body(appointment);
        } catch (DoctorNotFoundException e) {
            return ResponseEntity.notFound().build();
        } catch (AppointmentConflictException e) {
            return ResponseEntity
                    .status(HttpStatus.CONFLICT)
                    .body("Appointment conflict: " + e.getMessage());
        }
    }

    // Méthode générique pour gérer différents cas de réponse
    @GetMapping("/{id}/availability")
    public ResponseEntity<?> getDoctorAvailability(
            @PathVariable Long id,
            @RequestParam @DateTimeFormat(iso = DateTimeFormat.ISO.DATE) LocalDate date) {
        try {
            List<TimeSlot> availableSlots = doctorService.getAvailability(id, date);
            if (availableSlots.isEmpty()) {
                return ResponseEntity
                        .noContent()
                        .header("X-No-Availability", "true")
                        .build();
            }
            return ResponseEntity.ok(availableSlots);
        } catch (DoctorNotFoundException e) {
            return ResponseEntity.notFound().build();
        }
    }
}
```

Dans ces exemples :

1. Les méthodes spécifiques (comme `ResponseEntity.ok()`) sont utilisées pour des cas simples où seul le corps de la réponse et un code de statut standard sont nécessaires.

2. Les méthodes génériques (comme `ResponseEntity.status().header().body()`) sont utilisées pour des cas plus complexes où vous devez personnaliser davantage la réponse, par exemple en ajoutant des en-têtes personnalisés ou en gérant différents types de réponses selon les cas.

3. Les méthodes génériques sont également utiles lorsque vous devez gérer différents scénarios d'erreur et retourner différents types de réponses en fonction de l'exception levée.

En général, utilisez les méthodes spécifiques pour des cas simples et standards, et passez aux méthodes génériques lorsque vous avez besoin de plus de flexibilité ou de personnalisation dans vos réponses HTTP.

# Citations:

[1] https://www.geeksforgeeks.org/how-to-use-spring-responseentity-to-manipulate-the-http-response/

[2] https://stackoverflow.com/questions/26549379/when-use-responseentityt-and-restcontroller-for-spring-restful-applications/26550981

[3] https://jpa-buddy.com/guides/spring-boot-restful-crud-application-with-intellij-idea-and-jpa-buddy/

[4] https://spring.io/guides/gs/accessing-data-jpa/

[5] https://docs.spring.io/spring-data/data-commons/docs/1.6.1.RELEASE/reference/html/repositories.html

[6] https://technicalsand.com/using-responseentity-in-spring/
