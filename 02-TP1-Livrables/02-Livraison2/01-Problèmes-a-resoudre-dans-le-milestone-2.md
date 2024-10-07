------------------------------------------
# Problèmes milestone 1 - Gestion des Cartes Bancaires
------------------------------------------



## Problèmes Actuels

### Problème 0 : Dépendance à l'existence d'un client
- **Description** : L'ajout d'une carte nécessite l'existence préalable d'un client dans la base de données.
- **Conséquence** : Une erreur 500 est générée si aucun client n'existe lors de l'ajout d'une carte.
- **Solution proposée** : Implémenter une vérification de l'existence du client avant l'ajout de la carte et retourner une réponse appropriée.

### Problème 1 : Duplication des numéros de carte
- **Description** : Le système permet actuellement l'insertion de plusieurs cartes avec le même numéro.
- **Conséquence** : Risque d'incohérence des données et de problèmes de gestion des cartes.
- **Solution proposée** : Ajouter une contrainte d'unicité sur le numéro de carte dans le modèle et vérifier l'existence du numéro avant l'insertion.

## Avertissements pour le Développement Futur

### Dépendances Cycliques Potentielles
- **Situation actuelle** : Le projet ne comporte qu'un seul modèle, ce qui simplifie la gestion des dépendances et la sérialisation JSON.
- **Avertissement** : L'ajout de nouveaux modèles pourrait introduire des dépendances cycliques.
- **Risques** :
  1. Erreurs de sérialisation JSON.
  2. Problèmes d'architecture dans l'application.
- **Recommandations** :
  1. Être vigilant lors de l'ajout de nouveaux modèles et de leurs interactions.
  2. Utiliser des techniques comme @JsonManagedReference et @JsonBackReference pour gérer les relations bidirectionnelles.
  3. Considérer l'utilisation de DTO (Data Transfer Objects) pour séparer la logique de présentation de la logique métier.

## Bonnes Pratiques
- Toujours vérifier les dépendances entre les modèles lors de l'ajout de nouvelles entités.
- Utiliser des annotations JPA appropriées pour gérer les relations entre entités.
- Implémenter des validations côté serveur pour assurer l'intégrité des données.
- Gérer correctement les exceptions et retourner des codes HTTP appropriés.

## Prochaines Étapes
1. Résoudre le problème de dépendance au client existant.
2. Implémenter la contrainte d'unicité sur les numéros de carte.
3. Préparer l'architecture pour l'ajout futur de nouveaux modèles en anticipant les problèmes potentiels de dépendances cycliques.


## ==> Pour résoudre les problèmes du milestone 1, voici une proposition de code amélioré pour votre CardController et le modèle Card, ainsi qu'un fichier HTTP mis à jour :



------------------------------------

# CardController

```java
package com.exemple.demo.controller;

import com.exemple.demo.model.Card;
import com.exemple.demo.repository.CardRepository;
import com.exemple.demo.repository.CustomerRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/cards")
public class CardController {

    @Autowired
    private CardRepository cardRepository;

    @Autowired
    private CustomerRepository customerRepository;

    @GetMapping("/all")
    public ResponseEntity<List<Card>> getAllCards() {
        return ResponseEntity.ok(cardRepository.findAll());
    }

    @PostMapping("/add")
    public ResponseEntity<?> addCard(@RequestBody Card card) {
        if (cardRepository.existsByCardNumber(card.getCardNumber())) {
            return ResponseEntity
                .status(HttpStatus.CONFLICT)
                .body("Une carte avec ce numéro existe déjà");
        }

        if (!customerRepository.existsById(card.getCustomer().getCustomerId())) {
            return ResponseEntity
                .status(HttpStatus.NOT_FOUND)
                .body("Le client spécifié n'existe pas");
        }

        Card savedCard = cardRepository.save(card);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedCard);
    }
}
```

# Card (modèle)

```java
package com.exemple.demo.model;

import java.time.LocalDate;
import jakarta.persistence.*;
import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@Entity
@Table(name = "card")
public class Card {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "card_id")
    private int cardId;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "customer_id", nullable = false)
    @JsonIgnoreProperties({"hibernateLazyInitializer", "handler"})
    private Customer customer;

    @Column(name = "card_number", unique = true)
    private String cardNumber;

    // ... autres champs ...

    // Constructeurs, getters et setters
}
```

# CardRepository

```java
package com.exemple.demo.repository;

import com.exemple.demo.model.Card;
import org.springframework.data.jpa.repository.JpaRepository;

public interface CardRepository extends JpaRepository<Card, Integer> {
    boolean existsByCardNumber(String cardNumber);
}
```

# CustomerRepository

```java
package com.exemple.demo.repository;

import com.exemple.demo.model.Customer;
import org.springframework.data.jpa.repository.JpaRepository;

public interface CustomerRepository extends JpaRepository<Customer, Integer> {
}
```

# Fichier HTTP

```http
### Scénario 1: Gestion des cartes (Card)

### 1.1 Read (All): Afficher toutes les cartes
GET http://localhost:8080/cards/all

### 1.2 Create: Ajouter une carte
POST http://localhost:8080/cards/add
Content-Type: application/json

{
  "customer": {
    "customerId": 1
  },
  "cardNumber": "1234-5678-9012-3456",
  "cardType": "Visa",
  "totalLimit": 5000,
  "amountUsed": 1000,
  "availableAmount": 4000,
  "createDt": "2024-09-30"
}

### 1.3 Create: Tenter d'ajouter une carte avec un numéro existant (devrait échouer)
POST http://localhost:8080/cards/add
Content-Type: application/json

{
  "customer": {
    "customerId": 1
  },
  "cardNumber": "1234-5678-9012-3456",
  "cardType": "MasterCard",
  "totalLimit": 10000,
  "amountUsed": 2000,
  "availableAmount": 8000,
  "createDt": "2024-09-30"
}

### 1.4 Read (All): Vérifier l'ajout des cartes
GET http://localhost:8080/cards/all
```

Ces modifications résolvent les problèmes mentionnés :

1. La contrainte d'unicité sur le numéro de carte est ajoutée dans le modèle Card et vérifiée dans le contrôleur.
2. La vérification de l'existence du client est effectuée avant l'ajout d'une carte.
3. L'annotation @JsonIgnoreProperties est utilisée pour éviter les problèmes potentiels de sérialisation avec les relations lazy.
4. Le contrôleur renvoie maintenant des ResponseEntity avec des codes HTTP appropriés.

Ces changements améliorent la robustesse de votre application et préviennent les problèmes potentiels liés aux dépendances cycliques et à la duplication des numéros de carte.
