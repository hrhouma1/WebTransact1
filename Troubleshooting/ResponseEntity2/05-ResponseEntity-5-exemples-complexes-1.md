#### Je vous présente un exemple d'une combinaison de méthodes enchaînées très longue pour créer une réponse ResponseEntity complexe. 
- Cette combinaison illustre l'utilisation de plusieurs méthodes pour construire une réponse détaillée avec de nombreux paramètres :

```java
ResponseEntity.status(HttpStatus.PARTIAL_CONTENT)
    .header("Content-Type", "application/json")
    .header("X-Custom-Header", "CustomValue")
    .header("Cache-Control", "no-cache, no-store, must-revalidate")
    .header("Pragma", "no-cache")
    .header("Expires", "0")
    .header("X-RateLimit-Limit", "100")
    .header("X-RateLimit-Remaining", "75")
    .header("X-RateLimit-Reset", "1623456789")
    .header("Content-Range", "bytes 21010-47021/47022")
    .allow(HttpMethod.GET, HttpMethod.POST, HttpMethod.PUT)
    .lastModified(System.currentTimeMillis())
    .eTag("\"0123456789\"")
    .contentType(MediaType.APPLICATION_JSON)
    .contentLength(1234)
    .location(URI.create("https://api.example.com/resources/123"))
    .cacheControl(CacheControl.maxAge(30, TimeUnit.MINUTES).mustRevalidate())
    .varyBy("User-Agent")
    .body(complexResponseObject);
```

Cette combinaison de méthodes enchaînées crée une réponse HTTP très détaillée avec les caractéristiques suivantes :

1. Statut HTTP 206 (Partial Content)
2. En-tête Content-Type défini sur application/json
3. En-tête personnalisé X-Custom-Header
4. En-têtes de contrôle de cache (Cache-Control, Pragma, Expires)
5. En-têtes de limitation de taux (X-RateLimit-*)
6. En-tête Content-Range pour le contenu partiel
7. Méthodes HTTP autorisées (GET, POST, PUT)
8. Timestamp de dernière modification
9. ETag pour la gestion du cache
10. Type de contenu JSON
11. Longueur du contenu
12. URI de localisation
13. Contrôle de cache avancé avec max-age et must-revalidate
14. En-tête Vary pour la négociation de contenu basée sur User-Agent
15. Corps de la réponse (complexResponseObject)

- Cette combinaison montre la flexibilité et la puissance de l'API ResponseEntity pour créer des réponses HTTP très détaillées et personnalisées. 
- Dans la pratique, il est rare d'avoir besoin d'autant de paramètres pour une seule réponse, mais cet exemple illustre les possibilités offertes par l'API.

# Autres exemples complexes :


- Je vous présente deux exemples très détaillés : un avec XML et un autre extrêmement long avec de nombreuses méthodes enchaînées.

### Exemple avec XML

```java
ResponseEntity.status(HttpStatus.OK)
    .header("Content-Type", "application/xml")
    .header("X-Custom-Header", "XMLResponse")
    .contentType(MediaType.APPLICATION_XML)
    .body("<?xml version=\"1.0\" encoding=\"UTF-8\"?>" +
          "<response>" +
          "  <status>success</status>" +
          "  <message>Opération réussie</message>" +
          "  <data>" +
          "    <user>" +
          "      <id>12345</id>" +
          "      <name>John Doe</name>" +
          "      <email>john.doe@example.com</email>" +
          "    </user>" +
          "    <permissions>" +
          "      <permission>READ</permission>" +
          "      <permission>WRITE</permission>" +
          "    </permissions>" +
          "  </data>" +
          "</response>");
```

### Exemple extrêmement long

