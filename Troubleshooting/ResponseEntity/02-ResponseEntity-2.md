## Documentation 2: Utilisation de `ResponseEntity` dans Spring Boot

### Introduction
La classe `ResponseEntity` est une extension de la classe `HttpEntity`, qui permet de manipuler des réponses HTTP complètes, y compris les en-têtes, le corps et le code d'état. Elle est largement utilisée dans Spring Boot pour renvoyer des réponses personnalisées à partir des méthodes des contrôleurs.

### Structure d'une `ResponseEntity`
Une instance de `ResponseEntity` contient :
1. **Le corps de la réponse** : Les données que vous souhaitez renvoyer.
2. **Les en-têtes HTTP** : Optionnels, ils permettent de personnaliser la réponse HTTP.
3. **Le code d'état HTTP** : Indique le succès ou l'échec de la requête (par exemple, 200 OK, 404 Not Found, etc.).

### Création d'une `ResponseEntity`

#### 1. Utilisation du constructeur
Vous pouvez créer une `ResponseEntity` directement en utilisant son constructeur, comme suit :
```java
ResponseEntity<String> response = new ResponseEntity<>("Contenu de la réponse", HttpStatus.OK);
```

#### 2. Utilisation de la méthode `ok()`
Spring fournit une méthode statique `ok()` qui permet de simplifier la création de réponses avec un code d'état **200 OK** :
```java
ResponseEntity<String> response = ResponseEntity.ok("Contenu de la réponse");
```

#### 3. Réponses avec en-têtes
Pour ajouter des en-têtes personnalisés à votre réponse, vous pouvez utiliser la méthode `headers()` :
```java
HttpHeaders headers = new HttpHeaders();
headers.add("Custom-Header", "Valeur");

ResponseEntity<String> response = new ResponseEntity<>("Contenu avec en-têtes", headers, HttpStatus.OK);
```

#### 4. Réponses avec d'autres codes d'état
Pour créer une réponse avec un autre code d'état (ex. **404 Not Found**), vous pouvez utiliser les méthodes statiques comme `notFound()`, `badRequest()`, ou définir le code d'état directement :
```java
// Réponse avec 404 Not Found
ResponseEntity<String> response = ResponseEntity.status(HttpStatus.NOT_FOUND).body("Ressource non trouvée");
```

### Utilisation dans un contrôleur

Voici un exemple de l'utilisation de `ResponseEntity` dans un contrôleur Spring Boot :
```java
@RestController
@RequestMapping("/api")
public class MonController {

    @GetMapping("/message")
    public ResponseEntity<String> obtenirMessage() {
        return ResponseEntity.ok("Bonjour, ceci est un message !");
    }

    @GetMapping("/erreur")
    public ResponseEntity<String> obtenirErreur() {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body("Il y a eu une erreur dans la requête !");
    }

    @GetMapping("/custom")
    public ResponseEntity<String> customResponse() {
        HttpHeaders headers = new HttpHeaders();
        headers.add("X-Custom-Header", "Valeur de l'en-tête");

        return new ResponseEntity<>("Réponse personnalisée", headers, HttpStatus.CREATED);
    }
}
```

### Méthodes utiles de `ResponseEntity`

- **`ok()`** : Crée une réponse avec le code d'état HTTP **200 OK**.
- **`created(URI location)`** : Crée une réponse avec **201 Created** et définit l'emplacement de la ressource créée.
- **`accepted()`** : Crée une réponse avec le code **202 Accepted**, indiquant que la requête a été acceptée mais n'est pas encore terminée.
- **`badRequest()`** : Crée une réponse avec **400 Bad Request**.
- **`notFound()`** : Crée une réponse avec **404 Not Found**.
- **`noContent()`** : Crée une réponse avec **204 No Content**, utilisée lorsque la requête a réussi, mais il n'y a rien à renvoyer.

### Exemple complet

```java
@RestController
@RequestMapping("/produits")
public class ProduitController {

    @GetMapping("/{id}")
    public ResponseEntity<Produit> obtenirProduit(@PathVariable Long id) {
        Optional<Produit> produit = produitService.findById(id);
        if (produit.isPresent()) {
            return ResponseEntity.ok(produit.get());  // 200 OK avec le produit en corps
        } else {
            return ResponseEntity.status(HttpStatus.NOT_FOUND).body(null);  // 404 Not Found
        }
    }

    @PostMapping("/")
    public ResponseEntity<Produit> ajouterProduit(@RequestBody Produit produit) {
        Produit produitCree = produitService.save(produit);
        URI location = ServletUriComponentsBuilder
                .fromCurrentRequest()
                .path("/{id}")
                .buildAndExpand(produitCree.getId())
                .toUri();

        return ResponseEntity.created(location).body(produitCree);  // 201 Created avec URI et produit en corps
    }
}
```

### Conclusion
`ResponseEntity` est un outil puissant pour gérer les réponses HTTP dans Spring Boot. Il permet de personnaliser entièrement les réponses que vous renvoyez à partir de vos contrôleurs, en ajoutant des en-têtes, en gérant différents codes d'état, et en fournissant des corps de réponses adaptés aux besoins de votre application.
