# Exemples:


--------------------------------
# 1 - Rappel des deux classes **Customer** et **Card** avec les relations de clé primaire (PK) et clé étrangère (FK) :
--------------------------------

## Relation entre un client et ses cartes, où un client peut avoir plusieurs cartes.

```
+--------------------------+             +--------------------------+
|        Customer           |             |          Card             |
+--------------------------+             +--------------------------+
| - customerId (PK)         | <-----------| - cardId (PK)             |
| - customerFirstName       |             | - customerId (FK)         |
| - customerLastName        |             | - cardNumber              |
+--------------------------+             | - cardType                |
                                          | - totalLimit              |
                                          | - amountUsed              |
                                          | - availableAmount         |
                                          | - createDt                |
                                          +--------------------------+
```

- **Customer** : La classe `Customer` contient `customerId` comme clé primaire (PK).
- **Card** : La classe `Card` contient `cardId` comme clé primaire (PK) et `customerId` comme clé étrangère (FK) qui fait référence à `Customer`.

### 1. **Ajouter un client (POST /api/v1/customers/add)**

Dans cet exemple, on crée un nouveau client, et `ResponseEntity` est utilisé pour renvoyer une réponse appropriée en fonction de l'état de la création.

#### Code (CustomerController.java) :
```java
@RestController
@RequestMapping("/api/v1/customers")
public class CustomerController {

    @Autowired
    private CustomerService customerService;

    @PostMapping("/add")
    public ResponseEntity<Customer> addCustomer(@RequestBody Customer customer) {
        if (customerService.existsByFirstNameAndLastName(customer.getCustomerFirstName(), customer.getCustomerLastName())) {
            return ResponseEntity.status(HttpStatus.CONFLICT)
                    .body(null); // Le client existe déjà, renvoie un conflit (409)
        }

        Customer createdCustomer = customerService.save(customer);
        return ResponseEntity.status(HttpStatus.CREATED)
                .body(createdCustomer); // Le client a été créé, renvoie un statut 201
    }
}
```

#### Explication :
- **`HttpStatus.CREATED` (201)** : Utilisé pour indiquer que la ressource (client) a été créée avec succès.
- **`HttpStatus.CONFLICT` (409)** : Utilisé pour indiquer un conflit, par exemple si un client avec les mêmes informations existe déjà.

### 2. **Récupérer un client par ID (GET /api/v1/customers/{id})**

Cet exemple montre comment renvoyer un client spécifique ou gérer le cas où le client n'existe pas.

#### Code (CustomerController.java) :
```java
@GetMapping("/{id}")
public ResponseEntity<Customer> getCustomerById(@PathVariable Long id) {
    Optional<Customer> customer = customerService.findById(id);

    if (customer.isPresent()) {
        return ResponseEntity.ok(customer.get()); // Le client existe, statut 200 OK
    } else {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
                .body(null); // Le client n'existe pas, renvoie un statut 404
    }
}
```

#### Explication :
- **`ResponseEntity.ok()`** : Utilisé pour renvoyer une réponse avec un statut HTTP 200 (OK) et le corps de la réponse.
- **`HttpStatus.NOT_FOUND` (404)** : Utilisé pour indiquer que la ressource demandée (client) n'a pas été trouvée.

### 3. **Récupérer tous les clients (GET /api/v1/customers)**

Si la liste des clients est vide, une réponse avec le statut **204 No Content** est renvoyée.

#### Code (CustomerController.java) :
```java
@GetMapping
public ResponseEntity<List<Customer>> getAllCustomers() {
    List<Customer> customers = customerService.findAll();

    if (customers.isEmpty()) {
        return ResponseEntity.noContent().build(); // Aucun client, statut 204 No Content
    }

    return ResponseEntity.ok(customers); // Des clients existent, renvoie un statut 200 OK
}
```

#### Explication :
- **`ResponseEntity.noContent().build()`** : Renvoie une réponse vide avec le statut HTTP 204 (No Content).
- **`ResponseEntity.ok()`** : Renvoie une réponse contenant la liste des clients avec un statut 200 (OK).

### 4. **Ajouter une carte à un client (POST /api/v1/cards/add)**

On vérifie d'abord que le client existe avant d'ajouter une carte. Si le client n'existe pas ou si le numéro de carte est déjà utilisé, des erreurs sont renvoyées.

