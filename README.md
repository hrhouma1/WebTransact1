---------------------------------------------------------------------------------
# Livraison 1 : Étapes 1 à 4
---------------------------------------------------------------------------------

| Étape | Description |
|-------|-------------|
| **1. Créer un projet avec Spring Initializr** | Choisir la version de Java, le nom du projet et le type de packaging (Jar). |
| **2. Ajouter les dépendances** | Ajouter les dépendances Spring Web, Spring Data JPA et PostgreSQL dans le `pom.xml`. |
| **3. Créer les modèles (entités)** | Créer les classes de modèle avec `@Entity`, `@Id`, `@GeneratedValue`. Ajouter les getters, setters, et constructeurs. |
| **4. Implémenter les contrôleurs** | Utiliser des listes en mémoire pour implémenter des opérations CRUD dans les contrôleurs REST. |

---------------------------------------------------------------------------------
# Livraison 2 : Étapes 5 à 8
---------------------------------------------------------------------------------

| Étape | Description |
|-------|-------------|
| **5. Configurer PostgreSQL** | Configurer la base de données dans `application.properties` (URL, utilisateur, mot de passe). Créer les tables à partir des entités. |
| **6. Vérifier JPA** | Vérifier que `spring-boot-starter-data-jpa` est dans `pom.xml`. Assurer l’utilisation correcte des annotations JPA (`@Entity`, `@Table`, etc.). |
| **7. Implémenter la base de données** | Remplacer les listes en mémoire par une base de données réelle, en utilisant Spring Data JPA pour les opérations CRUD. |
| **8. Tester les fonctionnalités** | Utiliser Postman ou des tests unitaires pour vérifier les requêtes HTTP et les interactions avec la base de données PostgreSQL. |

---------------------------------------------------------------------------------
# Livraison 3 : Étapes 9 à 12
---------------------------------------------------------------------------------

| Étape | Description |
|-------|-------------|
| **9. Implémentation des méthodes simples (CRUD simples)** | Implémenter toutes les opérations CRUD (Create, Read, Update, Delete) en utilisant Spring Data JPA. |
| **10. Ajouter Swagger pour la documentation** | Intégrer Swagger dans le projet pour générer automatiquement la documentation des API. |
| **11. Implémenter des méthodes  personnalisées** | Ajouter des méthodes personnalisées dans les repositories JPA pour des requêtes spécifiques. |
| **12. Utiliser un exemple JPQL** | Utiliser JPQL (Java Persistence Query Language) pour écrire des requêtes plus complexes. |


Voici la suite avec **Livraison 4**, qui inclut les implémentations demandées dans le laboratoire et l'ajout des codes de retour appropriés :

---------------------------------------------------------------------------------
# Livraison 1 : Étapes 1 à 4
---------------------------------------------------------------------------------

| Étape | Description |
|-------|-------------|
| **1. Créer un projet avec Spring Initializr** | Choisir la version de Java, le nom du projet et le type de packaging (Jar). |
| **2. Ajouter les dépendances** | Ajouter les dépendances Spring Web, Spring Data JPA et PostgreSQL dans le `pom.xml`. |
| **3. Créer les modèles (entités)** | Créer les classes de modèle avec `@Entity`, `@Id`, `@GeneratedValue`. Ajouter les getters, setters, et constructeurs. |
| **4. Implémenter les opérations CRUD de base** | Implémenter les méthodes de base pour les opérations CRUD dans le contrôleur (GET, POST, PUT, DELETE). Utiliser des listes en mémoire pour simuler la base de données. |

---------------------------------------------------------------------------------
# Livraison 2 : Étapes 5 à 8
---------------------------------------------------------------------------------

| Étape | Description |
|-------|-------------|
| **5. Configurer PostgreSQL** | Configurer la base de données dans `application.properties` (URL, utilisateur, mot de passe). Créer les tables à partir des entités. |
| **6. Vérifier JPA** | Vérifier que `spring-boot-starter-data-jpa` est dans `pom.xml`. Assurer l’utilisation correcte des annotations JPA (`@Entity`, `@Table`, etc.). |
| **7. Implémenter les opérations CRUD avec la base de données** | Remplacer les listes en mémoire par une base de données réelle, en utilisant Spring Data JPA pour les opérations CRUD. |
| **8. Tester les fonctionnalités** | Utiliser Postman ou des tests unitaires pour vérifier les requêtes HTTP et les interactions avec la base de données PostgreSQL. |

---------------------------------------------------------------------------------
# Livraison 3 : Étapes 9 à 12
---------------------------------------------------------------------------------

| Étape | Description |
|-------|-------------|
| **9. Implémentation des CRUD complets** | Implémenter toutes les opérations CRUD (Create, Read, Update, Delete) en utilisant Spring Data JPA pour les cartes de crédit. |
| **10. Ajouter Swagger pour la documentation** | Intégrer Swagger dans le projet pour générer automatiquement la documentation des API. |
| **11. Implémenter des méthodes personnalisées** | Ajouter des méthodes spécifiques dans `CardsRepository`, comme la suppression par `cardNumber` et la recherche par `customerId`. |
| **12. Utiliser JPQL** | Utiliser JPQL pour écrire des requêtes personnalisées, par exemple, récupérer toutes les cartes d’un type donné (Visa, MasterCard, etc.). |

---------------------------------------------------------------------------------
# Livraison 4 : Implémentations du laboratoire et gestion des codes de retour
---------------------------------------------------------------------------------

| Fonctionnalité | Description | Endpoint | Réponses attendues |
|----------------|-------------|----------|--------------------|
| **1. Suppression de Carte par Numéro** | Supprime une carte à l'aide de son numéro. | `DELETE /deleteCardByNumber/{cardNumber}` | 200 (Succès), 404 (Carte non trouvée), 500 (Erreur interne) |
| **2. Suppression des Cartes d'un Client** | Supprime toutes les cartes associées à un client. | `DELETE /deleteCardsByCustomerId/{customerId}` | 200 (Succès), 404 (Aucune carte trouvée), 500 (Erreur interne) |
| **3. Suppression en Lots** | Supprime plusieurs cartes spécifiées par leurs IDs. | `DELETE /deleteMultipleCards` | 200 (Succès), 400 (Échec de suppression), 500 (Erreur interne) |
| **4. Mise à Jour de la Limite de Crédit** | Met à jour la limite de crédit d'une carte. | `PUT /updateCardLimit/{cardId}` | 200 (Mise à jour réussie), 404 (Carte non trouvée), 400 (Données invalides) |
| **5. Recherche de Cartes par Type** | Récupère toutes les cartes d'un type spécifié (Visa, MasterCard). | `GET /cardsByType/{cardType}` | 200 (Liste de cartes), 404 (Aucune carte trouvée) |

La **Livraison 4** inclut la mise en œuvre des fonctionnalités spécifiques demandées dans le laboratoire, ainsi que la gestion des codes de retour HTTP pour chaque fonctionnalité (succès, erreurs, etc.). Les étudiants devront tester ces endpoints et documenter leur fonctionnement avec Swagger.

# Références :

- https://github.com/hrhouma1/WebTransact/blob/main/99-evaluations-ete-2024/LABORATOIRE1/README.md
- https://github.com/hrhouma1/cards
- 

