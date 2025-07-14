# Dossier : GitOps et Déploiement Continu avec Argo CD 🔄

Ce dossier est dédié à la mise en œuvre du **GitOps** en utilisant **Argo CD** pour automatiser le déploiement de notre application `loginapp` sur Kubernetes. Nous verrons comment Argo CD s'intègre avec GitLab (pour le code source et les manifests) et Helm (pour la gestion des applications).

---

## 1. Comprendre le GitOps 🐙

Le GitOps est un paradigme d'opération qui utilise Git comme source unique de vérité pour l'état déclaratif de l'infrastructure et des applications.

* **Idée Clé :** Au lieu de pousser des changements vers le cluster Kubernetes (approche impérative du CI traditionnel), on pousse des changements vers un dépôt Git. Un opérateur (comme Argo CD) surveille ce dépôt et synchronise l'état du cluster avec l'état défini dans Git.
* **Avantages du GitOps :**
    * **Traçabilité Complète :** Chaque changement est un commit Git, avec un historique, un auteur et un message.
    * **Rollback Facile :** Pour revenir à un état précédent, il suffit de revenir à un commit précédent dans Git.
    * **Sécurité Améliorée :** Moins d'accès direct au cluster. Les développeurs poussent vers Git, pas vers Kubernetes.
    * **Collaboration Simplifiée :** Les PR/MR pour l'infrastructure et les applications.
    * **Détection des Dérives (Drift Detection) :** L'opérateur GitOps peut détecter si l'état réel du cluster diffère de l'état souhaité dans Git et les signaler (ou les corriger automatiquement).

---

## 2. Argo CD : Le Moteur GitOps 🚢

**Argo CD** est un outil de déploiement continu pour Kubernetes qui implémente le modèle GitOps. Il fonctionne comme un contrôleur Kubernetes qui surveille en continu un ou plusieurs dépôts Git et synchronise l'état du cluster avec les manifests définis dans Git.

### a) Installation d'Argo CD avec Helm 🛠️

L'installation d'Argo CD via Helm est la méthode recommandée pour une gestion simple et reproductible.

1.  **Prérequis :**
    * Un cluster Kubernetes fonctionnel (ex: K3s).
    * `kubectl` configuré pour accéder à votre cluster.
    * `helm` installé.

2.  **Créer un Namespace pour Argo CD :**
    Il est courant d'isoler Argo CD dans son propre Namespace.
    ```bash
    kubectl create namespace argocd
    ```

3.  **Ajouter le dépôt Helm d'Argo CD :**
    ```bash
    helm repo add argo [https://argoproj.github.io/argo-helm](https://argoproj.github.io/argo-helm)
    helm repo update
    ```

4.  **Installer Argo CD avec Helm :**
    ```bash
    helm install argocd argo/argo-cd -n argocd
    ```
    * Cette commande déploie tous les composants d'Argo CD (serveur API, contrôleurs, serveur de repo, etc.) dans le Namespace `argocd`.

5.  **Vérifier l'état de l'installation :**
    ```bash
    kubectl get pods -n argocd
    kubectl get svc -n argocd
    ```
    Assurez-vous que tous les Pods sont en état `Running`.

### b) Accéder à l'Interface Utilisateur d'Argo CD 🌐

1.  **Récupérer le mot de passe initial de l'administrateur :**
    Le mot de passe initial pour l'utilisateur `admin` est généré et stocké dans un Secret Kubernetes.
    ```bash
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
    ```
    *Copiez ce mot de passe.*

2.  **Exposer l'interface utilisateur d'Argo CD :**
    Par défaut, le service Argo CD est de type `ClusterIP`. Pour y accéder localement, utilisez le port forwarding :
    ```bash
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```
    * Cette commande redirige le port 443 (HTTPS) du service `argocd-server` vers le port 8080 de votre machine locale.

3.  **Accéder à l'interface :**
    Ouvrez votre navigateur web et naviguez vers `https://localhost:8080`.
    * Ignorez l'avertissement de certificat (car c'est un certificat auto-signé).
    * Connectez-vous avec le nom d'utilisateur `admin` et le mot de passe que vous avez récupéré.

4.  **Installer l'outil CLI `argocd` (Optionnel mais recommandé) :**
    L'outil en ligne de commande `argocd` simplifie l'interaction avec le serveur Argo CD.
    ```bash
    # Télécharger le binaire (pour Linux AMD64)
    curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
    chmod +x /usr/local/bin/argocd
    # Se connecter au serveur Argo CD (en utilisant le port forwarding si actif)
    argocd login localhost:8080 --username admin --password <votre-mot-de-passe-admin> --insecure --grpc-web # Utilisez --insecure car c'est un certificat auto-signé
    # Vérifier les applications existantes (il n'y en aura pas encore)
    argocd app list
    ```
