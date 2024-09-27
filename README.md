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

