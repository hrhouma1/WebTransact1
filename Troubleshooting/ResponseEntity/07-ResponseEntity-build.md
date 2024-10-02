# C'est quoi le .build() dans ResponseEntity ?

Le `.build()` dans Spring Boot est souvent utilisé en combinaison avec certaines méthodes statiques de `ResponseEntity`, comme `noContent()`, `notFound()`, etc., pour **finaliser** la construction d'une instance de `ResponseEntity`. Cela permet de créer une réponse HTTP sans spécifier explicitement un corps, en se concentrant uniquement sur le statut et les en-têtes.

### Explication détaillée

Lorsque vous utilisez `ResponseEntity`, vous pouvez soit renvoyer un corps de réponse, soit renvoyer simplement un statut HTTP avec ou sans en-têtes. Le `.build()` est utilisé pour **construire** et finaliser la réponse lorsque vous ne renvoyez pas de corps (ou que vous laissez le corps vide).

### Cas d'utilisation typiques de `.build()`

1. **Réponse sans corps** (comme un statut `204 No Content` ou `404 Not Found`).
2. **Réponse uniquement avec des en-têtes et un statut**.

### Exemples d'utilisation

#### 1. **Réponse avec `204 No Content`**

Cela signifie que la requête a été traitée correctement, mais il n'y a pas de contenu à renvoyer dans la réponse (comme pour une suppression réussie).

```java
@GetMapping("/customers")
public ResponseEntity<Void> getAllCustomers() {
    List<Customer> customers = customerService.findAll();
    if (customers.isEmpty()) {
        return ResponseEntity.noContent().build();  // Statut 204 No Content, aucun corps
    }
    return ResponseEntity.ok(customers);  // Statut 200 OK, avec la liste des clients
}
```

- **`noContent()`** : Indique que la réponse est un statut HTTP 204 (No Content).
- **`build()`** : Finalise la réponse sans ajouter de corps.

#### 2. **Réponse avec `404 Not Found`**

Lorsque la ressource demandée n'est pas trouvée, vous pouvez renvoyer un statut `404` sans corps :

```java
@GetMapping("/customers/{id}")
public ResponseEntity<Customer> getCustomerById(@PathVariable Long id) {
    Optional<Customer> customer = customerService.findById(id);
    if (!customer.isPresent()) {
        return ResponseEntity.notFound().build();  // Statut 404 Not Found, aucun corps
    }
    return ResponseEntity.ok(customer.get());  // Statut 200 OK, avec le client trouvé
}
```

- **`notFound()`** : Prépare une réponse HTTP 404.
- **`build()`** : Finalise la réponse sans corps.

#### 3. **Réponse avec en-têtes mais sans corps**

Vous pouvez ajouter des en-têtes à la réponse sans spécifier de corps.

```java
@DeleteMapping("/customers/{id}")
public ResponseEntity<Void> deleteCustomer(@PathVariable Long id) {
    customerService.deleteById(id);
    return ResponseEntity.noContent()
            .header("Custom-Header", "HeaderValue")
            .build();  // Statut 204 No Content avec un en-tête personnalisé
}
```

- **`header()`** : Ajoute un en-tête à la réponse.
- **`build()`** : Finalise la réponse avec les en-têtes mais sans corps.



---------------------
###  Exemple réponses **sans corps** et les réponses **avec corps** dans le cadre de l'utilisation de `ResponseEntity` en Spring Boot :
---------------------

