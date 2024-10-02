# Application JPA 

- Cet exercice fournit une progression naturelle de méthodes simples (`findBy`) vers des requêtes plus complexes nécessitant une réflexion plus approfondie et l'utilisation de jointures et d'agrégations.
- Je vous propose une série de 30 requêtes SQL, classées de la plus simple à la plus complexe, basées sur le diagramme de votre base de données `Customer` et `Card`.
- Votre mission est de compléter les noms des méthodes JPA dans la colonne dédiée :



| **Niveau**   | **Requête SQL**                                                                                                                                  | **Indice**                                                          | **Nom de la méthode JPA**              |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|-----------------------------------------|
| **Facile**   | `SELECT * FROM Customer;`                                                                                                                        | Récupérer tous les clients.                                           |                                         |
| **Facile**   | `SELECT * FROM Card;`                                                                                                                            | Récupérer toutes les cartes.                                          |                                         |
| **Facile**   | `SELECT * FROM Customer WHERE customerId = 1;`                                                                                                   | Récupérer un client par son ID.                                       |                                         |
| **Facile**   | `SELECT * FROM Card WHERE cardId = 2;`                                                                                                           | Récupérer une carte par son ID.                                       |                                         |
| **Facile**   | `SELECT * FROM Customer WHERE customerFirstName = 'John';`                                                                                       | Rechercher un client par son prénom.                                  |                                         |
| **Facile**   | `SELECT * FROM Customer WHERE customerLastName = 'Doe';`                                                                                         | Rechercher un client par son nom de famille.                          |                                         |
| **Facile**   | `SELECT * FROM Card WHERE customerId = 1;`                                                                                                       | Rechercher toutes les cartes d'un client spécifique par son `customerId`. |                                         |
| **Facile**   | `SELECT * FROM Card WHERE cardNumber = '1234567890';`                                                                                            | Rechercher une carte par son numéro.                                  |                                         |
| **Facile**   | `SELECT * FROM Card WHERE cardType = 'Gold';`                                                                                                    | Rechercher toutes les cartes de type "Gold".                          |                                         |
| **Facile**   | `SELECT * FROM Customer WHERE customerFirstName = 'John' AND customerLastName = 'Doe';`                                                          | Rechercher un client par son prénom et son nom de famille.            |                                         |
| **Facile**   | `SELECT * FROM Customer WHERE customerFirstName LIKE 'J%';`                                                                                      | Rechercher les clients dont le prénom commence par 'J'.               |                                         |
| **Facile**   | `SELECT * FROM Card WHERE totalLimit = 10000;`                                                                                                   | Rechercher les cartes avec une limite totale de 10000.                |                                         |
| **Moyen**    | `SELECT * FROM Card WHERE availableAmount > 500;`                                                                                                | Rechercher les cartes avec un montant disponible supérieur à 500.     |                                         |
| **Moyen**    | `SELECT * FROM Card WHERE amountUsed < 2000;`                                                                                                    | Rechercher les cartes avec un montant utilisé inférieur à 2000.       |                                         |
| **Moyen**    | `SELECT * FROM Card WHERE createDt > '2024-01-01';`                                                                                              | Rechercher les cartes créées après une date spécifique.               |                                         |
| **Moyen**    | `SELECT * FROM Customer WHERE customerLastName = 'Smith' OR customerLastName = 'Doe';`                                                           | Rechercher les clients avec les noms de famille 'Smith' ou 'Doe'.     |                                         |
| **Moyen**    | `SELECT * FROM Card WHERE totalLimit > 3000 AND totalLimit < 7000;`                                                                              | Rechercher les cartes avec une limite totale comprise entre 3000 et 7000. |                                         |
| **Moyen**    | `SELECT * FROM Card WHERE cardType = 'Gold' AND totalLimit > 5000;`                                                                              | Rechercher les cartes de type "Gold" avec une limite totale supérieure à 5000. |                                         |
| **Moyen**    | `SELECT * FROM Customer WHERE customerFirstName IN ('John', 'Jane', 'Jim');`                                                                     | Rechercher les clients avec une liste de prénoms spécifiques.         |                                         |
| **Moyen**    | `SELECT COUNT(*) FROM Card WHERE cardType = 'Platinum';`                                                                                         | Compter le nombre de cartes de type "Platinum".                      |                                         |
| **Moyen**    | `SELECT AVG(totalLimit) FROM Card WHERE cardType = 'Gold';`                                                                                      | Calculer la limite totale moyenne des cartes de type "Gold".          |                                         |
| **Difficile**| `SELECT * FROM Customer c JOIN Card ca ON c.customerId = ca.customerId WHERE ca.cardType = 'Gold';`                                               | Rechercher les clients ayant une carte de type "Gold".                |                                         |
| **Difficile**| `SELECT * FROM Customer WHERE EXISTS (SELECT 1 FROM Card WHERE customerId = Customer.customerId AND cardType = 'Platinum');`                     | Rechercher les clients ayant au moins une carte de type "Platinum".   |                                         |
| **Difficile**| `SELECT * FROM Card WHERE amountUsed > 1000 AND availableAmount < 500;`                                                                          | Rechercher les cartes avec un montant utilisé supérieur à 1000 et un montant disponible inférieur à 500. |                                         |
| **Difficile**| `SELECT c.* FROM Customer c LEFT JOIN Card ca ON c.customerId = ca.customerId WHERE ca.cardId IS NULL;`                                           | Rechercher les clients n'ayant aucune carte associée.                 |                                         |
| **Difficile**| `SELECT * FROM Card WHERE createDt BETWEEN '2024-01-01' AND '2024-12-31';`                                                                       | Rechercher les cartes créées dans l'année 2024.                       |                                         |
| **Difficile**| `SELECT * FROM Card WHERE availableAmount > 1000 ORDER BY availableAmount DESC;`                                                                 | Rechercher les cartes avec un montant disponible supérieur à 1000, triées par montant disponible décroissant. |                                         |
| **Difficile**| `SELECT * FROM Customer c JOIN Card ca ON c.customerId = ca.customerId WHERE ca.cardType = 'Gold' AND ca.totalLimit > 5000 ORDER BY c.customerLastName;` | Rechercher les clients ayant une carte "Gold" avec une limite totale supérieure à 5000, triés par nom de famille. |                                         |
| **Difficile**| `SELECT SUM(availableAmount) FROM Card WHERE customerId = 1;`                                                                                    | Calculer le montant disponible total des cartes d'un client spécifique. |                                         |
| **Difficile**| `SELECT * FROM Card WHERE totalLimit > 5000 AND amountUsed BETWEEN 1000 AND 2000;`                                                               | Rechercher les cartes avec une limite totale supérieure à 5000 et un montant utilisé compris entre 1000 et 2000. |                                         |

