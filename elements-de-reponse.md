
## 1. POST /api/v1/customers/add

Modifiez votre `CustomerController` pour ajouter cette méthode :

```java
@RestController
@RequestMapping("/api/v1/customers")
public class CustomerController {

    @Autowired
    private CustomerRepository customerRepository;

    @PostMapping("/add")
    public ResponseEntity<Customer> addCustomer(@RequestBody Customer customer) {
        Customer savedCustomer = customerRepository.save(customer);
        return ResponseEntity.ok(savedCustomer);
    }
}
```

## 2. GET /api/v1/customers/{id}

Ajoutez cette méthode à `CustomerController` :

```java
@GetMapping("/{id}")
public ResponseEntity<Customer> getCustomerById(@PathVariable int id) {
    return customerRepository.findById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
}
```

## 3. GET /api/v1/customers

Ajoutez cette méthode à `CustomerController` :

```java
@GetMapping
public ResponseEntity<List<Customer>> getAllCustomers() {
    List<Customer> customers = customerRepository.findAll();
    if (customers.isEmpty()) {
        return ResponseEntity.noContent().build();
    }
    return ResponseEntity.ok(customers);
}
```

## 4. POST /api/v1/cards/add

Créez un `CardController` et ajoutez cette méthode :

```java
@RestController
@RequestMapping("/api/v1/cards")
public class CardController {

    @Autowired
    private CardRepository cardRepository;

    @Autowired
    private CustomerRepository customerRepository;

    @PostMapping("/add")
    public ResponseEntity<Card> addCard(@RequestBody Card card) {
        if (!customerRepository.existsById(card.getCustomer().getCustomerId())) {
            return ResponseEntity.badRequest().build();
        }
        if (cardRepository.existsByCardNumber(card.getCardNumber())) {
            return ResponseEntity.badRequest().build();
        }
        Card savedCard = cardRepository.save(card);
        return ResponseEntity.ok(savedCard);
    }
}
```

## 5. GET /api/v1/cards/{id}

Ajoutez cette méthode à `CardController` :

```java
@GetMapping("/{id}")
public ResponseEntity<Card> getCardById(@PathVariable int id) {
    return cardRepository.findById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
}
```

## 6. GET /api/v1/cards

Ajoutez cette méthode à `CardController` :

```java
@GetMapping
public ResponseEntity<List<Card>> getAllCards() {
    List<Card> cards = cardRepository.findAll();
    if (cards.isEmpty()) {
        return ResponseEntity.noContent().build();
    }
    return ResponseEntity.ok(cards);
}
```

## 7. PUT /api/v1/customers/update/{id}

Ajoutez cette méthode à `CustomerController` :

```java
@PutMapping("/update/{id}")
public ResponseEntity<Customer> updateCustomer(@PathVariable int id, @RequestBody Customer customerDetails) {
    return customerRepository.findById(id)
            .map(customer -> {
                customer.setCustomerFirstName(customerDetails.getCustomerFirstName());
                customer.setCustomerLastName(customerDetails.getCustomerLastName());
                return ResponseEntity.ok(customerRepository.save(customer));
            })
            .orElse(ResponseEntity.notFound().build());
}
```

## 8. PUT /api/v1/cards/update/{id}

Ajoutez cette méthode à `CardController` :

```java
@PutMapping("/update/{id}")
public ResponseEntity<Card> updateCard(@PathVariable int id, @RequestBody Card cardDetails) {
    return cardRepository.findById(id)
            .map(card -> {
                if (!customerRepository.existsById(cardDetails.getCustomer().getCustomerId())) {
                    return ResponseEntity.badRequest().build();
                }
                card.setCustomer(cardDetails.getCustomer());
                card.setCardNumber(cardDetails.getCardNumber());
                card.setCardType(cardDetails.getCardType());
                card.setTotalLimit(cardDetails.getTotalLimit());
                card.setAmountUsed(cardDetails.getAmountUsed());
                card.setAvailableAmount(cardDetails.getAvailableAmount());
                card.setCreateDt(cardDetails.getCreateDt());
                return ResponseEntity.ok(cardRepository.save(card));
            })
            .orElse(ResponseEntity.notFound().build());
}
```

## 9. DELETE /api/v1/customers/delete/{id}

Ajoutez cette méthode à `CustomerController` :

```java
@DeleteMapping("/delete/{id}")
public ResponseEntity<?> deleteCustomer(@PathVariable int id) {
    return customerRepository.findById(id)
            .map(customer -> {
                if (!customer.getCards().isEmpty()) {
                    return ResponseEntity.badRequest().body("Cannot delete customer with associated cards");
                }
                customerRepository.delete(customer);
                return ResponseEntity.ok().build();
            })
            .orElse(ResponseEntity.notFound().build());
}
```

## 10. DELETE /api/v1/cards/delete/{id}

Ajoutez cette méthode à `CardController` :

```java
@DeleteMapping("/delete/{id}")
public ResponseEntity<?> deleteCard(@PathVariable int id) {
    return cardRepository.findById(id)
            .map(card -> {
                cardRepository.delete(card);
                return ResponseEntity.ok().build();
            })
            .orElse(ResponseEntity.notFound().build());
}
```

## 11. GET /api/v1/cards/number/{cardNumber}

Ajoutez cette méthode à `CardController` :

```java
@GetMapping("/number/{cardNumber}")
public ResponseEntity<Card> getCardByNumber(@PathVariable String cardNumber) {
    return cardRepository.findByCardNumber(cardNumber)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
}
```

## 12. DELETE /api/v1/cards/delete/number/{cardNumber}

Ajoutez cette méthode à `CardController` :

```java
@DeleteMapping("/delete/number/{cardNumber}")
public ResponseEntity<?> deleteCardByNumber(@PathVariable String cardNumber) {
    return cardRepository.findByCardNumber(cardNumber)
            .map(card -> {
                cardRepository.delete(card);
                return ResponseEntity.ok().build();
            })
            .orElse(ResponseEntity.notFound().build());
}
```

## 13. DELETE /api/v1/customers

Ajoutez cette méthode à `CustomerController` :

```java
@DeleteMapping
public ResponseEntity<?> deleteAllCustomers() {
    customerRepository.deleteAll();
    return ResponseEntity.ok().build();
}
```

## 14. DELETE /api/v1/cards

Ajoutez cette méthode à `CardController` :

```java
@DeleteMapping
public ResponseEntity<?> deleteAllCards() {
    cardRepository.deleteAll();
    return ResponseEntity.ok().build();
}
```

N'oubliez pas d'ajouter les méthodes nécessaires dans vos repositories :

```java
public interface CardRepository extends JpaRepository<Card, Integer> {
    boolean existsByCardNumber(String cardNumber);
    Optional<Card> findByCardNumber(String cardNumber);
}
```

Ces implémentations couvrent tous les endpoints demandés dans le cahier des charges. Assurez-vous d'avoir les dépendances nécessaires dans votre `pom.xml` et que vos entités `Customer` et `Card` sont correctement définies avec les relations JPA appropriées.