#### Code (CardController.java) :
```java
@RestController
@RequestMapping("/api/v1/cards")
public class CardController {

    @Autowired
    private CardService cardService;

    @Autowired
    private CustomerService customerService;

    @PostMapping("/add")
    public ResponseEntity<Card> addCard(@RequestBody Card card) {
        if (!customerService.existsById(card.getCustomerId())) {
            return ResponseEntity.status(HttpStatus.BAD_REQUEST)
                    .body(null); // Le client n'existe pas, statut 400 Bad Request
        }

        if (cardService.existsByCardNumber(card.getCardNumber())) {
            return ResponseEntity.status(HttpStatus.CONFLICT)
                    .body(null); // Le numéro de carte existe déjà, statut 409 Conflict
        }

        Card createdCard = cardService.save(card);
        return ResponseEntity.status(HttpStatus.CREATED)
                .body(createdCard); // La carte a été créée avec succès, statut 201
    }
}
```

#### Explication :
- **`HttpStatus.BAD_REQUEST` (400)** : Utilisé si le client n'existe pas, car la demande est invalide.
- **`HttpStatus.CONFLICT` (409)** : Utilisé si le numéro de carte est déjà associé à une autre carte, pour indiquer un conflit.

### 5. **Supprimer un client (DELETE /api/v1/customers/delete/{id})**

Avant de supprimer un client, il faut vérifier qu'il n'a pas de cartes associées. Si des cartes existent, la suppression échoue avec un message d'erreur.

#### Code (CustomerController.java) :
```java
@DeleteMapping("/delete/{id}")
public ResponseEntity<String> deleteCustomer(@PathVariable Long id) {
    if (!customerService.existsById(id)) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
                .body("Customer not found"); // Le client n'existe pas, statut 404
    }

    if (cardService.existsByCustomerId(id)) {
        return ResponseEntity.status(HttpStatus.CONFLICT)
                .body("Cannot delete customer with associated cards"); // Le client a des cartes, statut 409 Conflict
    }

    customerService.deleteById(id);
    return ResponseEntity.ok("Customer with ID " + id + " successfully deleted"); // Suppression réussie, statut 200 OK
}
```

#### Explication :
- **`HttpStatus.CONFLICT` (409)** : Utilisé pour éviter la suppression d'un client avec des cartes associées.
- **`HttpStatus.NOT_FOUND` (404)** : Utilisé si le client n'existe pas.

### 6. **Supprimer une carte par ID (DELETE /api/v1/cards/delete/{id})**

Cet exemple montre comment supprimer une carte par son identifiant, avec gestion des cas d'erreurs.

#### Code (CardController.java) :
```java
@DeleteMapping("/delete/{id}")
public ResponseEntity<String> deleteCard(@PathVariable Long id) {
    if (!cardService.existsById(id)) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
                .body("Card not found"); // La carte n'existe pas, statut 404
    }

    cardService.deleteById(id);
    return ResponseEntity.ok("Card with ID " + id + " successfully deleted"); // Suppression réussie, statut 200 OK
}
```

#### Explication :
- **`HttpStatus.NOT_FOUND` (404)** : Utilisé si la carte à supprimer n'existe pas.

### 7. **Mettre à jour un client (PUT /api/v1/customers/update/{id})**

Un exemple pour mettre à jour un client avec vérification de l'existence de celui-ci.

#### Code (CustomerController.java) :
```java
@PutMapping("/update/{id}")
public ResponseEntity<Customer> updateCustomer(@PathVariable Long id, @RequestBody Customer updatedCustomer) {
    if (!customerService.existsById(id)) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
                .body(null); // Le client n'existe pas, statut 404
    }

    Customer customer = customerService.update(id, updatedCustomer);
    return ResponseEntity.ok(customer); // Mise à jour réussie, statut 200 OK
}
```

#### Explication :
- **`HttpStatus.NOT_FOUND` (404)** : Utilisé si le client n'existe pas.

---

### Conclusion

L'utilisation de `ResponseEntity` est cruciale dans les contrôleurs Spring Boot pour gérer les réponses HTTP de manière claire et flexible. Les exemples ci-dessus montrent comment utiliser différents codes de statut HTTP (200, 201, 404, 409) et personnaliser les réponses en fonction des conditions.
