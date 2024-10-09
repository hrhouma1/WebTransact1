# Utilisation de ResponseEntity dans Spring Boot

ResponseEntity est une classe puissante de Spring qui permet de personnaliser les réponses HTTP. Voici quelques exemples d'utilisation courante :

## Réponse avec corps

### Réponse OK (200)

```java
@GetMapping("/users/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    User user = userService.findById(id);
    return ResponseEntity.ok(user);
}
```

### Réponse Created (201)

```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@RequestBody User newUser) {
    User savedUser = userService.save(newUser);
    return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
}
```

## Réponse sans corps

### No Content (204)

```java
@DeleteMapping("/users/{id}")
public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
    userService.deleteById(id);
    return ResponseEntity.noContent().build();
}
```

### Not Found (404)

```java
@GetMapping("/users/{id}")
public ResponseEntity<User> getUser(@PathVariable Long id) {
    Optional<User> user = userService.findById(id);
    return user.map(ResponseEntity::ok)
               .orElseGet(() -> ResponseEntity.notFound().build());
}
```

## Réponse avec en-têtes personnalisés

```java
@GetMapping("/custom-header")
public ResponseEntity<String> customHeader() {
    return ResponseEntity.ok()
                         .header("Custom-Header", "value")
                         .body("Custom header set");
}
```

## Réponse avec code d'état et corps personnalisés

```java
@PostMapping("/users")
public ResponseEntity<Object> createUser(@RequestBody User user) {
    if (userService.isInvalid(user)) {
        return ResponseEntity
                .badRequest()
                .body("Invalid user data");
    }
    User savedUser = userService.save(user);
    return ResponseEntity
            .status(HttpStatus.CREATED)
            .body(savedUser);
}
```

Ces exemples montrent comment utiliser ResponseEntity pour contrôler précisément le statut HTTP, le corps et les en-têtes de la réponse dans vos contrôleurs Spring Boot.

