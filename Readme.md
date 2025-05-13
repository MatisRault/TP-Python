# API Neo4j avec Flask - Réseau Social

Une API RESTful développée avec Flask et Neo4j pour gérer un réseau social, implémentant des utilisateurs, des posts, des commentaires et leurs interactions.

## Table des matières

- [Présentation](#présentation)
- [Fonctionnalités](#fonctionnalités)
- [Prérequis](#prérequis)
- [Installation](#installation)
- [Configuration](#configuration)
- [Utilisation](#utilisation)
- [Structure du projet](#structure-du-projet)
- [API Endpoints](#api-endpoints)
- [Exemples](#exemples)
- [Base de données Neo4j](#base-de-données-neo4j)

## Présentation

Ce projet est une API REST qui utilise Neo4j, une base de données orientée graphe, pour modéliser un réseau social. L'API permet de gérer des utilisateurs, leurs publications (posts), commentaires, et relations d'amitié entre utilisateurs.

L'utilisation de Neo4j est particulièrement pertinente pour les réseaux sociaux car elle permet de modéliser efficacement les relations complexes entre les entités (comme les amitiés, les likes, etc.) et d'effectuer des requêtes performantes sur ces relations.

## Fonctionnalités

- **Gestion des utilisateurs** : création, lecture, mise à jour, suppression
- **Gestion des posts** : création, lecture, mise à jour, suppression
- **Gestion des commentaires** : création, lecture, mise à jour, suppression
- **Relations d'amitié** : ajouter/supprimer des amis, vérifier une amitié, trouver des amis en commun
- **Système de likes** : ajouter/supprimer des likes sur les posts et commentaires

## Prérequis

- Python 3.8+
- Docker (pour Neo4j)
- pip (gestionnaire de paquets Python)
- Postman ou curl (pour tester l'API)

## Installation

1. **Cloner le dépôt**

```bash
git clone https://github.com/MatisRault/TP-Python.git
cd neo4j-flask-api
```

2. **Créer et activer un environnement virtuel**

```bash
# Création de l'environnement virtuel
python -m venv venv

# Activation sur Windows
venv\Scripts\activate

# Activation sur macOS/Linux
source venv/bin/activate
```

3. **Installer les dépendances**

```bash
pip install -r requirements.txt
```

4. **Lancer Neo4j avec Docker**

```bash
docker-compose up -d
```

## Configuration

Voir le fichier `.env` à la racine du projet.

## Utilisation

1. **Démarrer l'API**

```bash
python run.py
```

L'API sera accessible à l'adresse : `http://localhost:5000`

2. **Initialiser la base de données avec des données de test (optionnel)**

```bash
python -m app.db_init
```

Cette commande va créer des utilisateurs, des posts, des commentaires et des relations entre eux pour faciliter les tests.

## Structure du projet

```
neo4j-flask-api/
├── app/
│   ├── __init__.py
│   ├── config.py
│   ├── database.py
│   ├── db_init.py
│   ├── models/
│   │   ├── __init__.py
│   │   ├── user.py
│   │   ├── post.py
│   │   └── comment.py
│   └── routes/
│       ├── __init__.py
│       ├── user_routes.py
│       ├── post_routes.py
│       └── comment_routes.py
├── docker-compose.yml
├── .env
├── requirements.txt
└── run.py
```

## API Endpoints

### Utilisateurs

| Méthode | Endpoint                              | Description                           |
|---------|---------------------------------------|---------------------------------------|
| GET     | `/users`                              | Récupérer tous les utilisateurs       |
| POST    | `/users`                              | Créer un utilisateur                  |
| GET     | `/users/:id`                          | Récupérer un utilisateur par ID       |
| PUT     | `/users/:id`                          | Mettre à jour un utilisateur          |
| DELETE  | `/users/:id`                          | Supprimer un utilisateur              |
| GET     | `/users/:id/friends`                  | Récupérer les amis d'un utilisateur   |
| POST    | `/users/:id/friends`                  | Ajouter un ami                        |
| DELETE  | `/users/:id/friends/:friendId`        | Supprimer un ami                      |
| GET     | `/users/:id/friends/:friendId`        | Vérifier si deux utilisateurs sont amis |
| GET     | `/users/:id/mutual-friends/:otherId`  | Récupérer les amis en commun          |

### Posts

| Méthode | Endpoint                   | Description                           |
|---------|----------------------------|---------------------------------------|
| GET     | `/posts`                   | Récupérer tous les posts              |
| GET     | `/posts/:id`               | Récupérer un post par ID              |
| PUT     | `/posts/:id`               | Mettre à jour un post                 |
| DELETE  | `/posts/:id`               | Supprimer un post                     |
| POST    | `/posts/:id/like`          | Ajouter un like à un post             |
| DELETE  | `/posts/:id/like`          | Retirer un like d'un post             |
| GET     | `/users/:id/posts`         | Récupérer les posts d'un utilisateur  |
| POST    | `/users/:id/posts`         | Créer un post pour un utilisateur     |

### Commentaires

| Méthode | Endpoint                                 | Description                           |
|---------|------------------------------------------|---------------------------------------|
| GET     | `/comments`                              | Récupérer tous les commentaires       |
| GET     | `/comments/:id`                          | Récupérer un commentaire par ID       |
| PUT     | `/comments/:id`                          | Mettre à jour un commentaire          |
| DELETE  | `/comments/:id`                          | Supprimer un commentaire              |
| POST    | `/comments/:id/like`                     | Ajouter un like à un commentaire      |
| DELETE  | `/comments/:id/like`                     | Retirer un like d'un commentaire      |
| GET     | `/posts/:id/comments`                    | Récupérer les commentaires d'un post  |
| POST    | `/posts/:id/comments`                    | Ajouter un commentaire à un post      |
| DELETE  | `/posts/:postId/comments/:commentId`     | Supprimer un commentaire d'un post    |

## Exemples

### Créer un utilisateur

**Requête** :

```bash
curl -X POST http://localhost:5000/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Jean Dupont", "email": "jean.dupont@example.com"}'
```

**Réponse** :

```json
{
  "status": "success",
  "message": "Utilisateur créé avec succès",
  "user": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Jean Dupont",
    "email": "jean.dupont@example.com",
    "created_at": 1621234567890
  }
}
```

### Créer un post

**Requête** :

```bash
curl -X POST http://localhost:5000/users/550e8400-e29b-41d4-a716-446655440000/posts \
  -H "Content-Type: application/json" \
  -d '{"title": "Mon premier post", "content": "Contenu de mon premier post"}'
```

### Ajouter un ami

**Requête** :

```bash
curl -X POST http://localhost:5000/users/550e8400-e29b-41d4-a716-446655440000/friends \
  -H "Content-Type: application/json" \
  -d '{"friend_id": "550e8400-e29b-41d4-a716-446655440001"}'
```

## Base de données Neo4j

### Modèle de données

- **Nœuds** :
  - `User` : Utilisateur du réseau social
  - `Post` : Publication créée par un utilisateur
  - `Comment` : Commentaire sur un post

- **Relations** :
  - `CREATED` : Relation entre un utilisateur et ce qu'il a créé (post ou commentaire)
  - `HAS_COMMENT` : Relation entre un post et ses commentaires
  - `FRIENDS_WITH` : Relation d'amitié entre deux utilisateurs
  - `LIKES` : Relation entre un utilisateur et ce qu'il a aimé (post ou commentaire)

### Interface Neo4j Browser

Vous pouvez accéder à l'interface web de Neo4j pour visualiser et interagir avec la base de données :

- URL : `http://localhost:7474`
- Identifiant : `neo4j`
- Mot de passe : `password` (ou celui configuré dans le .env)

### Exemples de requêtes Cypher

#### Trouver les posts d'un utilisateur

```cypher
MATCH (u:User {id: 'user_id_here'})-[:CREATED]->(p:Post)
RETURN p
ORDER BY p.created_at DESC
```

#### Trouver les amis en commun entre deux utilisateurs

```cypher
MATCH (u1:User {id: 'user_id_1'})-[:FRIENDS_WITH]-(mutual:User)-[:FRIENDS_WITH]-(u2:User {id: 'user_id_2'})
RETURN mutual
```

#### Trouver les utilisateurs qui ont aimé un post

```cypher
MATCH (u:User)-[:LIKES]->(p:Post {id: 'post_id_here'})
RETURN u
```

---

