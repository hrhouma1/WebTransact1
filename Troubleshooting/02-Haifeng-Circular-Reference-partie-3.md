# Mise en situation

Haifeng, un développeur travaillant sur un projet de gestion de cartes et de clients, a récemment rencontré un problème lors de la sérialisation d'objets Java en JSON avec Spring Boot et Jackson. Son objectif était de récupérer et de sérialiser les informations des **cartes** et des **clients** dans une API REST, mais il s'est retrouvé face à une erreur difficile à diagnostiquer : une **référence circulaire** entre les objets `Customer` et `Cards`.

# L'ancienne classe

Voici comment Haifeng avait initialement structuré ses classes `Customer` et `Cards` :

```java
@Entity
@Table(name = "customer")
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "customer_id")
    private int customerId;

    @Column(name = "name")
    private String name;

    @OneToMany(mappedBy = "customer", cascade = CascadeType.ALL)
    private List<Cards> cards;
}

@Entity
@Table(name = "cards")
public class Cards {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "card_id")
    private int cardId;

    @Column(name = "card_number")
    private String cardNumber;

    @ManyToOne
    @JoinColumn(name = "customer_id", nullable = false)
    private Customer customer;
}
```

# Problème rencontré

Lorsque Haifeng tentait d'appeler l'API pour récupérer des données concernant un client et ses cartes, Jackson, la bibliothèque responsable de la sérialisation en JSON, essayait de **sérialiser récursivement** les objets liés. Voici ce qui se passait :
1. **Customer** a une liste de **Cards**.
2. Chaque **Card** fait référence à un **Customer**.
3. Cette relation circulaire a créé une boucle infinie lors de la sérialisation.

Cela a conduit à l'**erreur suivante** :
```
org.hibernate.PropertyValueException: not-null property references a null or transient value
```

Haifeng a également constaté que son service REST renvoyait des réponses vides ou échouait, car Jackson n'arrivait pas à gérer ces boucles.

# Solution proposée

Pour résoudre ce problème, Haifeng a utilisé les annotations `@JsonManagedReference` et `@JsonBackReference` fournies par Jackson. Ces annotations permettent à Jackson de comprendre quelles relations sérialiser et lesquelles ignorer, afin d'éviter les boucles infinies.

# Classe modifiée avec la solution

Voici comment Haifeng a modifié ses classes :

```java
@Entity
@Table(name = "customer")
public class Customer {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "customer_id")
    private int customerId;

    @Column(name = "name")
    private String name;

    @JsonManagedReference
    @OneToMany(mappedBy = "customer", cascade = CascadeType.ALL)
    private List<Cards> cards;
}

@Entity
@Table(name = "cards")
public class Cards {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "card_id")
    private int cardId;

    @Column(name = "card_number")
    private String cardNumber;

    @JsonBackReference
    @ManyToOne
    @JoinColumn(name = "customer_id", nullable = false)
    private Customer customer;
}
```

# Explication des annotations

- **@JsonManagedReference** : Placée sur la relation `OneToMany` dans la classe `Customer`, cette annotation indique à Jackson de **sérialiser la référence vers les cartes**. Cela signifie que lorsque Jackson rencontre un objet `Customer`, il inclura les cartes associées dans le JSON.
  
- **@JsonBackReference** : Placée sur la relation `ManyToOne` dans la classe `Cards`, cette annotation dit à Jackson de **ne pas sérialiser la référence inverse** (vers le `Customer`) lorsqu'il sérialise un objet `Cards`. Cela coupe la boucle circulaire.

# Résultat

Après avoir appliqué cette solution, Haifeng a pu sérialiser ses entités sans rencontrer de boucles infinies ni d'erreurs liées aux références circulaires. L'API REST renvoyait désormais correctement les informations sur un client et ses cartes, sans tenter de sérialiser en boucle l'intégralité des objets liés.

# Conclusion

Le problème rencontré par Haifeng est assez courant lorsqu'on travaille avec des relations bidirectionnelles dans les entités JPA/Hibernate et la sérialisation JSON. En utilisant les annotations Jackson adaptées (`@JsonManagedReference` et `@JsonBackReference`), il a pu résoudre ce problème et garantir une sérialisation correcte de ses objets tout en évitant les erreurs et les boucles infinies. Cette approche est particulièrement utile dans des applications où les relations entre les entités sont complexes et multiples.
