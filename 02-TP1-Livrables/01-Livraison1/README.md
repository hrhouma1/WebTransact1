# Milestone 1 : Introduction à Spring Boot et JPA

## 🎯 Objectif
Ce milestone vise à vous familiariser avec les bases de Spring Boot et JPA à travers une série d'exercices pratiques.

## 📚 Contenu
Le Milestone 1 comprend les étapes suivantes :

### 📌 Étape 1 : Échauffement avec Spring Boot
- Fichier : [2.3. Application-Pratique1-Exercice1.md](2.3.Application-Pratique1-Exercice1.md)
- Objectif : Créer une application Spring Boot simple
- Poids : 10% de la note finale

### 📌 Étape 2 (Optionnelle) : Gestion en mémoire avec REST
- Fichier : [2.4. ApplicationPratique-Exercice1-Suite-Optionnel.md](2.4.ApplicationPratique-Exercice1-Suite-Optionnel.md)
- Objectif : Implémenter un service REST pour gérer l'entité Customer en mémoire
- Poids : Bonus

### 📌 Étape 3 : Introduction à JPA
- Fichier : [3.3. Application-Pratique2-Exercice2.md](3.3.Application-Pratique2-Exercice2.md)
- Objectif : Ajouter la classe Customer avec JPA et implémenter des méthodes de base
- Tâches :
  1. Récupérer toutes les cartes (GET)
  2. Ajouter une carte (POST)
- Poids : 18% de la note finale
- Ressource supplémentaire : [3.3.Troubleshooting-add.md](3.3.Troubleshooting-add.md) pour le dépannage

## 🛠 Compétences développées
- Configuration de base de Spring Boot
- Création de contrôleurs REST simples
- Utilisation de JPA pour la persistance des données
- Gestion des entités et des relations de base

## ⏭ Prochaines étapes
Après avoir complété ce milestone, vous serez prêt(e) à aborder des concepts plus avancés dans les prochains milestones, notamment la mise en œuvre complète d'une API REST et la gestion de relations plus complexes entre entités.

## 💡 Conseils
- Prenez le temps de bien comprendre chaque concept avant de passer à l'étape suivante.
- N'hésitez pas à consulter la documentation officielle de Spring Boot et JPA en cas de doute.
- La pratique est clé : essayez de coder par vous-même autant que possible.

Bon courage pour ce premier milestone ! 🚀





------------------------------------------------------------------------------------------
# Réflexion d'équipe sur le Milestone 1 et perspectives pour le Milestone 2
------------------------------------------------------------------------------------------

Chers collègues,

Félicitations pour votre excellent travail sur le Milestone 1 ! Vous avez posé les bases solides de notre application de gestion de cartes bancaires. Lors de notre dernière réunion d'équipe, nous avons eu une discussion enrichissante sur les défis rencontrés et les améliorations à apporter. Voici un résumé de nos échanges :

## 🔍 Problèmes identifiés et réflexions de l'équipe

### 1. Dépendance à l'existence d'un client
Eric a soulevé un point important : "J'ai remarqué que notre application renvoie une erreur 500 lorsqu'on essaie d'ajouter une carte sans client existant. Ne devrions-nous pas gérer cela de manière plus élégante ?"

### 2. Duplication des numéros de carte
Alamiro a partagé une observation pertinente : "En testant l'application, j'ai pu ajouter plusieurs cartes avec le même numéro. Cela pourrait causer des problèmes de gestion à long terme, non ?"

### 3. Anticipation des dépendances cycliques
Sarah, toujours prévoyante, a mentionné : "Pour l'instant, nous n'avons qu'un seul modèle, mais que se passera-t-il quand nous en ajouterons d'autres ? Ne risquons-nous pas des problèmes de dépendances cycliques ?"

## 🚀 Plan d'action pour le Milestone 2

Après avoir écouté vos réflexions, voici ce que notre chef de projet, Haythem, a proposé pour le Milestone 2 :

"Excellent travail d'équipe, tout le monde ! Vos observations sont très pertinentes. Voici comment nous allons aborder ces défis dans le prochain milestone :

