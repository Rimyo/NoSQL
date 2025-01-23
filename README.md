# Introduction

Dans ce TP, nous allons explorer le fonctionnement de Redis, un système de gestion de bases de données NoSQL, en suivant les manipulations décrites dans trois vidéos. Ce document a pour but d'expliquer chaque manipulation en détail et de situer Redis parmi les quatre grandes familles de bases de données NoSQL étudiées en cours. À la fin, nous décrivons au moins quatre fonctionnalités supplémentaires de Redis non couvertes dans les vidéos.

## Liens vers les vidéos utilisées :
1. [Vidéo 1 : Introduction à Redis et commandes de base](https://www.youtube.com/watch?v=qeEY4giT3RM)
2. [Vidéo 2 : Ensembles ordonnés et Hashmaps](https://www.youtube.com/watch?v=cYjx5WSeA9U)
3. [Vidéo 3 : Fonctionnalités avancées de Redis (Pub/Sub)](https://www.youtube.com/watch?v=Y0bgXojSBNw&t=6s)

# Les familles de bases de données NoSQL

Les bases de données NoSQL se classent en quatre grandes familles, chacune adaptée à des cas d'utilisation spécifiques. Voici une description des différentes familles :

## 1. Bases de données Clé-Valeur

Les bases de données clé-valeur stockent des paires associant une clé unique à une valeur. Elles sont très performantes pour les recherches simples et les opérations en lecture/écriture. Exemple : Redis, Memcached.

### Caractéristiques principales :

- Simple et rapide.
- Pas de schéma rigide.
- Convient pour la mise en cache, les sessions utilisateur, et les files d'attente.

## 2. Bases de données Documentaires

Les bases de données documentaires stockent des données sous forme de documents, généralement au format JSON, BSON ou XML. Exemple : MongoDB, CouchDB.

### Caractéristiques principales :

- Idéales pour les données semi-structurées.
- Requêtes riches avec des filtres et des projections.
- Utilisées pour les applications web, les catalogues de produits, etc.

## 3. Bases de données en Colonnes

Les bases de données orientées colonnes organisent les données en colonnes plutôt qu'en lignes. Exemple : Apache Cassandra, HBase.

### Caractéristiques principales :

- Optimisées pour les analyses de données à grande échelle.
- Hautement évolutives.
- Utilisées dans le Big Data, les systèmes de recommandation, etc.

## 4. Bases de données en Graphe

Les bases de données orientées graphe stockent des relations entre des entités sous forme de graphes. Exemple : Neo4j, ArangoDB.

### Caractéristiques principales :

- Idéales pour les relations complexes.
- Performantes pour les calculs de chemin et les recherches de connexions.
- Utilisées pour les réseaux sociaux, la détection de fraudes, etc.

# Présentation de Redis

Redis ("REmote DIctionary Server") est une base de données NoSQL de type clé-valeur, souvent utilisée comme cache en mémoire, courtier de messages ou pour des traitements temps réel. Elle est classée dans la famille des bases de données clé-valeur, mais offre des structures de données avancées qui la rendent polyvalente.

## Points clés sur Redis :

- Entièrement en mémoire, avec une persistance optionnelle.
- Supporte des structures comme les listes, les ensembles, les ensembles ordonnés, les hashmaps, les bitmaps, et les hyperloglogs.
- Très performant grâce à son architecture simple.

# Installation de Redis avec Docker

Plutôt que d'installer Redis directement sur le système, nous allons utiliser Docker pour simplifier le processus.

## Prérequis

Assurez-vous que Docker est installé sur votre machine. Si ce n'est pas le cas, installez-le en suivant les instructions officielles : [Installation de Docker](https://docs.docker.com/get-docker/).

## Étape 1 : Récupérer l'image Redis

Téléchargez l'image officielle de Redis depuis Docker Hub :

```bash
docker pull redis
```

## Étape 2 : Lancer un conteneur Redis

Exécutez un conteneur Redis en mode détachement :

```bash
docker run -d --name redis-server -p 6379:6379 redis
```

Cela lance un serveur Redis accessible sur le port `6379` de votre machine.

## Étape 3 : Tester la connexion Redis

Connectez-vous au conteneur Redis avec un client Redis :

```bash
docker exec -it redis-server redis-cli
```

Essayez une commande simple :

```bash
SET key1 "Hello, Redis!"
GET key1
```

# Utilisation de Redis

Redis offre des commandes simples pour manipuler les données. Voici une présentation claire et détaillée des manipulations possibles.

## 1. Gestion des Clés

Les clés sont l'élément de base dans Redis. Voici comment les manipuler :

### Ajouter une clé

Pour créer une clé et lui attribuer une valeur :
```bash
SET key1 "Bonjour"
```
Redis renvoie `OK` pour confirmer que l'opération a réussi.

### Récupérer une clé

Pour lire la valeur associée à une clé existante :
```bash
GET key1
```
Retour : "Bonjour".

### Supprimer une clé

Pour supprimer une clé spécifique :
```bash
DEL key1
```
Retour : `1` si la clé a été supprimée, `0` si elle n'existe pas.


## 2. Compteur de Visiteurs

Un exemple pratique est de suivre le nombre de visiteurs sur un site web :

### Initialiser un compteur
```bash
SET visitors 0
```

### Incrémenter le compteur

À chaque visite :
```bash
INCR visitors
```
Chaque appel augmente la valeur de `visitors` de 1.

### Décrémenter le compteur

Si nécessaire, décrémentez le compteur :
```bash
DECR visitors
```

## 3. Listes

Les listes permettent de stocker des valeurs ordonnées :

### Ajouter à une liste
```bash
RPUSH courses "Base de Données Avancée"
RPUSH courses "Services Web"
```

### Afficher une liste
Pour afficher tous les éléments :
```bash
LRANGE courses 0 -1
```
Retour : `["Base de Données Avancée", "Services Web"]`

### Supprimer un élément d'une liste
```bash
LPOP courses
```
Supprime le premier élément.

## 4. Ensembles (Sets)

Les ensembles sont des collections non ordonnées d'éléments uniques.

### Ajouter à un ensemble
```bash
SADD users "Alice"
SADD users "Bob"
SADD users "Alice"  # Ignoré, car "Alice" existe déjà
```

### Afficher un ensemble
```bash
SMEMBERS users
```
Retour : `["Alice", "Bob"]`

### Supprimer un élément
```bash
SREM users "Bob"
```

## 5. Ensembles Ordonnés (Sorted Sets)

Les ensembles ordonnés permettent d'associer une valeur à un score numérique, ce qui permet de trier les éléments.

### Ajouter à un ensemble ordonné
```bash
ZADD scores 19 "Augustin"
ZADD scores 18 "Inès"
ZADD scores 20 "Philippe"
```

### Afficher les éléments triés par score
```bash
ZRANGE scores 0 -1
```
Retour : `["Inès", "Augustin", "Philippe"]` (ordre croissant).

### Afficher les éléments en ordre décroissant
```bash
ZREVRANGE scores 0 -1
```
Retour : `["Philippe", "Augustin", "Inès"]`.

### Connaître la position d'un élément
```bash
ZRANK scores "Augustin"
```
Retour : `1` (position d'Augustin en ordre croissant).

## 6. Hashmaps

Les hashmaps permettent de stocker des paires clé-valeur associées à une clé principale.

### Ajouter des données dans une hashmap
```bash
HSET user:1 username "Youssef" age 31 email "youssef@polytech.fr"
```

### Afficher toutes les données
```bash
HGETALL user:1
```
Retour : `{"username": "Youssef", "age": "31", "email": "youssef@polytech.fr"}`.

### Incrémenter une valeur
```bash
HINCRBY user:1 age 1
```
Retour : `32` (nouvel âge de Youssef).


## 7. Publication/Souscription (Pub/Sub)

Redis permet de gérer des canaux de communication où les clients peuvent publier ou s'abonner à des messages en temps réel. Cette fonctionnalité est utilisée dans des applications comme les notifications ou les chats.

### S'abonner à un canal
Pour qu'un client s'inscrive à un canal spécifique :
```bash
SUBSCRIBE mes_cours
```
Le client reste à l'écoute des messages publiés sur ce canal.

### Publier un message
Pour envoyer un message à tous les abonnés d'un canal :
```bash
PUBLISH mes_cours "Un nouveau cours sur Redis est disponible!"
```
Tous les clients inscrits au canal `mes_cours` recevront ce message instantanément.

### Utiliser des motifs (patterns) pour s'abonner
Vous pouvez vous abonner à plusieurs canaux avec un motif :
```bash
PSUBSCRIBE mes_*
```
Cela abonne le client à tous les canaux commençant par `mes_`.

### Publier sur un canal correspondant au motif
```bash
PUBLISH mes_notes "Une nouvelle note est arrivée."
```
Les clients abonnés à `mes_*` recevront ce message.

# Fonctionnalités supplémentaires de Redis

En plus des fonctionnalités décrites dans les vidéos, voici quatre autres caractéristiques notables de Redis :

1. Transactions

Redis permet d'exécuter un ensemble de commandes en tant que transaction atomique avec les commandes MULTI et EXEC.

2. Clustering

Redis propose une architecture de clustering pour distribuer les données sur plusieurs nœuds, offrant haute disponibilité et scalabilité.

3. Scripts Lua

Redis permet d'exécuter des scripts Lua pour automatiser des tâches complexes directement au niveau du serveur.

4. HyperLogLog

Redis utilise cette structure pour estimer rapidement et efficacement le cardinal de grands ensembles, en économisant de l'espace mémoire.



