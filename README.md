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