1. **Gestion des clients** : Nous allons implémenter une vérification de l'existence du client avant l'ajout d'une carte. Si le client n'existe pas, nous renverrons un message d'erreur clair plutôt qu'une erreur 500.

2. **Unicité des numéros de carte** : Nous ajouterons une contrainte d'unicité sur le numéro de carte dans notre base de données et mettrons en place une validation côté serveur pour éviter les doublons.

3. **Préparation à l'expansion** : Bien que nous n'ayons pas encore de problèmes de dépendances cycliques, nous allons préparer notre architecture pour l'ajout futur de nouveaux modèles. Nous explorerons l'utilisation de DTO et des annotations spécifiques de Jackson pour gérer les relations complexes.

4. **Améliorations générales** :
   - Mise en place d'une gestion d'erreurs plus robuste.
   - Ajout de validations pour tous les champs des entités.
   - Création de tests unitaires et d'intégration.
   - Début de la documentation de notre API.

Je suis vraiment impressionné par votre perspicacité et votre travail d'équipe. Ces améliorations vont considérablement renforcer notre application. Préparez-vous pour un Milestone 2 passionnant et enrichissant !"

Continuez votre excellent travail, et n'oubliez pas : chaque défi est une opportunité d'apprentissage et d'amélioration. Bonne chance pour la suite du projet !
```


-------------------------------------------------------------------------------
# Solutions innovantes pour le Milestone 2 : Contributions de l'équipe
-------------------------------------------------------------------------------

Chers collègues,

Suite à notre discussion d'équipe sur les défis rencontrés dans le Milestone 1, plusieurs de vos camarades ont proposé des solutions innovantes. Voici un aperçu de leurs brillantes suggestions :

## 1. Gestion de la dépendance client

**Problème** : L'application renvoie une erreur 500 lors de l'ajout d'une carte sans client existant.

**Solution proposée par Elena et Yuki** : 
Elena a suggéré : "Créons un endpoint pour vérifier l'existence d'un client avant d'ajouter une carte."
Yuki a complété : "Nous pourrions aussi implémenter un gestionnaire d'exceptions global pour traiter ce cas spécifique."

## 2. Duplication des numéros de carte

**Problème** : Possibilité d'ajouter plusieurs cartes avec le même numéro.

**Solution proposée par Viktor et Léa** :
Viktor a proposé : "Ajoutons une contrainte d'unicité sur la colonne `card_number` dans notre base de données."
Léa a ajouté : "Nous devrions aussi faire une vérification côté serveur avant chaque insertion."

## 3. Anticipation des dépendances cycliques

**Problème** : Risque de dépendances cycliques avec l'ajout futur de nouveaux modèles.

**Solution proposée par Chen et Sophia** :
Chen a suggéré : "Adoptons une architecture en couches avec des DTOs pour séparer la représentation des données."
Sophia a complété : "L'utilisation de MapStruct pour le mapping entre entités et DTOs pourrait être très utile ici."

## 4. Amélioration générale de la qualité du code (nous allons l'inclure dans un autre milestone)

Plusieurs membres de l'équipe ont contribué avec des idées :

- Luca : "Implémentons des validations avec Bean Validation sur nos DTOs."
- Nadia : "Créons une classe `GlobalExceptionHandler` pour centraliser la gestion des exceptions."
- Ravi : "Utilisons Swagger pour documenter automatiquement notre API."
- Emilie : "Mettons en place des tests unitaires avec JUnit et Mockito."

Le chef de projet, Thomas, a été impressionné par ces propositions : "Vos idées sont excellentes et démontrent une vraie réflexion d'équipe. Elles formeront la base de notre travail pour le Milestone 2."

Il a ajouté : "Je suis particulièrement satisfait de voir comment vous avez tous contribué, en apportant vos perspectives uniques. C'est exactement ce type de collaboration qui mène à des solutions robustes et innovantes."

Préparez-vous à mettre en œuvre ces solutions passionnantes dans la prochaine phase de notre projet. C'est une excellente opportunité pour approfondir vos connaissances en développement Spring Boot et en bonnes pratiques de programmation, tout en travaillant en équipe !

Bon courage à tous pour le Milestone 2 !
