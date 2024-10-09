# Méthodes de **ResponseEntity** qu'on peut utiliser dans Spring Boot, avec des exemples de combinaisons pour chaque méthode.

| **Méthode**                               | **Description**                                                                 | **Combinaison possible**                                                   | **Exemple de code**                                                                                                                                   |
|-------------------------------------------|---------------------------------------------------------------------------------|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| `ResponseEntity.ok()`                     | Retourne une réponse avec statut **200 OK**.                                    | Peut être combinée avec **.body()** pour ajouter un corps à la réponse.    | `ResponseEntity.ok(user);` <br> `ResponseEntity.ok().body(user);`                                                                                     |
| `ResponseEntity.status(HttpStatus)`       | Spécifie un statut HTTP personnalisé (ex : **201 Created**, **400 Bad Request**)| Peut être combinée avec **.body()** pour ajouter un corps ou **.build()** pour une réponse sans corps. | `ResponseEntity.status(HttpStatus.CREATED).body(newUser);` <br> `ResponseEntity.status(HttpStatus.NOT_FOUND).build();`                                 |
| `ResponseEntity.body()`                   | Ajoute un corps à la réponse.                                                   | Combinée avec **.ok()**, **.status()** ou toute autre méthode statique.    | `ResponseEntity.ok().body("Hello World!");` <br> `ResponseEntity.status(HttpStatus.CREATED).body(savedEntity);`                                       |
| `ResponseEntity.noContent()`              | Retourne une réponse avec le statut **204 No Content**.                         | Peut être combinée avec **.build()** pour finaliser la réponse sans corps. | `ResponseEntity.noContent().build();`                                                                                                                |
| `ResponseEntity.notFound()`               | Retourne une réponse avec le statut **404 Not Found**.                          | Peut être combinée avec **.build()** pour finaliser la réponse sans corps. | `ResponseEntity.notFound().build();`                                                                                                                 |
| `ResponseEntity.badRequest()`             | Retourne une réponse avec le statut **400 Bad Request**.                        | Peut être combinée avec **.body()** pour inclure des détails d'erreur.     | `ResponseEntity.badRequest().body("Invalid request");`                                                                                               |
| `ResponseEntity.accepted()`               | Retourne une réponse avec le statut **202 Accepted**.                           | Peut être combinée avec **.body()** pour ajouter un corps à la réponse.    | `ResponseEntity.accepted().body("Request accepted for processing");`                                                                                  |
| `ResponseEntity.created(URI)`             | Retourne une réponse avec le statut **201 Created** et un en-tête **Location**. | Peut être combinée avec **.body()** pour ajouter le corps de la ressource. | `ResponseEntity.created(locationURI).body(newResource);`                                                                                              |
| `ResponseEntity.headers(HttpHeaders)`     | Ajoute des en-têtes HTTP personnalisés.                                         | Peut être combinée avec **.ok()**, **.status()**, **.body()**, etc.        | `ResponseEntity.ok().headers(customHeaders).body(data);`                                                                                              |
| `ResponseEntity.build()`                  | Finalise la création de la réponse sans ajouter de corps.                       | Combinée avec **.status()**, **.noContent()**, **.notFound()**, etc.       | `ResponseEntity.noContent().build();` <br> `ResponseEntity.status(HttpStatus.NOT_FOUND).build();`                                                     |
| `ResponseEntity.header(String, String)`   | Ajoute un en-tête personnalisé avec une clé et une valeur.                      | Peut être combinée avec **.status()**, **.ok()**, **.body()**, etc.        | `ResponseEntity.ok().header("Custom-Header", "value").body(data);`                                                                                    |
| `ResponseEntity.contentType(MediaType)`   | Spécifie le type de contenu de la réponse (ex : **application/json**).          | Peut être combinée avec **.status()**, **.body()**, **.headers()**, etc.   | `ResponseEntity.ok().contentType(MediaType.APPLICATION_JSON).body(data);`                                                                             |
| `ResponseEntity.location(URI)`            | Spécifie un en-tête **Location** (utilisé avec le statut **201 Created**).      | Peut être combinée avec **.status()** et **.body()**.                      | `ResponseEntity.created(locationURI).body(newResource);`                                                                                              |

### Exemples de combinaisons :

1. **Réponse OK avec un corps** :
   ```java
   return ResponseEntity.ok().body("Hello, World!");
   ```

2. **Réponse Created avec en-têtes personnalisés** :
   ```java
   HttpHeaders headers = new HttpHeaders();
   headers.add("Custom-Header", "Value");
   return ResponseEntity.status(HttpStatus.CREATED)
           .headers(headers)
           .body(newUser);
   ```

3. **Réponse No Content avec en-tête personnalisé** :
   ```java
   return ResponseEntity.noContent()
           .header("X-Empty-Response", "true")
           .build();
   ```

4. **Réponse Bad Request avec un corps** :
   ```java
   return ResponseEntity.badRequest().body("Invalid data provided");
   ```

5. **Réponse avec un fichier PDF (exemple avec des en-têtes et le corps)** :
   ```java
   HttpHeaders headers = new HttpHeaders();
   headers.add(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=document.pdf");
   byte[] document = {};  // Contenu du fichier PDF
   return ResponseEntity.ok()
           .headers(headers)
           .contentType(MediaType.APPLICATION_PDF)
           .body(document);
   ```

### Conclusion :
- Tu peux combiner ces différentes méthodes pour personnaliser totalement la réponse HTTP que tu renvoies, en incluant le corps, les en-têtes, et en choisissant le bon statut HTTP. 
- Ces combinaisons permettent une grande flexibilité dans la gestion des réponses dans tes API REST.
