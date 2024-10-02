----------------------------
# Exemple 2
---------------------------



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


L'utilisation de l'annotation `@ManyToOne` et `@OneToMany` dans une relation bidirectionnelle entre entités en JPA peut aussi entraîner des dépendances cycliques si elle n'est pas bien gérée. Voici un exemple typique de relation bidirectionnelle entre deux entités, `Customer` et `Card`, avec les annotations JPA, et une explication sur la manière d'éviter une dépendance cyclique dans ce cas.

### 1. **Exemple avec `@OneToMany` et `@ManyToOne` :**

```java
// Entité Customer
@Entity
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long customerId;

    private String customerFirstName;
    private String customerLastName;

    // Relation bidirectionnelle avec Card
    @OneToMany(mappedBy = "customer", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Card> cards;

    // Constructeurs, getters et setters
    public Customer() {}

    public List<Card> getCards() {
        return cards;
    }

    public void setCards(List<Card> cards) {
        this.cards = cards;
    }

    // Autres getters et setters
}
```

```java
// Entité Card
@Entity
public class Card {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long cardId;

    private String cardNumber;
    private String cardType;
    private Double totalLimit;
    private Double amountUsed;
    private Double availableAmount;
    private LocalDateTime createDt;

    // Relation bidirectionnelle avec Customer
    @ManyToOne
    @JoinColumn(name = "customer_id", nullable = false)
    private Customer customer;

    // Constructeurs, getters et setters
    public Card() {}

    public Customer getCustomer() {
        return customer;
    }

    public void setCustomer(Customer customer) {
        this.customer = customer;
    }

    // Autres getters et setters
}
```

### 2. **Explication de la relation bidirectionnelle :**

- **Customer** : Un client (`Customer`) peut avoir plusieurs cartes de crédit, donc la relation est de **"One-to-Many"** avec les cartes.
- **Card** : Une carte de crédit (`Card`) appartient à un seul client, donc la relation est de **"Many-to-One"** avec le client.

Dans cette configuration, chaque entité est liée à l'autre à travers une relation bidirectionnelle. Cela signifie que `Customer` connaît ses `Card` et chaque `Card` connaît son `Customer`. Cela peut provoquer un cycle si vous ne gérez pas correctement la manière dont les entités sont chargées ou mises à jour.

### 3. **Problème potentiel de dépendance cyclique :**
Dans les relations bidirectionnelles, si vous essayez de charger l'une des entités, comme `Customer`, et que JPA essaie de charger toutes les `Card` associées, puis que chaque `Card` essaie de charger son `Customer`, cela pourrait conduire à une situation où le système entre dans une boucle infinie de chargement. En particulier, si la sérialisation des objets est impliquée (par exemple, lors de la conversion des objets en JSON), vous pouvez obtenir une exception comme :

```
com.fasterxml.jackson.databind.JsonMappingException: Infinite recursion (StackOverflowError)
```

### 4. **Comment éviter les problèmes :**

#### A. **Utilisation de `@JsonIgnore` ou `@JsonManagedReference` et `@JsonBackReference` avec Jackson :**

Pour éviter des problèmes de sérialisation (comme une récursion infinie lors de la conversion en JSON), vous pouvez utiliser les annotations Jackson `@JsonIgnore`, `@JsonManagedReference`, et `@JsonBackReference`.

**Solution 1 : Utilisation de `@JsonIgnore`**

```java
// Entité Customer
@Entity
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long customerId;

    private String customerFirstName;
    private String customerLastName;

    @OneToMany(mappedBy = "customer", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    @JsonIgnore // Ignore les cartes lors de la sérialisation de Customer
    private List<Card> cards;

    // Constructeurs, getters et setters
}
```

Ici, l'annotation `@JsonIgnore` permet d'éviter que les cartes soient sérialisées lors de la conversion d'un objet `Customer` en JSON.

**Solution 2 : Utilisation de `@JsonManagedReference` et `@JsonBackReference`**

```java
// Entité Customer
@Entity
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long customerId;

    private String customerFirstName;
    private String customerLastName;

    @OneToMany(mappedBy = "customer", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    @JsonManagedReference // Gère la sérialisation de la relation
    private List<Card> cards;

    // Constructeurs, getters et setters
}
```

```java
// Entité Card
@Entity
public class Card {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long cardId;

    private String cardNumber;
    private String cardType;

    @ManyToOne
    @JoinColumn(name = "customer_id", nullable = false)
    @JsonBackReference // Empêche la récursion infinie en ne sérialisant pas le customer
    private Customer customer;

    // Constructeurs, getters et setters
}
```

- **`@JsonManagedReference`** est utilisée du côté parent de la relation (dans `Customer`) pour gérer la sérialisation.
- **`@JsonBackReference`** est utilisée du côté enfant (dans `Card`) pour ignorer la sérialisation de la propriété `customer`, empêchant ainsi la récursion infinie.

#### B. **Utilisation de `fetch = FetchType.LAZY` :**
Un autre moyen d'éviter les problèmes de dépendance cyclique est d'utiliser la **récupération paresseuse** (`FetchType.LAZY`). Cela signifie que les collections associées (comme les `Card` d'un `Customer`) ne seront chargées qu'à la demande et non immédiatement lorsque l'objet `Customer` est récupéré.

```java
@OneToMany(mappedBy = "customer", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
private List<Card> cards;
```

Cela peut réduire la charge initiale et éviter que toutes les dépendances ne soient résolues immédiatement, ce qui peut causer des problèmes dans des situations de dépendance complexe.

### 5. **Conclusion :**
Les relations bidirectionnelles avec `@OneToMany` et `@ManyToOne` peuvent introduire des problèmes de dépendance cyclique, notamment lors de la sérialisation des objets ou des chargements en cascade. Utiliser des annotations comme `@JsonIgnore`, `@JsonManagedReference` et `@JsonBackReference`, ainsi que configurer le chargement paresseux avec `fetch = FetchType.LAZY`, sont des techniques essentielles pour éviter ces cycles et garantir le bon fonctionnement de votre application Spring Boot avec JPA.
