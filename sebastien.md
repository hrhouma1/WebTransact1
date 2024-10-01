

```java
public interface CardRepository extends JpaRepository<Card, Integer> {
    boolean existsByCardNumber(String cardNumber);
    Optional<Card> findByCardNumber(String cardNumber);
    
    // Ajoutez cette méthode
    void deleteByCardNumber(String cardNumber);
}
```


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
