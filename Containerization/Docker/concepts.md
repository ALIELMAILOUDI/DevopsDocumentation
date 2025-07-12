---

## 📄 Contenu pour `01-Containerization/Docker/concepts.md` 

Ce fichier approfondira la notion de Dockerfile et les bonnes pratiques.

```markdown
# Dockerfile et Images Docker : Concepts Avancés

Ce document explore le `Dockerfile`, le cœur de la création d'images Docker, et introduit le concept essentiel des builds multi-étapes pour des images optimisées.

## Qu'est-ce qu'un Dockerfile ?

Un **`Dockerfile`** est un fichier texte qui contient une série d'instructions que Docker Engine exécute séquentiellement pour construire une image Docker. C'est essentiellement une "recette" pour créer votre environnement d'application conteneurisé.

Chaque instruction dans un `Dockerfile` crée une nouvelle couche dans l'image. Ces couches sont superposables, légères et réutilisables, ce qui rend les images Docker efficaces et rapides à construire.

### Instructions Courantes d'un Dockerfile

* **`FROM` :** Définit l'image de base (ou image parente) à partir de laquelle votre image sera construite. C'est toujours la première instruction.
    * Exemple : `FROM node:20-alpine` (utilise une image Node.js légère basée sur Alpine Linux)
* **`WORKDIR` :** Définit le répertoire de travail pour toutes les instructions suivantes (comme `RUN`, `CMD`, `COPY`, `ADD`).
    * Exemple : `WORKDIR /app`
* **`COPY` :** Copie des fichiers ou des répertoires de votre système hôte (où la build Docker est lancée) vers l'image.
    * Exemple : `COPY . .` (copie tout le contenu du répertoire courant de l'hôte vers le `WORKDIR` de l'image)
* **`ADD` :** Similaire à `COPY`, mais peut également extraire automatiquement les archives (`.tar.gz`) et télécharger des fichiers à partir d'URLs. `COPY` est généralement préféré pour sa simplicité et sa prévisibilité.
* **`RUN` :** Exécute des commandes dans une nouvelle couche sur l'image actuelle. Utilisé pour installer des packages, créer des répertoires, etc.
    * Exemple : `RUN npm install` ou `RUN apt-get update && apt-get install -y git`
* **`EXPOSE` :** Informe Docker que le conteneur écoute sur le port spécifié au moment de l'exécution. C'est purement de la documentation et n'expose pas réellement le port sans l'option `-p` de `docker run`.
    * Exemple : `EXPOSE 80`
* **`CMD` :** Fournit la commande par défaut à exécuter lorsque le conteneur est démarré. Il ne peut y avoir qu'une seule instruction `CMD` par Dockerfile. Si vous fournissez une commande à `docker run`, elle remplacera la `CMD`.
    * Exemple : `CMD ["npm", "start"]`
* **`ENTRYPOINT` :** Configure le conteneur pour s'exécuter comme un exécutable. `CMD` fournit des arguments par défaut à `ENTRYPOINT`. Souvent utilisé pour des wrappers ou des scripts de démarrage.
    * Exemple : `ENTRYPOINT ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]`
* **`ENV` :** Définit des variables d'environnement dans l'image (et donc dans les conteneurs basés sur cette image).
    * Exemple : `ENV NODE_ENV production`
* **`ARG` :** Définit des variables de build qui peuvent être passées au moment de la construction de l'image (`docker build --build-arg`). Elles ne sont pas persistantes dans l'image finale.
    * Exemple : `ARG BUILD_VERSION`

## Dockerfile Multi-Étapes (Multi-Stage Builds)

Les builds multi-étapes sont une technique puissante pour créer des images Docker **plus petites et plus sécurisées**. L'idée est d'utiliser plusieurs instructions `FROM` dans un seul `Dockerfile`, chaque instruction commençant une nouvelle étape de build.

### Pourquoi utiliser des builds multi-étapes ?

* **Taille d'Image Réduite :** Vous n'incluez dans l'image finale que les artefacts nécessaires à l'exécution de l'application, en laissant de côté les outils de build, les dépendances de développement, les SDK, etc.
* **Sécurité Améliorée :** Moins de composants signifie moins de vulnérabilités potentielles dans l'image finale.
* **Cache de Build Efficace :** Les étapes de build sont mises en cache indépendamment, accélérant les rebuilds.

### Comment ça marche ?

1.  **Première Étape (Build Stage) :** Vous utilisez une image de base plus "lourde" (par exemple, `node:20` pour un build React, ou `openjdk:17-jdk` pour Spring Boot) pour compiler votre application. Vous donnez un nom à cette étape (ex: `AS builder`).
2.  **Deuxième Étape (Runtime Stage) :** Vous commencez une nouvelle étape avec une image de base beaucoup plus légère (par exemple, `nginx:alpine` pour React ou `openjdk:17-jre-slim` pour Spring Boot).
3.  **Copie des Artefacts :** Vous utilisez l'instruction `COPY --from=<nom_etape_precedente>` pour copier *uniquement* les artefacts de build nécessaires (comme les fichiers statiques compilés, les fichiers JAR/WAR) de l'étape précédente vers l'étape finale.

**Exemple Conceptuel :**

```dockerfile
# Étape 1 : Construire l'application
FROM node:20 AS build_stage
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Étape 2 : Servir l'application (image finale)
FROM nginx:alpine
COPY --from=build_stage /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
