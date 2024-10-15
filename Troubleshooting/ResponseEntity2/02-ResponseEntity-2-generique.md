---------------------------------
# Approche plus générique
---------------------------------
- Je vous présente une approche plus générique pour construire des réponses ResponseEntity en utilisant principalement les méthodes statiques et le pattern builder. 
- Cette approche offre plus de flexibilité et de cohérence dans la construction des réponses.

- Cette approche générique permet de construire de manière cohérente n'importe quelle réponse HTTP en utilisant la méthode `status()` comme point de départ, puis en ajoutant des en-têtes et un corps selon les besoins. 
- Elle offre une grande flexibilité tout en maintenant une syntaxe claire et uniforme.

## Méthode générique de base

```java
ResponseEntity.status(HttpStatus status)
```

Cette méthode est le point de départ pour construire n'importe quelle réponse HTTP.

## Construction de réponses

### Réponse simple avec statut

```java
ResponseEntity.status(HttpStatus.OK).build()
ResponseEntity.status(HttpStatus.NOT_FOUND).build()
ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).build()
```

### Réponse avec corps

```java
ResponseEntity.status(HttpStatus.OK).body(bodyObject)
ResponseEntity.status(HttpStatus.CREATED).body(newResource)
ResponseEntity.status(HttpStatus.BAD_REQUEST).body(errorMessage)
```

### Réponse avec en-têtes personnalisés

```java
ResponseEntity.status(HttpStatus.OK)
    .header("Custom-Header", "value")
    .build()

ResponseEntity.status(HttpStatus.MOVED_PERMANENTLY)
    .header("Location", "http://newurl.com")
    .build()
```

### Réponse avec corps et en-têtes

```java
ResponseEntity.status(HttpStatus.OK)
    .header("Custom-Header", "value")
    .body(responseBody)
```

## Exemples spécifiques utilisant l'approche générique

### 200 OK

```java
ResponseEntity.status(HttpStatus.OK).body("Opération réussie")
```

### 201 Created

```java
ResponseEntity.status(HttpStatus.CREATED).body(createdUser)
```

### 204 No Content

```java
ResponseEntity.status(HttpStatus.NO_CONTENT).build()
```

### 400 Bad Request

```java
ResponseEntity.status(HttpStatus.BAD_REQUEST).body("Données invalides")
```

### 401 Unauthorized

```java
ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Non autorisé")
```

### 403 Forbidden

```java
ResponseEntity.status(HttpStatus.FORBIDDEN).body("Accès refusé")
```

### 404 Not Found

```java
ResponseEntity.status(HttpStatus.NOT_FOUND).body("Ressource non trouvée")
```

### 409 Conflict

```java
ResponseEntity.status(HttpStatus.CONFLICT).body("Conflit détecté")
```

### Redirection 301

```java
ResponseEntity.status(HttpStatus.MOVED_PERMANENTLY)
    .header("Location", "http://newurl.com")
    .build()
```

### Réponse avec en-tête personnalisé

```java
ResponseEntity.status(HttpStatus.OK)
    .header("X-Custom-Header", "CustomValue")
    .body("Réponse avec en-tête personnalisé")
```

