# Résumé 1

- Je vous présente un résumé des principales méthodes de ResponseEntity organisées par type de réponse, avec leurs équivalents utilisant le constructeur :

## Réponses 2xx (Succès)

### 200 OK

```java
// Méthode statique
ResponseEntity.ok()
ResponseEntity.ok(body)

// Constructeur équivalent  
new ResponseEntity<>(body, HttpStatus.OK)
```

### 201 Created

```java
// Méthode statique
ResponseEntity.created(location)

// Constructeur équivalent
new ResponseEntity<>(body, headers, HttpStatus.CREATED)
```

### 202 Accepted  

```java
// Méthode statique
ResponseEntity.accepted()

// Constructeur équivalent
new ResponseEntity<>(HttpStatus.ACCEPTED)
```

### 204 No Content

```java
// Méthode statique
ResponseEntity.noContent().build()

// Constructeur équivalent
new ResponseEntity<>(HttpStatus.NO_CONTENT)
```

## Réponses 4xx (Erreurs client)

### 400 Bad Request

```java
// Méthode statique
ResponseEntity.badRequest()

// Constructeur équivalent
new ResponseEntity<>(HttpStatus.BAD_REQUEST)
```

### 401 Unauthorized

```java
// Méthode statique
ResponseEntity.status(HttpStatus.UNAUTHORIZED)

// Constructeur équivalent
new ResponseEntity<>(HttpStatus.UNAUTHORIZED)
```

### 403 Forbidden

```java
// Méthode statique
ResponseEntity.status(HttpStatus.FORBIDDEN)

// Constructeur équivalent
new ResponseEntity<>(HttpStatus.FORBIDDEN)
```

### 404 Not Found

```java
// Méthode statique
ResponseEntity.notFound().build()

// Constructeur équivalent
new ResponseEntity<>(HttpStatus.NOT_FOUND)
```

## Réponses 5xx (Erreurs serveur)

### 500 Internal Server Error

```java
// Méthode statique
ResponseEntity.internalServerError()

// Constructeur équivalent
new ResponseEntity<>(HttpStatus.INTERNAL_SERVER_ERROR)
```

## Méthodes génériques

```java
// Statut personnalisé
ResponseEntity.status(HttpStatus.CUSTOM_STATUS)

// Ajout d'en-têtes
ResponseEntity.ok().header("Custom-Header", "value")

// Constructeur équivalent
HttpHeaders headers = new HttpHeaders();
headers.add("Custom-Header", "value");
new ResponseEntity<>(body, headers, HttpStatus.OK)
```

Ces méthodes permettent de construire facilement des réponses HTTP avec différents statuts, corps et en-têtes selon les besoins.

# Citations:

[1] https://codingnomads.com/spring-responseentity

[2] https://technicalsand.com/using-responseentity-in-spring/

[3] https://sudarshandoiphode.hashnode.dev/responseentity

[4] https://www.danvega.dev/blog/spring-response-entity

[5] https://docs.spring.io/spring-framework/docs/4.1.0.RC1_to_4.1.0.RC2/Spring%20Framework%204.1.0.RC2/org/springframework/http/ResponseEntity.html

[6] https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/ResponseEntity.html
