# Dossier : Gestion de Paquets avec Helm 📦

Ce dossier est dédié à Helm, le gestionnaire de paquets pour Kubernetes. Helm simplifie le déploiement et la gestion d'applications complexes sur Kubernetes en utilisant des "Charts".

---

## 1. Introduction à Helm

* **Le problème sans Helm :** Déployer une application comme notre `loginapp` nécessite de gérer plusieurs fichiers YAML (Deployment, Service, Ingress, Secret, etc.). Les mises à jour et les configurations spécifiques à l'environnement deviennent vite complexes.
* **Qu'est-ce que Helm ?** Helm est un outil en ligne de commande qui permet de définir, installer et mettre à jour des applications Kubernetes sous forme de "Charts" Helm.
* **Concepts Clés :**
    * **Chart :** Un package Helm qui contient toutes les ressources Kubernetes nécessaires pour une application, ainsi que sa configuration. C'est comme un package `apt` ou `npm` pour Kubernetes.
    * **Release :** Une instance d'un Chart déployée dans un cluster Kubernetes. Vous pouvez avoir plusieurs releases d'un même Chart.
    * **Repository :** Un endroit où les Charts Helm sont stockés et peuvent être partagés.

---

## 2. Installation de Helm Localement 🚀

L'installation de Helm est simple et rapide.

1.  **Prérequis :** Avoir un cluster Kubernetes fonctionnel (comme notre K3s) et `kubectl` configuré pour y accéder.
2.  **Téléchargement et Installation (Linux/Ubuntu) :**
    ```bash
    curl [https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3](https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3) | bash
    ```
    *Cette commande télécharge le script d'installation officiel de Helm et l'exécute pour installer la dernière version de Helm 3.*
3.  **Vérifier l'installation :**
    ```bash
    helm version
    ```
    

---

## 3. Commandes de Base Helm 💻

Voici les commandes essentielles pour travailler avec Helm :

* **Initialiser un nouveau Chart :**
    ```bash
    helm create <nom-du-chart>
    # Exemple : helm create loginapp-chart
    ```
    Ceci crée une structure de répertoire standard pour un Chart Helm.
* **Lister les Releases déployées :**
    ```bash
    helm list
    ```
* **Installer un Chart :** Déploie une nouvelle instance d'application dans votre cluster.
    ```bash
    helm install <nom-de-la-release> <chemin-vers-le-chart>
    # Exemple : helm install loginapp-prod ./loginapp-chart
    ```
* **Mettre à jour une Release :** Applique les modifications d'un Chart existant à une release déployée.
    ```bash
    helm upgrade <nom-de-la-release> <chemin-vers-le-chart>
    # Exemple : helm upgrade loginapp-prod ./loginapp-chart
    ```
* **Supprimer une Release :** Désinstalle une application et supprime ses ressources Kubernetes.
    ```bash
    helm uninstall <nom-de-la-release>
    # Exemple : helm uninstall loginapp-prod
    ```
* **Inspecter un Chart ou une Release :**
    ```bash
    helm show values <chemin-vers-le-chart> # Affiche les valeurs par défaut d'un chart
    helm get values <nom-de-la-release>    # Affiche les valeurs utilisées par une release
    helm get manifest <nom-de-la-release>  # Affiche les manifestes Kubernetes générés pour une release
    helm template <nom-de-la-release> <chemin-vers-le-chart> --debug # Génère les manifestes sans les déployer (utile pour le débogage)
    ```
* **Vérifier la syntaxe et les meilleures pratiques d'un Chart :**
    ```bash
    helm lint <chemin-vers-le-chart>
    ```
 
* **Packager un Chart :** Crée un fichier `.tgz` (package du Chart) pour le distribuer.
    ```bash
    helm package <chemin-vers-le-chart>
    # Exemple : helm package ./loginapp-chart
    ```

