# Téléchargement de l'image alpine, on l'update et on l'a met à jour
FROM node:13.12.0-alpine
RUN apk update && apk upgrade

# On ajoute le repertoire d'application 
WORKDIR /app

# Ajout variable d'environnement
ENV PATH /app/node_modules/.bin:$PATH

# Ici on installe les dependantces
COPY package.json .
RUN yarn install

# Copie fichiers du projet
COPY . .
CMD [ "yarn", "run", "start" ]