```java
ResponseEntity.status(HttpStatus.PARTIAL_CONTENT)
    .header("Content-Type", "application/json")
    .header("X-Custom-Header", "LongChainedExample")
    .header("X-Request-ID", UUID.randomUUID().toString())
    .header("X-Powered-By", "Spring Boot")
    .header("X-Frame-Options", "DENY")
    .header("X-XSS-Protection", "1; mode=block")
    .header("X-Content-Type-Options", "nosniff")
    .header("Strict-Transport-Security", "max-age=31536000; includeSubDomains")
    .header("Referrer-Policy", "strict-origin-when-cross-origin")
    .header("Feature-Policy", "geolocation 'none'; microphone 'none'; camera 'none'")
    .header("Access-Control-Allow-Origin", "https://example.com")
    .header("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS")
    .header("Access-Control-Allow-Headers", "Content-Type, Authorization")
    .header("Access-Control-Max-Age", "3600")
    .header("Cache-Control", "no-cache, no-store, must-revalidate")
    .header("Pragma", "no-cache")
    .header("Expires", "0")
    .header("X-RateLimit-Limit", "1000")
    .header("X-RateLimit-Remaining", "995")
    .header("X-RateLimit-Reset", String.valueOf(System.currentTimeMillis() / 1000 + 3600))
    .header("Content-Range", "bytes 1000-2000/5000")
    .header("Accept-Ranges", "bytes")
    .header("Vary", "Accept-Encoding, User-Agent")
    .header("ETag", "\"" + UUID.randomUUID().toString() + "\"")
    .header("Last-Modified", new Date().toString())
    .header("X-API-Version", "v2.1")
    .header("X-Response-Time", "15.23ms")
    .header("X-Correlation-ID", UUID.randomUUID().toString())
    .header("X-Content-Security-Policy", "default-src 'self'")
    .header("X-DNS-Prefetch-Control", "off")
    .header("X-Download-Options", "noopen")
    .header("X-Permitted-Cross-Domain-Policies", "none")
    .header("Expect-CT", "max-age=86400, enforce")
    .allow(HttpMethod.GET, HttpMethod.POST, HttpMethod.PUT, HttpMethod.DELETE, HttpMethod.OPTIONS)
    .lastModified(System.currentTimeMillis())
    .eTag("\"" + UUID.randomUUID().toString() + "\"")
    .contentType(MediaType.APPLICATION_JSON)
    .contentLength(2345)
    .location(URI.create("https://api.example.com/resources/" + UUID.randomUUID().toString()))
    .cacheControl(CacheControl.maxAge(30, TimeUnit.MINUTES).mustRevalidate().proxyRevalidate().cachePublic())
    .varyBy("User-Agent", "Accept-Encoding")
    .body(Map.of(
        "status", "partial_content",
        "message", "Contenu partiel récupéré avec succès",
        "data", Map.of(
            "id", UUID.randomUUID().toString(),
            "name", "Exemple de données partielles",
            "description", "Ceci est un exemple de réponse très détaillée avec de nombreux en-têtes et options",
            "timestamp", new Date(),
            "metadata", Map.of(
                "version", "2.1",
                "generated_by", "Spring Boot Application",
                "environment", "production"
            )
        ),
        "pagination", Map.of(
            "current_page", 2,
            "total_pages", 10,
            "items_per_page", 50,
            "total_items", 500
        ),
        "links", Map.of(
            "self", "https://api.example.com/resources?page=2",
            "first", "https://api.example.com/resources?page=1",
            "prev", "https://api.example.com/resources?page=1",
            "next", "https://api.example.com/resources?page=3",
            "last", "https://api.example.com/resources?page=10"
        )
    ));
```

- Cet exemple extrêmement long illustre l'utilisation de nombreux en-têtes HTTP, des options de sécurité, des contrôles de cache, des métadonnées de réponse, et un corps de réponse JSON complexe. 
- Il montre la flexibilité de l'API ResponseEntity pour créer des réponses HTTP très détaillées et personnalisées.
- Dans la pratique, une réponse aussi complexe serait rare, mais cet exemple démontre les nombreuses possibilités offertes par l'API.
