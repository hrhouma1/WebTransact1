
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


-----------------------------
# Annexe 1 
-----------------------------

- Quels sont les avantages spécifiques d'ajouter les méthodes existsByCardNumber et findByCardNumber dans l'interface CardRepository, et comment ces méthodes améliorent-elles la performance et la fonctionnalité de l'application ?

### Réponse : 

L'ajout de ces deux méthodes spécifiques dans votre interface `CardRepository` a des objectifs précis et offre des avantages en termes de performance et de fonctionnalité :

1. `boolean existsByCardNumber(String cardNumber);`

Cette méthode est utilisée pour vérifier rapidement si une carte avec un numéro spécifique existe dans la base de données, sans avoir à récupérer l'entité complète. Voici pourquoi c'est utile :

- **Performance** : Elle génère une requête SQL optimisée qui utilise généralement `SELECT 1 FROM ... LIMIT 1` au lieu de récupérer toutes les colonnes de l'entité.
- **Simplicité** : Elle retourne directement un booléen, ce qui simplifie les vérifications d'existence dans votre code.
- **Validation** : Utile pour valider l'unicité d'un numéro de carte avant d'en créer une nouvelle.

2. `Optional<Card> findByCardNumber(String cardNumber);`

Cette méthode permet de récupérer une carte complète basée sur son numéro. Elle est utile pour :

- **Récupération de données** : Quand vous avez besoin de l'entité `Card` complète pour effectuer des opérations ou afficher des détails.
- **Flexibilité** : Le retour `Optional` permet de gérer élégamment le cas où la carte n'existe pas.
- **Opérations spécifiques** : Utile pour des opérations comme la mise à jour ou la suppression d'une carte basée sur son numéro.

L'utilisation de ces deux méthodes ensemble offre une flexibilité dans votre code :

- Utilisez `existsByCardNumber` quand vous avez juste besoin de vérifier l'existence.
- Utilisez `findByCardNumber` quand vous avez besoin de l'entité complète.

Cette approche suit les bonnes pratiques de Spring Data JPA en utilisant des méthodes dérivées qui sont automatiquement implémentées par le framework, offrant une syntaxe claire et une exécution optimisée.

----------------------------
# Annexe 2
----------------------------

- Pourquoi n'est-il pas nécessaire de déclarer explicitement des méthodes comme findAll() ou findById() dans l'interface CardRepository, et comment ces méthodes sont-elles disponibles pour utilisation dans le code ?

### Réponse : 
- En effet, les méthodes comme `findAll()`, `findById()`, et d'autres méthodes de requête standard sont généralement fournies automatiquement par Spring Data JPA lorsque vous étendez l'interface `JpaRepository`. Voici pourquoi ces méthodes n'ont pas été explicitement mentionnées dans le code précédent :

1. **Méthodes héritées automatiquement** : 
   Lorsque votre interface de repository étend `JpaRepository`, elle hérite automatiquement de nombreuses méthodes utiles, dont `findAll()`, `findById()`, `save()`, `deleteById()`, etc. Vous n'avez pas besoin de les déclarer explicitement dans votre interface.

2. **Méthodes personnalisées** :
   Les méthodes `existsByCardNumber()` et `findByCardNumber()` ont été ajoutées explicitement car ce sont des méthodes personnalisées qui ne font pas partie des méthodes standard de `JpaRepository`. Spring Data JPA les implémente automatiquement en se basant sur leur nom.

3. **Utilisation implicite** :
   Dans le code du contrôleur, vous utilisez probablement déjà des méthodes comme `findAll()` ou `findById()` sans les avoir déclarées explicitement dans l'interface du repository. C'est parce qu'elles sont héritées de `JpaRepository`.

4. **Flexibilité et extensibilité** :
   En étendant `JpaRepository`, vous obtenez un ensemble complet de méthodes CRUD et de pagination, tout en gardant la possibilité d'ajouter des méthodes personnalisées selon vos besoins spécifiques.

Si vous voulez rendre ces méthodes plus explicites dans votre code, vous pouvez les déclarer dans votre interface `CardRepository`, mais ce n'est pas strictement nécessaire. Par exemple :

```java
public interface CardRepository extends JpaRepository<Card, Integer> {
    // Ces méthodes sont déjà héritées de JpaRepository, 
    // mais vous pouvez les déclarer explicitement si vous le souhaitez
    List<Card> findAll();
    Optional<Card> findById(Integer id);
    Card save(Card card);
    void deleteById(Integer id);

    // Méthodes personnalisées
    boolean existsByCardNumber(String cardNumber);
    Optional<Card> findByCardNumber(String cardNumber);
}
```

- En résumé, ces méthodes standard sont disponibles et utilisables sans déclaration explicite grâce à l'héritage de `JpaRepository`. 
- Les méthodes personnalisées, en revanche, doivent être déclarées explicitement pour que Spring Data JPA puisse les implémenter automatiquement.

----------------------

# Annexe 3

- Pouvez-vous expliquer la différence entre les méthodes héritées de JpaRepository et les méthodes personnalisées dans une interface de repository, en donnant des exemples de chaque type et en expliquant leurs utilisations spécifiques ?
### Réponse : 

- Je vous présente un tableau comparatif des méthodes héritées de JpaRepository et des méthodes personnalisées que vous pourriez ajouter à votre interface CardRepository :

| Méthodes héritées de JpaRepository | Méthodes personnalisées (exemples) |
|-----------------------------------|-------------------------------------|
| findAll()                         | existsByCardNumber(String cardNumber) |
| findById(Integer id)              | findByCardNumber(String cardNumber) |
| save(Card card)                   | findByCustomerId(Integer customerId) |
| deleteById(Integer id)            | findByCardType(String cardType) |
| count()                           | findByTotalLimitGreaterThan(int limit) |
| existsById(Integer id)            | findByCreateDtBetween(LocalDate start, LocalDate end) |
| flush()                           | deleteByCardNumber(String cardNumber) |
| saveAll(Iterable<Card> cards)     | countByCustomerId(Integer customerId) |
| deleteAll()                       | findTop5ByOrderByTotalLimitDesc() |
| deleteAllById(Iterable<Integer> ids) | updateAvailableAmount(Integer cardId, int newAmount) |

### Explications :

1. Les méthodes héritées sont fournies automatiquement par Spring Data JPA lorsque vous étendez JpaRepository. Elles offrent des opérations CRUD de base et quelques fonctionnalités supplémentaires.

2. Les méthodes personnalisées sont celles que vous définissez vous-même dans l'interface du repository. Spring Data JPA les implémente automatiquement en se basant sur leur nom ou sur des annotations personnalisées.

3. Les méthodes personnalisées permettent de créer des requêtes spécifiques à votre domaine, comme rechercher par numéro de carte ou trouver toutes les cartes d'un client spécifique.

4. Vous n'avez pas besoin de déclarer explicitement les méthodes héritées dans votre interface CardRepository, car elles sont automatiquement disponibles. En revanche, vous devez déclarer les méthodes personnalisées pour qu'elles soient reconnues et implémentées par Spring Data JPA.

5. Les méthodes personnalisées offrent une grande flexibilité pour répondre aux besoins spécifiques de votre application, tandis que les méthodes héritées couvrent les opérations de base communes à la plupart des entités.



