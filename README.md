# E-Commerce Vue.js avec Microservices

## Structure simplifiée du projet

```
e-commerce-vue/
├── docker-compose.yml
├── frontend/
│   ├── Dockerfile
├── package.json
├── README.md
├── scripts/
│   ├── init-products.sh
└── services/
    ├── auth-service/
    │   ├── Dockerfile
    ├── order-service/
    │   ├── Dockerfile
    ├── product-service/
    │   ├── Dockerfile
```

## **Composants Principaux**

### **Frontend**
- Application utilisateur développée avec Vue.js.
- Interagit avec les microservices backend via des APIs REST.

### **Backend** (Microservices Node.js + MongoDB)

- **Auth Service** : Gestion de l'authentification et des utilisateurs.
- **Product Service** : Gestion des produits et du panier.
- **Order Service** : Gestion des commandes.
- **MongoDB** : Base de données utilisée par tous les services backend.

## **Déploiement avec Docker et Docker Compose**

Le projet est conteneurisé avec Docker et orchestré avec Docker Compose.

### **Docker Compose - Configuration**

1. **Création du réseau `backend`** : Permet aux conteneurs de communiquer sans exposer MongoDB à l'extérieur.  

2. **Démarrage de la base de données `mongodb`** :  
   - Utilise l’image officielle MongoDB (`mongo:4.4`).  
   - Monte un volume `mongo-data2` pour conserver les données.  
   - Initialise la base de données `ecommerce`.  

3. **Lancement des microservices (`auth-service`, `order-service`, `product-service`)** :  
   - Chacun construit son conteneur à partir du `Dockerfile` correspondant.  
   - Attente de MongoDB grâce à `depends_on`.  
   - Connexion à la base via `MONGODB_URI`.  
   - Utilisation de `JWT_SECRET` pour l’authentification.  

4. **Démarrage du `frontend` (Vue.js)** :  
   - Construit l’interface utilisateur avec Vue.js.  
   - Expose le port `8080`.  
   - Configure les URLs des microservices via des variables d’environnement.  

5. **Les services fonctionnent ensemble** :  
   - Le frontend appelle les API (`auth-service`, `product-service`, `order-service`).  
   - Les microservices interagissent avec MongoDB pour gérer les utilisateurs, les produits et les commandes.

6. **Exécution du script d'initialisation des produits** :  
   - Après le lancement des services, le script `scripts/init-products.sh` est exécuté pour insérer des produits de test dans la base de données.

## **Lancement du Projet**

1. **Cloner le dépôt** :
   ```bash
   git clone https://github.com/Lydia-BEDRI/e-commerce-docker
   cd e-commerce-vue
   ```
2. **Lancer les services avec Docker Compose** :
   ```bash
   docker-compose up --build
   ```
3. **Initialiser les produits dans la base de données** :
   ```bash
   bash scripts/init-products.sh
   ```
4. **Accéder à l'application** :
   - Frontend : http://localhost:8080
   - Auth Service : http://localhost:3001
   - Product Service : http://localhost:3000
   - Order Service : http://localhost:3002

