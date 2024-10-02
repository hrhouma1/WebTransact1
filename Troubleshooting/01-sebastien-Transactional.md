### Contexte

Sébastien a été chargé de tester trois méthodes de suppression dans son application Spring Boot, toutes liées à la gestion des cartes de crédit dans une base de données. Il s'agit des méthodes suivantes :
1. `deleteAllCard()` – qui supprime toutes les cartes.
2. `deleteById()` – qui supprime une carte en fonction de son identifiant.
3. `deleteByCardNumber()` – qui supprime une carte en fonction de son numéro.

Les deux premières méthodes fonctionnent parfaitement pour lui, mais il rencontre une erreur lors du test de la méthode `deleteByCardNumber()`.

### Problème

Lors de l'exécution de la méthode `deleteByCardNumber()`, Sébastien obtient l'erreur suivante :

```java
jakarta.persistence.TransactionRequiredException: No EntityManager with actual transaction available for current thread - cannot reliably process 'remove' call
```

Cette erreur indique que l'opération de suppression nécessite une transaction active, mais aucune transaction n'est active au moment de l'exécution.

### 1ère tentative de Sébastien

Sébastien commence par tenter de modifier la signature de la méthode de suppression en passant d'un type de retour spécifique (`ResponseEntity<Void>`) à un type plus générique (`ResponseEntity<?>`), dans l'espoir que cela résolve l'erreur. Voici comment il a modifié son code :

```java
@DeleteMapping("/delete/number/{cardNumber}")
public ResponseEntity<?> deleteCardByNumber(@PathVariable String cardNumber) {
    if (cardRepository.existsByCardNumber(cardNumber)) {
        cardRepository.deleteByCardNumber(cardNumber);
        return ResponseEntity.ok().build();
    } else {
        return ResponseEntity.notFound().build();
    }
}
```

Cependant, cette modification ne résout pas le problème, et l'erreur persiste.

### 2ème tentative de Sébastien

Sébastien introduit ensuite l'annotation `@Transactional` pour s'assurer que l'opération de suppression s'effectue dans le cadre d'une transaction. Voici les modifications qu'il apporte :

#### a) Ajout de `@Transactional` au contrôleur

```java
import org.springframework.transaction.annotation.Transactional;

@DeleteMapping("/number/{cardNumber}")
@Transactional
public ResponseEntity<Void> deleteCardNumber(@PathVariable String cardNumber) {
    if (cardRepository.existsByCardNumber(cardNumber)) {
        cardRepository.deleteByCardNumber(cardNumber);
        return ResponseEntity.ok().build();
    } else {
        return ResponseEntity.notFound().build();
    }
}
```

#### b) Ajout de `@Transactional` au repository

Il essaie également une autre approche en appliquant l'annotation `@Transactional` directement au niveau de l'interface `CardRepository` :

```java
import org.springframework.transaction.annotation.Transactional;

public interface CardRepository extends JpaRepository<Card, Integer> {
    boolean existsByCardNumber(String cardNumber);
    
    @Transactional
    void deleteByCardNumber(String cardNumber);
}
```

### Explication de l'erreur et solution

L'erreur initiale provient du fait que l'opération de suppression (`remove()`) nécessite une transaction active. Dans une application Spring Boot, les méthodes CRUD comme `deleteById()` ou `deleteAllCard()` sont automatiquement gérées dans le contexte d'une transaction, car Spring Data JPA les traite en tant que transactions. Cependant, dans le cas de méthodes personnalisées comme `deleteByCardNumber()`, Spring ne les traite pas automatiquement en transaction. C'est pourquoi il est nécessaire d'introduire explicitement l'annotation `@Transactional` pour indiquer à Spring de gérer la transaction pour cette méthode.

### Conclusion

Après avoir ajouté l'annotation `@Transactional` soit au contrôleur, soit au repository, Sébastien devrait maintenant être capable de supprimer une carte en fonction de son numéro sans rencontrer l'erreur de transaction. Il est essentiel de s'assurer que toutes les opérations de suppression ou de modification dans une base de données soient effectuées dans le cadre d'une transaction pour garantir l'intégrité des données.

### Leçons apprises
- Les opérations de modification d'une base de données doivent toujours être effectuées dans une transaction.
- Les méthodes CRUD standard de Spring Data JPA sont automatiquement encapsulées dans des transactions, mais les méthodes personnalisées nécessitent une annotation explicite `@Transactional`.
- Lorsqu'une erreur de transaction apparaît, il faut vérifier si l'annotation `@Transactional` est présente et correctement appliquée.

### Prochaines étapes
Sébastien peut continuer à tester la méthode `deleteByCardNumber()` après avoir appliqué la correction. Il peut également envisager d'ajouter des tests unitaires pour vérifier que toutes les méthodes de suppression fonctionnent correctement dans différents scénarios, comme la suppression d'une carte inexistante, afin de couvrir les cas d'erreur.
