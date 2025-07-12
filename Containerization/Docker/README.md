# Docker : La Conteneurisation Simplifiée

Ce dossier explore Docker, l'outil leader pour la conteneurisation d'applications. La conteneurisation est une technologie fondamentale en DevOps, permettant d'empaqueter les applications et leurs dépendances dans des unités isolées appelées conteneurs.

## Qu'est-ce que la Conteneurisation ?

La conteneurisation est une méthode d'**empaquetage** d'une application logicielle et de toutes ses dépendances (bibliothèques, frameworks, configurations, etc.) dans une **unité standardisée** appelée **conteneur**. Ce conteneur peut ensuite être exécuté de manière cohérente sur n'importe quel environnement (ordinateur portable, machine virtuelle, cloud) disposant d'un moteur de conteneur.

### Conteneurs vs Machines Virtuelles (VMs)

| Caractéristique       | Conteneurs                               | Machines Virtuelles (VMs)                          |
| :-------------------- | :--------------------------------------- | :------------------------------------------------- |
| **Isolation** | Au niveau du système d'exploitation      | Au niveau du matériel (via hyperviseur)            |
| **OS Invité** | Partage l'OS hôte                        | Chaque VM a son propre OS invité complet           |
| **Taille** | Très légers (Mo)                         | Lourds (Go)                                        |
| **Démarrage** | Rapide (secondes)                        | Lent (minutes)                                     |
| **Ressources** | Moins de surcharge, plus efficaces       | Plus de surcharge, moins efficaces                 |
| **Portabilité** | Très élevée, "Build once, run anywhere"  | Élevée, mais dépend de l'hyperviseur               |

## Docker : L'Outil de Conteneurisation Dominant

**Docker** est une plateforme open-source qui permet aux développeurs de créer, déployer et exécuter des applications dans des conteneurs. Il utilise les fonctionnalités d'isolation du noyau Linux (comme les cgroups et les namespaces) pour créer des environnements isolés et reproductibles.

### Concepts Clés de Docker

* **Image Docker :** Un modèle léger, autonome et exécutable qui contient tout le nécessaire pour exécuter une application : le code, un environnement d'exécution, des bibliothèques, des variables d'environnement et des fichiers de configuration. Les images sont construites à partir de **`Dockerfile`s**.
* **Conteneur Docker :** Une instance exécutable d'une image. Un conteneur est une version en cours d'exécution d'une image, isolée des autres conteneurs et de l'hôte. Vous pouvez avoir plusieurs conteneurs basés sur la même image.
* **Dockerfile :** Un fichier texte contenant des instructions pour construire une image Docker. C'est votre "recette" pour créer des images.
* **Docker Engine :** Le logiciel client-serveur qui construit et exécute des conteneurs.
* **Docker Hub / Registre :** Un service cloud (ou privé) qui permet de stocker et de partager des images Docker (voir la section "Registres d'Images").

## Commandes Docker Essentielles

Voici les commandes de base pour interagir avec Docker.

### 1. **Gestion du Docker Engine**

* **`sudo systemctl start docker`**
    * Démarre le service Docker (sur les systèmes Linux basés sur systemd).
* **`sudo systemctl status docker`**
    * Vérifie l'état du service Docker.

### 2. **Construction d'Images**

* **`docker build -t <nom_image>:<tag> .`**
    * Construit une image Docker à partir d'un `Dockerfile` dans le répertoire courant (`.`).
    * `-t` : Balise (tag) l'image avec un nom et une version.
    * Exemple : `docker build -t mon-app-react:1.0.0 .`

### 3. **Gestion des Images**

* **`docker images`**
    * Liste toutes les images Docker locales.
* **`docker pull <nom_image>:<tag>`**
    * Télécharge une image depuis un registre (par défaut Docker Hub).
    * Exemple : `docker pull nginx:latest`
* **`docker rmi <nom_image>:<tag>`**
    * Supprime une image locale.
    * Exemple : `docker rmi mon-app-react:1.0.0`
* **`docker rmi $(docker images -aq)`**
    * Supprime toutes les images locales (utilisez avec prudence !).

### 4. **Exécution des Conteneurs**

