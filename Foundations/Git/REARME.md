# Essentiels de Git : Votre Guide de Contrôle de Version

Cette section couvre les aspects fondamentaux de Git, le système de contrôle de version distribué essentiel pour tout professionnel DevOps. Comprendre Git est crucial pour collaborer sur du code, suivre les modifications et maintenir un historique clair de vos projets.

## Qu'est-ce que le Contrôle de Version ?

Le **Contrôle de Version** est un système qui enregistre les modifications apportées à un fichier ou un ensemble de fichiers au fil du temps afin que vous puissiez rappeler des versions spécifiques plus tard. Il aide à :
* **Suivre les Modifications :** Savoir qui a fait quelles modifications, quand et pourquoi.
* **Collaboration :** Plusieurs personnes peuvent travailler sur le même projet simultanément sans écraser le travail des autres.
* **Revenir en Arrière :** Revenir facilement aux versions précédentes de votre code.
* **Branching & Merging :** Travailler sur de nouvelles fonctionnalités ou des correctifs de manière isolée sans affecter la base de code principale.

## Qu'est-ce que Git ?

**Git** est un **système de contrôle de version distribué (DVCS)**. Contrairement aux systèmes centralisés, chaque développeur possède une copie complète de l'historique entier du dépôt, permettant le travail hors ligne et une meilleure résilience.

## Workflow Git Basique

Le workflow typique implique :
1.  **Clonage :** Obtenir une copie du dépôt.
2.  **Modification :** Modifier des fichiers.
3.  **Staging :** Sélectionner les modifications à inclure dans le prochain commit.
4.  **Committing :** Enregistrer les modifications stagées dans votre historique local.
5.  **Pushing :** Télécharger vos commits locaux vers le dépôt distant.
6.  **Pulling/Fetching :** Télécharger les modifications depuis le dépôt distant.

## Commandes Git Essentielles

Voici les commandes que vous utiliserez le plus fréquemment :

### 1. **Configuration Initiale**

* **`git config --global user.name "Votre Nom"`**
    * Définit votre nom d'utilisateur global pour les commits Git.
* **`git config --global user.email "votre.email@example.com"`**
    * Définit votre adresse e-mail globale pour les commits Git.
* **`git config --global init.defaultBranch main`**
    * Définit le nom de branche par défaut pour les nouveaux dépôts à `main` (pratique moderne).

### 2. **Initialisation et Clonage de Dépôts**

* **`git init`**
    * Initialise un nouveau dépôt Git vide dans le répertoire courant.
* **`git clone <url_du_depot>`**
    * Copie un dépôt distant existant sur votre machine locale.
    * Exemple : `git clone https://github.com/votre-nom-utilisateur/votre-depot.git`

### 3. **Vérification du Statut**

* **`git status`**
    * Affiche l'état actuel de votre répertoire de travail et de la zone de staging.
    * Indique quels fichiers sont modifiés, stagés ou non suivis.

### 4. **Staging des Modifications**

* **`git add <nom_du_fichier>`**
    * Stager des modifications spécifiques dans un fichier pour le prochain commit.
    * Exemple : `git add index.html`
* **`git add .`**
    * Stager toutes les modifications (fichiers nouveaux, modifiés, supprimés) dans le répertoire courant et ses sous-répertoires.
* **`git reset <nom_du_fichier>`**
    * Détacher un fichier (déplace les modifications de la zone de staging vers le répertoire de travail).
    * Exemple : `git reset index.html`

### 5. **Committing des Modifications**

* **`git commit -m "Votre message de commit"`**
    * Enregistre les modifications stagées dans l'historique du dépôt avec un message descriptif.
    * Exemple : `git commit -m "feat: Ajout de l'authentification utilisateur"`
* **`git commit -am "Votre message de commit"`**
    * Combine `add .` et `commit -m` pour **les fichiers suivis uniquement**. (Soyez prudent avec les fichiers non suivis).
* **`git commit --amend`**
    * Permet de modifier le message du *dernier* commit ou d'ajouter/supprimer des modifications stagées au dernier commit.

### 6. **Visualisation de l'Historique**

* **`git log`**
    * Affiche l'historique des commits (auteur, date, message, hachage du commit).
* **`git log --oneline`**
    * Affiche un résumé condensé, sur une seule ligne, de chaque commit.
* **`git log --graph --oneline --all`**
    * Une commande puissante pour visualiser l'historique des commits, y compris les branches.

### 7. **Annulation des Modifications (Attention !)**

* **`git restore <nom_du_fichier>`** (ou `git checkout -- <nom_du_fichier>` pour les anciennes versions de Git)
    * Annule les modifications dans le répertoire de travail pour un fichier spécifique (revient à la dernière version committée).
* **`git reset --hard <hachage_du_commit>`**
    * **DANGEREUX !** Déplace le pointeur de la branche vers un commit spécifique et **annule toutes les modifications ultérieures** dans le répertoire de travail et la zone de staging. À utiliser avec une extrême prudence.
* **`git revert <hachage_du_commit>`**
    * Crée un *nouveau* commit qui annule les modifications d'un commit précédent. C'est plus sûr pour l'historique publié car cela ne réécrit pas l'historique.

### 8. **Travail avec les Dépôts Distants**

* **`git remote -v`**
    * Liste les dépôts distants configurés pour votre projet.
* **`git push origin <nom_de_la_branche>`**
    * Télécharge vos commits locaux vers la branche spécifiée du dépôt distant.
    * Exemple : `git push origin main`
* **`git pull origin <nom_de_la_branche>`**
    * Récupère les modifications du dépôt distant et les *fusionne* dans votre branche locale actuelle.
* **`git fetch origin <nom_de_la_branche>`**
    * Récupère les modifications du dépôt distant mais ne les **fusionne pas** dans votre branche locale actuelle. Utile pour examiner les modifications avant de les intégrer.

### 9. **Branching et Merging**

* **`git branch`**
    * Liste toutes les branches locales.
* **`git branch <nom_de_la_nouvelle_branche>`**
    * Crée une nouvelle branche locale.
* **`git checkout <nom_de_la_branche>`** (ou `git switch <nom_de_la_branche>` pour les versions plus récentes de Git)
    * Bascule vers une branche existante.
* **`git checkout -b <nom_de_la_nouvelle_branche>`** (ou `git switch -c <nom_de_la_nouvelle_branche>`)
    * Crée une nouvelle branche et y bascule immédiatement.
* **`git merge <branche_a_fusionner>`**
    * Fusionne les modifications de la branche spécifiée dans votre branche actuelle.
    * Exemple : Si vous êtes sur `main`, `git merge feature-branch`
* **`git branch -d <nom_de_la_branche>`**
    * Supprime une branche locale (uniquement si elle est déjà fusionnée).
* **`git branch -D <nom_de_la_branche>`**
    * **Force** la suppression d'une branche locale (même si elle n'est pas fusionnée).

---

## 💡 Bonnes Pratiques

* **Commitez Fréquemment :** Faites de petits commits logiques.
* **Rédigez des Messages de Commit Clairs :** Commencez par un verbe (ex: "feat:", "fix:", "chore:"), puis décrivez le changement de manière concise.
* **Utilisez des Branches :** Travaillez sur de nouvelles fonctionnalités ou des correctifs dans des branches dédiées.
* **Pulez Souvent :** Gardez votre branche locale à jour avec le dépôt distant pour minimiser les conflits de fusion.