| **Critère**                     | **Réponse sans corps**                             | **Réponse avec corps**                                 |
|----------------------------------|---------------------------------------------------|-------------------------------------------------------|
| **Description**                 | Réponses qui ne contiennent que le code de statut HTTP, éventuellement accompagnées d'en-têtes. | Réponses contenant à la fois un code de statut HTTP et un contenu (données) dans le corps de la réponse. |
| **Méthode de création**          | Utilisation de méthodes statiques comme `noContent()`, `notFound()`, ou en spécifiant uniquement le statut. | Utilisation de méthodes comme `ok()`, ou en passant un objet dans le corps de la réponse. |
| **Méthode `build()` utilisée**   | Oui, `build()` est utilisé pour finaliser la réponse sans ajouter de corps. | Non, on utilise directement `body()` pour spécifier le contenu de la réponse. |
| **Statuts HTTP typiques**        | - `204 No Content` (pas de contenu à renvoyer) <br> - `404 Not Found` (ressource non trouvée) <br> - `301 Moved Permanently` (redirection sans contenu) | - `200 OK` (réponse avec contenu) <br> - `201 Created` (nouvelle ressource créée avec un contenu) <br> - `400 Bad Request` (contenu d'erreur) |
| **Exemple d'utilisation**        | ```java ResponseEntity.noContent().build(); ``` | ```java ResponseEntity.ok(data); ``` |
| **Utilisation des en-têtes**     | Peut contenir des en-têtes HTTP (ex: informations de cache ou de redirection) sans corps. | Peut contenir à la fois des en-têtes et un corps de réponse (par exemple, JSON, HTML, ou fichier). |
| **Cas d'utilisation typiques**   | - Suppression d'une ressource (`DELETE`) <br> - Redirections (`301`, `302`) <br> - Ressource non trouvée (`404`) | - Renvoi de données au client (JSON, XML, etc.) <br> - Retour d'un nouvel objet créé (`201 Created`) <br> - Envoi de fichiers (PDF, image, etc.) |
| **Impact sur la performance**    | Légèrement plus performant, car aucun corps de réponse à générer ou à transférer. | Moins performant si le corps de la réponse est volumineux, car il y a plus de données à traiter et à envoyer. |
| **Exemple concret**              | - Suppression réussie d'un client : <br> ```java return ResponseEntity.noContent().build(); ``` <br> - Ressource non trouvée : <br> ```java return ResponseEntity.notFound().build(); ``` | - Retour d'une liste d'utilisateurs : <br> ```java return ResponseEntity.ok(usersList); ``` <br> - Création d'une ressource : <br> ```java return ResponseEntity.status(HttpStatus.CREATED).body(newUser); ``` |

### Détails supplémentaires :

1. **Réponse sans corps :**
   - Utilisée principalement lorsque la réponse ne nécessite pas d'envoyer de données au client. Le code de statut HTTP et les en-têtes suffisent à informer le client de l'état de la requête.
   - Typiquement vue avec les opérations **DELETE** (204 No Content), ou lorsqu’une ressource est absente (404 Not Found).
   - Utilisation de méthodes comme `ResponseEntity.noContent()`, `ResponseEntity.notFound()`, ou encore `ResponseEntity.status(HttpStatus.NO_CONTENT).build()`.

2. **Réponse avec corps :**
   - Utilisée lorsque des données doivent être renvoyées au client, comme une ressource créée ou récupérée, des erreurs détaillées, ou des résultats de traitement.
   - Typiquement vue avec les opérations **GET** et **POST**, où le contenu renvoyé peut être sous forme de JSON, HTML, XML, ou autre.
   - Utilisation de méthodes comme `ResponseEntity.ok(body)`, `ResponseEntity.status(HttpStatus.CREATED).body(newEntity)`.

En résumé, les réponses sans corps sont utilisées lorsque seuls le statut et les en-têtes sont nécessaires, tandis que les réponses avec corps contiennent des données pertinentes que le client doit recevoir.

### Conclusion

Le `.build()` est utilisé pour finaliser la construction d'une instance de `ResponseEntity` sans spécifier de corps, généralement dans les cas où seule une réponse HTTP sans contenu (comme `204 No Content` ou `404 Not Found`) ou avec des en-têtes spécifiques est requise. Il permet de définir un statut et des en-têtes, tout en laissant le corps de la réponse vide.
