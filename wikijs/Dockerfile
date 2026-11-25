# Image de Wiki.js, d'après https://docs.requarks.io/install/linux

# Image de base : Node.js 24 (versions supportées par la doc : 22 et 24) sur Debian allégée
FROM node:24-bookworm-slim

# Version de Wiki.js, figée pour que le build soit reproductible
ARG WIKI_VERSION=2.5.317

# Outils nécessaires au téléchargement, puis nettoyage du cache apt
RUN apt-get update \
 && apt-get install -y --no-install-recommends wget ca-certificates \
 && rm -rf /var/lib/apt/lists/*

# Dossier de l'application (créé automatiquement)
WORKDIR /wiki

# Étapes 1 et 2 de la doc : télécharger l'archive et l'extraire.
# Tout est dans un seul RUN pour que l'archive ne reste pas dans une couche de l'image.
# Seul le dossier data doit être modifiable par l'utilisateur "node".
RUN wget -q -O /tmp/wiki-js.tar.gz \
      "https://github.com/Requarks/wiki-v2/releases/download/v${WIKI_VERSION}/wiki-js.tar.gz" \
 && tar xzf /tmp/wiki-js.tar.gz -C /wiki --no-same-owner \
 && rm /tmp/wiki-js.tar.gz \
 && mkdir -p /wiki/data \
 && chown -R node:node /wiki/data

# Étapes 3 et 4 de la doc : fichier de configuration
COPY config.yml /wiki/config.yml

ENV NODE_ENV=production

# Ne pas exécuter l'application en root
USER node

# Port d'écoute de Wiki.js (documentation : la publication se fait avec -p)
EXPOSE 3000

# Étape 6 de la doc : démarrer Wiki.js
CMD ["node", "server"]
