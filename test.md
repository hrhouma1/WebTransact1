
```http
### Scénario 1: Gestion des clients (Customer)

### 1.1 Create: Ajouter un premier client
POST http://localhost:8080/api/v1/customers/add
Content-Type: application/json

{
  "customerFirstName": "John",
  "customerLastName": "Doe"
}

### Réponse attendue:
# {
#   "customerId": 1,
#   "customerFirstName": "John",
#   "customerLastName": "Doe"
# }
# (Nombre de clients: 1, Cartes par client: {1: 0})

### 1.2 Create: Ajouter un deuxième client
POST http://localhost:8080/api/v1/customers/add
Content-Type: application/json

{
  "customerFirstName": "Jane",
  "customerLastName": "Smith"
}

### Réponse attendue:
# {
#   "customerId": 2,
#   "customerFirstName": "Jane",
#   "customerLastName": "Smith"
# }
# (Nombre de clients: 2, Cartes par client: {1: 0, 2: 0})

### 1.3 Read (All): Afficher tous les clients
GET http://localhost:8080/api/v1/customers

### Réponse attendue:
# [
#   {
#     "customerId": 1,
#     "customerFirstName": "John",
#     "customerLastName": "Doe"
#   },
#   {
#     "customerId": 2,
#     "customerFirstName": "Jane",
#     "customerLastName": "Smith"
#   }
# ]
# (Nombre de clients: 2, Cartes par client: {1: 0, 2: 0})

### Scénario 2: Gestion des cartes (Card)

### 2.1 Create: Ajouter une carte au client 1
POST http://localhost:8080/api/v1/cards/add
Content-Type: application/json

{
  "customerId": 1,
  "cardNumber": "1234-5678-9012-3456",
  "cardType": "Visa",
  "totalLimit": 5000,
  "amountUsed": 1000,
  "availableAmount": 4000,
  "createDt": "2024-09-30"
}

### Réponse attendue:
# {
#   "cardId": 1,
#   "customerId": 1,
#   "cardNumber": "1234-5678-9012-3456",
#   "cardType": "Visa",
#   "totalLimit": 5000,
#   "amountUsed": 1000,
#   "availableAmount": 4000,
#   "createDt": "2024-09-30"
# }
# (Nombre de clients: 2, Cartes par client: {1: 1, 2: 0})

### 2.2 Create: Ajouter une carte au client 2
POST http://localhost:8080/api/v1/cards/add
Content-Type: application/json

{
  "customerId": 2,
  "cardNumber": "9876-5432-1098-7654",
  "cardType": "MasterCard",
  "totalLimit": 10000,
  "amountUsed": 2000,
  "availableAmount": 8000,
  "createDt": "2024-09-30"
}

### Réponse attendue:
# {
#   "cardId": 2,
#   "customerId": 2,
#   "cardNumber": "9876-5432-1098-7654",
#   "cardType": "MasterCard",
#   "totalLimit": 10000,
#   "amountUsed": 2000,
#   "availableAmount": 8000,
#   "createDt": "2024-09-30"
# }
# (Nombre de clients: 2, Cartes par client: {1: 1, 2: 1})

### Scénario 3: Gestion des suppressions et des contraintes

### 3.1 Delete (Single Customer): Tenter de supprimer le client 1 qui a une carte (devrait échouer)
DELETE http://localhost:8080/api/v1/customers/delete/1

### Réponse attendue:
# {
#   "error": "Cannot delete customer with associated cards"
# }
# (Nombre de clients: 2, Cartes par client: {1: 1, 2: 1})

### 3.2 Delete (Single Card by ID): Supprimer la carte du client 1
DELETE http://localhost:8080/api/v1/cards/delete/1

### Réponse attendue:
# {
#   "message": "Card with ID 1 successfully deleted"
# }
# (Nombre de clients: 2, Cartes par client: {1: 0, 2: 1})

### 3.3 Delete (Single Customer): Supprimer le client 1 (devrait réussir maintenant)
DELETE http://localhost:8080/api/v1/customers/delete/1

### Réponse attendue:
# {
#   "message": "Customer with ID 1 successfully deleted"
# }
# (Nombre de clients: 1, Cartes par client: {2: 1})

### Scénario 4: Préparation pour la suppression finale

### 4.1 Create: Ajouter un nouveau client
POST http://localhost:8080/api/v1/customers/add
Content-Type: application/json

{
  "customerFirstName": "Alice",
  "customerLastName": "Johnson"
}

### Réponse attendue:
# {
#   "customerId": 3,
#   "customerFirstName": "Alice",
#   "customerLastName": "Johnson"
# }
# (Nombre de clients: 2, Cartes par client: {2: 1, 3: 0})

### 4.2 Create: Ajouter une carte au nouveau client
POST http://localhost:8080/api/v1/cards/add
Content-Type: application/json

{
  "customerId": 3,
  "cardNumber": "1111-2222-3333-4444",
  "cardType": "Amex",
  "totalLimit": 15000,
  "amountUsed": 5000,
  "availableAmount": 10000,
  "createDt": "2024-09-30"
}

### Réponse attendue:
# {
#   "cardId": 3,
#   "customerId": 3,
#   "cardNumber": "1111-2222-3333-4444",
#   "cardType": "Amex",
#   "totalLimit": 15000,
#   "amountUsed": 5000,
#   "availableAmount": 10000,
#   "createDt": "2024-09-30"
# }
# (Nombre de clients: 2, Cartes par client: {2: 1, 3: 1})

### Scénario 5: Nettoyage final et vérifications

### 5.1 Delete (All Cards): Supprimer toutes les cartes
DELETE http://localhost:8080/api/v1/cards

### Réponse attendue:
# {
#   "message": "All cards successfully deleted"
# }
# (Nombre de clients: 2, Cartes par client: {2: 0, 3: 0})

### 5.2 Delete (All Customers): Supprimer tous les clients
DELETE http://localhost:8080/api/v1/customers

### Réponse attendue:
# {
#   "message": "All customers successfully deleted"
# }
# (Nombre de clients: 0, Cartes par client: {})

### 5.3 Read (All Customers): Vérifier que tous les clients sont supprimés
GET http://localhost:8080/api/v1/customers

### Réponse attendue:
# []
# (Nombre de clients: 0, Cartes par client: {})

### 5.4 Read (All Cards): Vérifier que toutes les cartes sont supprimées
GET http://localhost:8080/api/v1/cards

### Réponse attendue:
# []
# (Nombre de clients: 0, Cartes par client: {})
```


