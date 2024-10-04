
## **Étude de cas : Débogage et amélioration d'un projet Spring Boot**

### **Contexte**
- Dans ce projet Spring Boot, nous avons rencontré plusieurs problèmes liés à la gestion des entités, des repositories, et des contrôleurs, qui ont conduit à des erreurs lors de l'exécution de l'application. 
- Ces erreurs sont typiques dans les projets utilisant Spring Data JPA et les bases de données relationnelles.
- L'objectif de cette étude de cas est de passer en revue ces erreurs, d'expliquer pourquoi elles surviennent, et de fournir des solutions pour les corriger.
- Cette étude a également pour but d'enseigner de bonnes pratiques de développement à vos futurs projets.

# Voici le projet d'origine avec des problèmes



### **Problème 1 : Incohérence des types de données**
Dans le repository lié à l'entité `Customer`, une méthode a été définie pour rechercher un client par `customerId`. Cependant, le type de données utilisé dans la méthode ne correspondait pas au type de données du champ `customerId` dans l'entité `Customer`.

#### **Erreur rencontrée :**
```java
Card findByCustomerId(String customerId);
```
Ici, `customerId` était défini comme un `String`, alors que dans l'entité, il était de type `Integer`.

#### **Explication :**
Les méthodes de repository doivent utiliser des types de données cohérents avec ceux des entités pour éviter des erreurs de mapping ou de conversion dans la base de données.

#### **Solution :**
Modifier la signature de la méthode pour utiliser le type `Integer`, qui correspond à la définition dans l'entité `Customer` :
```java
Customer findByCustomerId(Integer customerId);
```

### **Problème 2 : Retour de type incorrect dans les méthodes**
Dans la méthode mentionnée ci-dessus, la méthode retournait un objet de type `Card`, alors que la recherche se faisait dans le repository lié à l'entité `Customer`. Cela a généré une incompatibilité de types.

#### **Erreur rencontrée :**
```java
Card findByCustomerId(Integer customerId);
```
La méthode devrait renvoyer un `Customer` et non un `Card`.

#### **Solution :**
Corriger la méthode pour qu’elle retourne un objet de type `Customer` :
```java
Customer findByCustomerId(Integer customerId);
```

### **Problème 3 : Mauvaise gestion des requêtes JPQL**
Dans le repository `CardRepository`, une requête JPQL était mal écrite pour supprimer des cartes basées sur `customerId`. L'erreur se produisait dans la façon dont le champ `customerId` était référencé dans la requête.

#### **Erreur rencontrée :**
```java
@Modifying
@Query("DELETE Card c WHERE c.customer.id = ?1")
```
Le champ `id` n'était pas correct. Dans l'entité `Customer`, le champ correspondant était `customerId`.

#### **Solution :**
Modifier la requête JPQL pour référencer correctement le champ `customerId` :
```java
@Modifying
@Query("DELETE FROM Card c WHERE c.customer.customerId = ?1")
void deleteCardsByCustomerId(int customerId);
```
En plus, l'annotation `@Transactional` doit être ajoutée pour garantir que cette opération de suppression est gérée dans une transaction.

### **Problème 4 : Manque d'annotations pour les opérations de modification**
Les méthodes de suppression dans le repository n'étaient pas correctement annotées avec `@Modifying` et `@Transactional`, ce qui est nécessaire pour indiquer à Spring qu’il s’agit d’une opération de modification des données.

#### **Solution :**
Assurez-vous d'ajouter les annotations `@Modifying` et `@Transactional` aux méthodes qui modifient les données :
```java
@Transactional
@Modifying
@Query("DELETE FROM Card c WHERE c.customer.customerId = ?1")
void deleteCardsByCustomerId(int customerId);
```

### **Problème 5 : Utilisation de conditions inutiles dans le code**
Dans le contrôleur, une condition inutile `if (true)` était utilisée pour supprimer une carte par numéro, ce qui rendait le code peu clair et difficile à comprendre.

#### **Erreur rencontrée :**
```java
if (true) {
    Card card = cardRepository.findByCardNumber(cardNumber);
    cardRepository.delete(card);
}
```

#### **Solution :**
Remplacer cette condition par une vérification logique qui vérifie si la carte existe dans la base de données avant de tenter de la supprimer :
```java
Card card = cardRepository.findByCardNumber(cardNumber);
if (card != null) {
    cardRepository.delete(card);
    return ResponseEntity.ok().build();
} else {
    return ResponseEntity.notFound().build();
}
```
Cette logique améliore la lisibilité et la robustesse du code, en garantissant que la carte n’est supprimée que si elle existe.

### **Problème 6 : Méthodes redondantes dans le repository**
Il y avait une méthode redondante dans le repository pour supprimer des cartes par `cardId`, appelée `deleteAllByIdIn(List<Integer>)`, alors qu’une autre méthode, plus précise, existait déjà : `deleteAllByCardIdIn(List<Integer>)`.

#### **Solution :**
Supprimer les méthodes redondantes pour simplifier le code et éviter les ambiguïtés :
```java
void deleteAllByCardIdIn(List<Integer> cardIds);
```

### **Problème 7 : Gestion inadéquate des réponses HTTP**
Dans certaines méthodes du contrôleur, les réponses HTTP n'étaient pas correctement gérées. Par exemple, lorsqu’une carte n’était pas trouvée, aucune réponse appropriée (comme `404 Not Found`) n’était renvoyée.

#### **Solution :**
Mettre en place une gestion correcte des réponses HTTP, notamment pour les cas où les ressources ne sont pas trouvées ou lorsqu'une opération est réussie :
```java
if (card != null) {
    cardRepository.delete(card);
    return ResponseEntity.ok().build();
} else {
    return ResponseEntity.notFound().build();
}
```
Cette pratique garantit que les utilisateurs reçoivent des réponses appropriées en fonction de l'état des opérations.

---

### **Conclusion et recommandations**

Dans ce projet, nous avons identifié plusieurs erreurs courantes liées à la gestion des entités, des repositories, et des contrôleurs. Voici quelques recommandations pour éviter ces erreurs à l'avenir :
1. **Vérifiez toujours la cohérence des types de données** entre vos méthodes de repository et vos entités.
2. **Utilisez les annotations appropriées** (`@Modifying`, `@Transactional`) pour les opérations de modification dans la base de données.
3. **Gérez correctement les réponses HTTP** dans vos contrôleurs pour offrir une expérience utilisateur cohérente.
4. **Évitez la duplication de méthodes** dans vos repositories pour simplifier la maintenance du code.
5. **Testez régulièrement vos requêtes JPQL** pour vous assurer qu'elles référencent les bons champs dans vos entités.

Appliquer ces bonnes pratiques dans vos projets futurs améliorera la robustesse et la lisibilité de votre code, tout en réduisant les risques d'erreurs.