* **`docker run <nom_image>:<tag>`**
    * Crée et démarre un nouveau conteneur à partir d'une image.
    * Exemple : `docker run nginx:latest`
* **`docker run -d <nom_image>:<tag>`**
    * Démarre le conteneur en mode détaché (en arrière-plan).
* **`docker run -p <port_hôte>:<port_conteneur> <nom_image>:<tag>`**
    * Mappe un port du conteneur à un port de l'hôte, permettant l'accès externe.
    * Exemple : `docker run -p 8080:80 mon-app-react:1.0.0` (mappe le port 80 du conteneur au port 8080 de l'hôte)
* **`docker run --name <nom_conteneur> <nom_image>:<tag>`**
    * Assigner un nom spécifique au conteneur pour faciliter sa gestion.
* **`docker run -it <nom_image>:<tag> /bin/bash`**
    * Démarre le conteneur en mode interactif avec un terminal (`-i` pour interactif, `-t` pour pseudo-tty). Utile pour déboguer.

### 5. **Gestion des Conteneurs**

* **`docker ps`**
    * Liste les conteneurs en cours d'exécution.
* **`docker ps -a`**
    * Liste tous les conteneurs (en cours d'exécution et arrêtés).
* **`docker start <ID_conteneur_ou_nom>`**
    * Démarre un conteneur arrêté.
* **`docker stop <ID_conteneur_ou_nom>`**
    * Arrête un conteneur en cours d'exécution.
* **`docker restart <ID_conteneur_ou_nom>`**
    * Redémarre un conteneur.
* **`docker rm <ID_conteneur_ou_nom>`**
    * Supprime un conteneur (doit être arrêté au préalable).
* **`docker rm $(docker ps -aq)`**
    * Supprime tous les conteneurs (utilisez avec prudence !).
* **`docker logs <ID_conteneur_ou_nom>`**
    * Affiche les logs d'un conteneur.
* **`docker exec -it <ID_conteneur_ou_nom> /bin/bash`**
    * Exécute une commande dans un conteneur en cours d'exécution (utile pour obtenir un shell à l'intérieur).

### 6. **Nettoyage des Ressources Docker**

* **`docker system prune`**
    * Supprime tous les conteneurs arrêtés, les réseaux non utilisés, les images non utilisées (pendulaires) et le cache de construction. **Très utile pour libérer de l'espace disque.**
* **`docker system prune -a`**
    * Supprime encore plus de ressources, y compris les images sans nom (dangling images) et les images non utilisées par un conteneur.

## Registres d'Images Docker

Un **registre d'images Docker** est un système de stockage et de distribution pour les images Docker. C'est là que vous poussez (`docker push`) et tirez (`docker pull`) vos images.

### 1. **Docker Hub**

* **Description :** Le registre public et par défaut pour les images Docker. Il contient des millions d'images officielles et communautaires.
* **Utilisation :** Vous pouvez y pousser vos propres images publiques ou privées après vous être connecté (`docker login`).
* **URL pour les pulls :** Généralement implicite ou `docker.io/<utilisateur>/<image>`.

### 2. **GitLab Container Registry**

* **Description :** Intégré directement à GitLab, chaque projet GitLab dispose de son propre registre de conteneurs. Idéal pour les projets qui utilisent GitLab CI/CD, car l'intégration est transparente.
* **Authentification :** Utilise les identifiants GitLab ou un token d'accès de déploiement/CI_JOB_TOKEN.
* **URL pour les pulls :** `registry.gitlab.com/<groupe>/<projet>/<image>`.

### 3. **Autres Registres Cloud & Privés**

* **Amazon Elastic Container Registry (ECR)**
* **Google Container Registry (GCR) / Artifact Registry**
* **Azure Container Registry (ACR)**
* **Harbor** (registre open-source auto-hébergé)

**Commandes de Push/Pull avec Registres Spécifiques :**

Pour pousser une image vers un registre spécifique, vous devez d'abord la **tagger** avec le nom complet du registre :

```bash
docker tag <image_locale>:<tag_locale> <url_registre>/<utilisateur>/<image_nom>:<tag_registre>
docker push <url_registre>/<utilisateur>/<image_nom>:<tag_registre>
