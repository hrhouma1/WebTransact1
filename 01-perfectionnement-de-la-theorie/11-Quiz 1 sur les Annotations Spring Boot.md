# 📚 Quiz 1 sur les Annotations Spring Boot

1. **Question:** Quelle annotation est utilisée pour marquer une classe comme un composant de Spring, permettant à Spring de la détecter automatiquement ?
   - A) `@Autowired`
   - B) `@Component`
   - C) `@Service`
   - D) `@Repository`
   - **Réponse:** B) `@Component`

2. **Question:** Quelle est la différence entre `@Component` et `@Service` ?
   - A) `@Service` est utilisée spécifiquement pour les services métiers, tandis que `@Component` est une annotation générique.
   - B) `@Component` est utilisée pour les services, et `@Service` est utilisée pour les composants génériques.
   - C) `@Component` et `@Service` sont identiques en tout point.
   - D) `@Service` est plus performante que `@Component`.
   - **Réponse:** A) `@Service` est utilisée spécifiquement pour les services métiers, tandis que `@Component` est une annotation générique.

3. **Question:** Quelle annotation devez-vous utiliser pour injecter une dépendance dans un constructeur ou un setter ?
   - A) `@Controller`
   - B) `@Autowired`
   - C) `@Configuration`
   - D) `@Bean`
   - **Réponse:** B) `@Autowired`

4. **Question:** Quelle annotation est nécessaire pour mapper une méthode HTTP GET à un contrôleur Spring ?
   - A) `@PostMapping`
   - B) `@PutMapping`
   - C) `@GetMapping`
   - D) `@DeleteMapping`
   - **Réponse:** C) `@GetMapping`

5. **Question:** Quelle annotation est utilisée pour marquer une classe comme un contrôleur REST dans Spring Boot ?
   - A) `@Controller`
   - B) `@RestController`
   - C) `@Service`
   - D) `@RequestMapping`
   - **Réponse:** B) `@RestController`

6. **Question:** Dans un projet Spring, quelle annotation utiliseriez-vous pour activer la configuration de votre application en utilisant des classes Java plutôt que des fichiers XML ?
   - A) `@EnableConfiguration`
   - B) `@Component`
   - C) `@Configuration`
   - D) `@Context`
   - **Réponse:** C) `@Configuration`

7. **Question:** Comment marquer une classe dans Spring pour indiquer qu'elle correspond à une entité dans une base de données ?
   - A) `@Table`
   - B) `@Entity`
   - C) `@Repository`
   - D) `@Column`
   - **Réponse:** B) `@Entity`

8. **Question:** Quelle annotation utiliseriez-vous pour définir une méthode comme une méthode transactionnelle ?
   - A) `@Transactional`
   - B) `@Service`
   - C) `@Component`
   - D) `@Repository`
   - **Réponse:** A) `@Transactional`

9. **Question:** Quelle annotation permet de gérer les requêtes HTTP entrantes dans un contrôleur Spring en leur assignant un chemin spécifique ?
   - A) `@PathVariable`
   - B) `@RequestParam`
   - C) `@RequestMapping`
   - D) `@GetMapping`
   - **Réponse:** C) `@RequestMapping`

10. **Question:** Dans un repository Spring Data JPA, pourquoi n'est-il pas nécessaire d'implémenter des méthodes comme `save` ou `delete` ?
    - A) Parce qu'elles sont automatiquement fournies par l'interface `JpaRepository`.
    - B) Parce que ces méthodes ne sont pas utilisées dans les projets Spring.
    - C) Parce qu'elles sont obsolètes.
    - D) Parce qu'elles doivent être créées manuellement dans chaque repository.
    - **Réponse:** A) Parce qu'elles sont automatiquement fournies par l'interface `JpaRepository`.

