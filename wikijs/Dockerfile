# Image de base : Node.js 20 sur Debian (slim = légère)
FROM node:20-bookworm-slim

# Installer wget (pour télécharger l'archive) puis nettoyer le cache apt
RUN apt-get update \
 && apt-get install -y --no-install-recommends wget ca-certificates \
 && rm -rf /var/lib/apt/lists/*

# Dossier de travail dans l'image
WORKDIR /wiki

# Télécharger la dernière version de Wiki.js et l'extraire dans /wiki
RUN wget -O /tmp/wiki-js.tar.gz https://github.com/Requarks/wiki/releases/latest/download/wiki-js.tar.gz \
 && tar xzf /tmp/wiki-js.tar.gz -C /wiki \
 && rm /tmp/wiki-js.tar.gz

# Copier notre configuration (voir étape 2)
COPY config.yml /wiki/config.yml

# Port sur lequel Wiki.js écoute
EXPOSE 3000

# Commande de démarrage (équivalent de "node server" de la doc)
CMD ["node", "server"]
