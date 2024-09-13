----
# Plan de contenu
----

 - **Maven** :
  - Introduction à Maven : gestionnaire de projets
  - Installation de Maven
  - Cycle de vie d'un projet Maven (mvn clean, mvn compile, mvn test, mvn spring-boot:run)

- **PostgreSQL** :
  - Installation et configuration de PostgreSQL
  - Création d'une base de données avec gestion des permissions et autorisations

- **VSCode** :
  - Installation de VSCode
  - Installation des extensions pour Maven, Java, et Spring Boot

- **Fichier `application.properties`** :
  - Configuration de la connexion à PostgreSQL (URL, nom d'utilisateur, mot de passe)
  - Paramètres pour H2 (base de données en mémoire)
  - Configuration du port d'application et autres paramètres spécifiques à l'application

- **Spring Boot Starter** :
  - Importation d'un projet Spring Boot
  - Exécution du projet pour la première fois

- **Swagger** :
  - Introduction à Swagger pour la documentation des API REST
  - Installation de la dépendance Swagger via Maven (`springfox-swagger2` et `springfox-swagger-ui`)
  - Accès à l'interface Swagger à l'URL : `/swagger-ui.html`
  - Documentation des endpoints REST automatiquement avec Swagger

- **Base de données H2** :
  - Configuration de la base de données H2 en tant que base de données en mémoire
  - Utilisation pour les tests et le développement local
  - Accès à la console H2 via l'URL : `/h2-console`
  - Paramétrage dans `application.properties` pour permettre la connexion automatique à H2

- **Spring Boot et REST** :
  - Création d'un service web simple avec REST
  - Structure d'un contrôleur REST
  - Explication des annotations Spring Boot : 
    - **@RestController** : Définit un contrôleur REST
    - **@RequestMapping** : Définit le mapping des requêtes HTTP
    - **@GetMapping, @PostMapping, @PutMapping, @DeleteMapping** : Gèrent les différents types de requêtes HTTP

- **Architecture MVC (Modèle-Vue-Controlleur)** :
  - Structure d'un projet Spring Boot : contrôleurs, services, répertoires
  - Rôle de chaque composant

- **JPA (Java Persistence API)** :
  - Ajout de la dépendance JPA dans Maven
  - Configuration de la connexion à PostgreSQL ou H2 via **application.properties**
  - **Annotations JPA** :
    - **@Entity** : Définit une classe comme entité JPA
    - **@Id** : Indique le champ qui sera la clé primaire
    - **@GeneratedValue** : Génère automatiquement la valeur de la clé primaire
    - **@Column** : Spécifie un champ comme une colonne de base de données
    - **@ManyToOne, @OneToMany, @OneToOne** : Gère les relations entre entités
  - Utilisation des repositories pour interagir avec la base de données via **@Repository**
  - **JPQL (Java Persistence Query Language)** :
    - Introduction aux requêtes JPQL
    - Exemples de requêtes pour récupérer des entités, joindre des tables, etc.

- **Repositories et règles de base** :
  - Utilisation des méthodes standards des repositories :
    - **findById** : Récupérer une entité par son ID
    - **findAll** : Récupérer toutes les entités
    - **save** : Enregistrer ou mettre à jour une entité
    - **deleteById** : Supprimer une entité par son ID
  - Personnalisation des méthodes de repository :
    - **findByNom** : Créer des méthodes personnalisées basées sur le nom des attributs de l'entité

- **Journalisation (Logging)** :
  - Introduction à la journalisation dans Spring Boot
  - Utilisation de SLF4J et Logback
  - Configuration des niveaux de log (DEBUG, INFO, WARN, ERROR)
  - **Logging avancé** :
    - Personnalisation des fichiers de logs
    - Différentes stratégies de rotation des logs (taille, date)
    - Paramétrage de loggers spécifiques pour des classes ou des packages
    - Gestion des niveaux de log dans `application.properties` ou `logback-spring.xml`

- **Docker** :
  - Introduction à Docker
  - Création d'un `Dockerfile` pour une application Spring Boot
  - Construction de l'image Docker : `docker build -t nom_de_l_image .`
  - Lancement de l'application via Docker : `docker run -p 8080:8080 nom_de_l_image`
  - Avantages de l'utilisation de Docker pour déployer des applications Spring Boot

- **Déploiement sur AWS** :
  - Création d'une instance EC2
  - Configuration du serveur (installation de Java, Docker, PostgreSQL)
  - Déploiement de l'application Spring Boot sur l'instance EC2 avec Docker
  - Gestion des accès via un groupe de sécurité AWS (ouverture du port 8080)
  - Validation de l'application via l'IP publique de l'instance

- **Déploiement sur Azure** :
  - Création d'un App Service sur Azure
  - Utilisation de Docker pour déployer l'application Spring Boot
  - Configuration de la base de données PostgreSQL sur Azure
  - Connexion de l'application à PostgreSQL via les paramètres Azure
  - Accès à l'application via l'URL publique fournie par Azure