### Instructions pour les étudiants :

- Complétez la colonne **Nom de la méthode JPA** en dérivant les méthodes à partir des requêtes SQL.
- Respectez la syntaxe des méthodes JPA avec des préfixes comme `findBy`, `countBy`, `sumBy`, et utilisez les bons opérateurs (`And`, `Or`, `GreaterThan`, etc.).







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


----------------------




| **Niveau**   | **Requête SQL**                                                                                                                                  | **Indice**                                                          | **Nom de la méthode JPA**              |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|-----------------------------------------|
| **Facile**   | `SELECT * FROM Customer;`                                                                                                                        | Récupérer tous les clients.                                           | `findAll()`                             |
| **Facile**   | `SELECT * FROM Card;`                                                                                                                            | Récupérer toutes les cartes.                                          | `findAll()`                             |
| **Facile**   | `SELECT * FROM Customer WHERE customerId = 1;`                                                                                                   | Récupérer un client par son ID.                                       | `findByCustomerId(Long customerId)`     |
| **Facile**   | `SELECT * FROM Card WHERE cardId = 2;`                                                                                                           | Récupérer une carte par son ID.                                       | `findByCardId(Long cardId)`             |
| **Facile**   | `SELECT * FROM Customer WHERE customerFirstName = 'John';`                                                                                       | Rechercher un client par son prénom.                                  | `findByCustomerFirstName(String firstName)` |
| **Facile**   | `SELECT * FROM Customer WHERE customerLastName = 'Doe';`                                                                                         | Rechercher un client par son nom de famille.                          | `findByCustomerLastName(String lastName)` |
| **Facile**   | `SELECT * FROM Card WHERE customerId = 1;`                                                                                                       | Rechercher toutes les cartes d'un client spécifique par son `customerId`. | `findByCustomerCustomerId(Long customerId)` |
| **Facile**   | `SELECT * FROM Card WHERE cardNumber = '1234567890';`                                                                                            | Rechercher une carte par son numéro.                                  | `findByCardNumber(String cardNumber)`   |
| **Facile**   | `SELECT * FROM Card WHERE cardType = 'Gold';`                                                                                                    | Rechercher toutes les cartes de type "Gold".                          | `findByCardType(String cardType)`       |
| **Facile**   | `SELECT * FROM Customer WHERE customerFirstName = 'John' AND customerLastName = 'Doe';`                                                          | Rechercher un client par son prénom et son nom de famille.            | `findByCustomerFirstNameAndCustomerLastName(String firstName, String lastName)` |
| **Facile**   | `SELECT * FROM Customer WHERE customerFirstName LIKE 'J%';`                                                                                      | Rechercher les clients dont le prénom commence par 'J'.               | `findByCustomerFirstNameStartingWith(String prefix)` |
| **Facile**   | `SELECT * FROM Card WHERE totalLimit = 10000;`                                                                                                   | Rechercher les cartes avec une limite totale de 10000.                | `findByTotalLimit(Double totalLimit)`   |
| **Moyen**    | `SELECT * FROM Card WHERE availableAmount > 500;`                                                                                                | Rechercher les cartes avec un montant disponible supérieur à 500.     | `findByAvailableAmountGreaterThan(Double amount)` |
| **Moyen**    | `SELECT * FROM Card WHERE amountUsed < 2000;`                                                                                                    | Rechercher les cartes avec un montant utilisé inférieur à 2000.       | `findByAmountUsedLessThan(Double amount)` |
| **Moyen**    | `SELECT * FROM Card WHERE createDt > '2024-01-01';`                                                                                              | Rechercher les cartes créées après une date spécifique.               | `findByCreateDtAfter(LocalDate date)`   |
| **Moyen**    | `SELECT * FROM Customer WHERE customerLastName = 'Smith' OR customerLastName = 'Doe';`                                                           | Rechercher les clients avec les noms de famille 'Smith' ou 'Doe'.     | `findByCustomerLastNameIn(List<String> lastNames)` |
| **Moyen**    | `SELECT * FROM Card WHERE totalLimit > 3000 AND totalLimit < 7000;`                                                                              | Rechercher les cartes avec une limite totale comprise entre 3000 et 7000. | `findByTotalLimitBetween(Double min, Double max)` |
| **Moyen**    | `SELECT * FROM Card WHERE cardType = 'Gold' AND totalLimit > 5000;`                                                                              | Rechercher les cartes de type "Gold" avec une limite totale supérieure à 5000. | `findByCardTypeAndTotalLimitGreaterThan(String cardType, Double totalLimit)` |
| **Moyen**    | `SELECT * FROM Customer WHERE customerFirstName IN ('John', 'Jane', 'Jim');`                                                                     | Rechercher les clients avec une liste de prénoms spécifiques.         | `findByCustomerFirstNameIn(List<String> firstNames)` |
| **Moyen**    | `SELECT COUNT(*) FROM Card WHERE cardType = 'Platinum';`                                                                                         | Compter le nombre de cartes de type "Platinum".                      | `countByCardType(String cardType)`      |
| **Moyen**    | `SELECT AVG(totalLimit) FROM Card WHERE cardType = 'Gold';`                                                                                      | Calculer la limite totale moyenne des cartes de type "Gold".          | `findAvgTotalLimitByCardType(String cardType)` |
| **Difficile**| `SELECT * FROM Customer c JOIN Card ca ON c.customerId = ca.customerId WHERE ca.cardType = 'Gold';`                                               | Rechercher les clients ayant une carte de type "Gold".                | `findByCardsCardType(String cardType)`  |
| **Difficile**| `SELECT * FROM Customer WHERE EXISTS (SELECT 1 FROM Card WHERE customerId = Customer.customerId AND cardType = 'Platinum');`                     | Rechercher les clients ayant au moins une carte de type "Platinum".   | `findByCardsCardTypeExists(String cardType)` |
| **Difficile**| `SELECT * FROM Card WHERE amountUsed > 1000 AND availableAmount < 500;`                                                                          | Rechercher les cartes avec un montant utilisé supérieur à 1000 et un montant disponible inférieur à 500. | `findByAmountUsedGreaterThanAndAvailableAmountLessThan(Double used, Double available)` |
| **Difficile**| `SELECT c.* FROM Customer c LEFT JOIN Card ca ON c.customerId = ca.customerId WHERE ca.cardId IS NULL;`                                           | Rechercher les clients n'ayant aucune carte associée.                 | `findByCardsIsNull()`                   |
| **Difficile**| `SELECT * FROM Card WHERE createDt BETWEEN '2024-01-01' AND '2024-12-31';`                                                                       | Rechercher les cartes créées dans l'année 2024.                       | `findByCreateDtBetween(LocalDate start, LocalDate end)` |
| **Difficile**| `SELECT * FROM Card WHERE availableAmount > 1000 ORDER BY availableAmount DESC;`                                                                 | Rechercher les cartes avec un montant disponible supérieur à 1000, triées par montant disponible décroissant. | `findByAvailableAmountGreaterThanOrderByAvailableAmountDesc(Double amount)` |
| **Difficile**| `SELECT * FROM Customer c JOIN Card ca ON c.customerId = ca.customerId WHERE ca.cardType = 'Gold' AND ca.totalLimit > 5000 ORDER BY c.customerLastName;` | Rechercher les clients ayant une carte "Gold" avec une limite totale supérieure à 5000, triés par nom de famille. | `findByCardsCardTypeAndTotalLimitGreaterThanOrderByCustomerLastName(String cardType, Double limit)` |
| **Difficile**| `SELECT SUM(availableAmount) FROM Card WHERE customerId = 1;`                                                                                    | Calculer le montant disponible total des cartes d'un client spécifique. | `sumByCustomerId(Long customerId)`      |
| **Difficile**| `SELECT * FROM Card WHERE totalLimit > 5000 AND amountUsed BETWEEN 1000 AND 2000;`                                                               | Rechercher les cartes avec une limite totale supérieure à 5000 et un montant utilisé compris entre 1000 et 2000. | `findByTotalLimitGreaterThanAndAmountUsedBetween(Double limit, Double minAmount, Double maxAmount)` |

Ces méthodes JPA sont dérivées des requêtes SQL selon la convention de nommage de Spring Data JPA.
