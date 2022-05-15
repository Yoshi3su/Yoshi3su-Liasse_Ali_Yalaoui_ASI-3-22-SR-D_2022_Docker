# Yoshi3su-Liasse_Ali_Yalaoui_ASI-3-22-SR-D_2022_Docker
Projet Docker avec une architecture backend / frontend

L’objectifs de ce projet est d’intégrer le déploiement d’une API Rest avec Docker afin de pouvoir
l’intégrer dans un système d’intégration continue.

1. Récupérer le projet API REST qui comporte une partie backend (API rest) et une partie
frontend (reactJS) : deux dossiers backend et frontend

2. Créer un/des fichier(s) DockerFile(s) pour builder notre image pour les serveurs de
production

3. Créer un fichier docker-compose.yml pour démarrer nos containers, nos services
4. Modifier la documentation qui va expliquer le contenu de ces deux fichiers et de
l’architecture dans un fichier README.md :
  
  a. DockerFile(s) pour builder notre image
  b. Docker-compose pour démarrer le(s) container(s) (Bien décrire l’ensemble des
  différentes étapes d’exécution des commandes associés aux services)
  c. Architecture de l’application
  
  # Dockerfiles
  
Pour conteneuriser nos applications front et backend, nous utiliser des dockerfile, fichiers dont l'objectif est de décrire les étapes de conteneurisations de l'application. On part sur un OS distribution alpine, légère et rapide. On ajoute le repertoire d'application et on installe les dependences. On fini par exectuer les commande yarn run start. On est sur le même principe pour les 2 dockerfiles.

Dockerfile front
```Dockerfile front
FROM node:13.12.0-alpine
RUN apk update && apk upgrade

WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH

COPY package.json .
RUN yarn install

COPY . .
CMD [ "yarn", "run", "start" ]
```
Dockerfile back
```Dockerfile back
FROM node:13.12.0-alpine
RUN apk update && apk upgrade

WORKDIR /app

ENV PATH /app/node_modules/.bin:$PATH

COPY package.json .
RUN yarn install

COPY . .
CMD [ "node", "server.js" ]
```

# Docker compose 

Le fichier docker-compose.yml est un fichier au format YML, donc l'objectif est de décrire le fonctionnement de nos conteneur et l'injecter dans l'architecture.

```dockerfile
version: '3'

services:

  # React
  frontend:
    ports:
        - 3000:3000 # Le port par défaut de reactjs est 3000 pour le frontend.
    build: ./frontend
    container_name: react-app

  # Data base mongo
  mongodb:
    image: mongo:latest
    container_name: mongo
    volumes: 
      - 'mongo-data:/data/db'
    networks:
      - backend

  # NodeJS pour interagir avec MongoDB
  backend:
    build: ./backend
    container_name: nodejs
    ports:
     - 8080:8080 # Le port par défaut de l’api est indiqué dans le code source du fichier server.js (line 69)
    environment:
      - MONGO_URI=mongodb://mongo:27017/dbdev
    depends_on:
      - mongodb
    networks:
      - backend

# Volume pour garder les données
  volumes:
    mongo-data:

  networks:
    backend:
        driver: bridge
```

On installe ici 3 services : 
  1. L'application react sur le port 3000 de notre hôte vers le port 3000 du conteneur. 
  2. L'api NodeJS, qui nous servira de point de communication 
  3. La bdd MondoDB, où nous créerons notre volume et permettra de conserver les données.
 
 
 # How to start it 
Pour start l'application, on se place dans le repertoire du projet à l'emplacement du docker compose et on utilise la commmande

```
docker-compose up
```
