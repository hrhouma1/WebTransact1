# Exemple 1

Dans le cadre de Spring Boot, une erreur de **dépendance cyclique** survient lorsque deux ou plusieurs classes ou beans sont interdépendants dans un cycle infini, empêchant le bon fonctionnement de l'injection de dépendances. Cela se produit généralement lorsque les classes dépendent les unes des autres pour leur création, ce qui crée une boucle.

### Exemple de dépendance cyclique dans le projet avec **Customer** et **Card** :

Imaginons que vous avez les classes `Customer` et `Card` et que chaque classe dépend de l'autre d'une manière cyclique. Par exemple :

```java
// Classe Customer
@Component
public class Customer {

    private Long customerId;
    private String customerFirstName;
    private String customerLastName;
    
    @Autowired
    private CardService cardService; // Dépend de CardService

    // Getters, setters, constructeurs...
}
```

```java
// Classe Card
@Component
public class Card {

    private Long cardId;
    private Long customerId;
    private String cardNumber;
    private String cardType;
    
    @Autowired
    private CustomerService customerService; // Dépend de CustomerService

    // Getters, setters, constructeurs...
}
```

Dans cet exemple, la classe `Customer` dépend de `CardService` (ou d'une autre logique liée aux cartes), tandis que la classe `Card` dépend de `CustomerService`. Cela crée une boucle de dépendance entre `Customer` et `Card`, ce qui provoque une erreur de dépendance cyclique.

### Problème avec cette structure :
Lorsque Spring essaie de résoudre les dépendances en injectant les beans (`@Autowired`), il ne peut pas créer l'instance de `Customer` sans d'abord créer `CardService`, et vice versa. Cela empêche la création des beans nécessaires, et vous verrez probablement une erreur comme celle-ci :

```
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'customerService': Unsatisfied dependency expressed through field 'cardService'; nested exception is org.springframework.beans.factory.BeanCurrentlyInCreationException: Error creating bean with name 'cardService': Requested bean is currently in creation: Is there an unresolvable circular reference?
```

### Comment éviter une dépendance cyclique :

#### 1. **Injection par constructeur ou setter** :
   Vous pouvez casser la boucle en injectant les dépendances d'une manière différente. Par exemple, au lieu d'injecter directement avec `@Autowired` dans les champs, utilisez l'injection par constructeur, ou envisagez d'injecter une seule dépendance à travers un setter.

```java
// Classe Customer avec injection par setter
@Component
public class Customer {

    private CardService cardService;

    @Autowired
    public void setCardService(CardService cardService) {
        this.cardService = cardService;
    }
}
```

#### 2. **Utilisation d'une couche de service pour les dépendances logiques** :
   Vous pouvez introduire une couche de service qui découple la logique métier et limite les dépendances directes entre entités.

```java
// CustomerService
@Service
public class CustomerService {

    @Autowired
    private CardService cardService; // Logique des cartes gérée ici

    public Customer getCustomerById(Long customerId) {
        // Logique métier, récupération des cartes si nécessaire
        return customerRepository.findById(customerId).get();
    }
}

// CardService
@Service
public class CardService {

    public List<Card> getCardsForCustomer(Long customerId) {
        // Logique pour récupérer les cartes liées au client
        return cardRepository.findByCustomerId(customerId);
    }
}
```

#### 3. **Découplage avec des événements ou des messages** :
   Si les classes doivent être fortement couplées, envisagez d'utiliser un mécanisme d'événements ou des messages pour qu'elles communiquent sans dépendre directement l'une de l'autre.

```java
@Service
public class CustomerService {

    @Autowired
    private ApplicationEventPublisher publisher;

    public void createCustomer(Customer customer) {
        // Logique de création du client
        publisher.publishEvent(new CustomerCreatedEvent(customer));
    }
}

@Service
public class CardService implements ApplicationListener<CustomerCreatedEvent> {

    @Override
    public void onApplicationEvent(CustomerCreatedEvent event) {
        // Logique pour créer une carte quand un client est créé
        createCardForCustomer(event.getCustomer());
    }
}
```

### Conclusion :
Dans votre projet **Customer** et **Card**, si vous injectez des services qui dépendent l'un de l'autre directement, cela pourrait causer une dépendance cyclique. Pour résoudre cela, vous pouvez :
1. Utiliser des injections par constructeur ou setter.
2. Introduire une couche de service pour éviter le couplage direct entre les entités.
3. Utiliser des mécanismes comme des événements pour éviter les dépendances directes.

Cela vous permettra d'organiser votre code sans qu'il ne tombe dans un cycle de dépendance problématique.
