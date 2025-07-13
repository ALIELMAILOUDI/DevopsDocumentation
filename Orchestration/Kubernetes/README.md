# Dossier : Orchestration avec Kubernetes

Ce dossier contient les manifestes Kubernetes pour déployer l'application `loginapp` (frontend React et backend Spr sur un cluster Kubernetes. Nous utiliserons K3s pour le développement local.

## 1. Installation de K3s Localement

Assurez-vous que Docker est installé sur votre machine Ubuntu.

1.  **Installer K3s :**
    ```bash
    curl -sfL https://get.k3s.io | sh -
    ```
2.  **Vérifier l'état de K3s :**
    ```bash
    sudo systemctl status k3s
    kubectl get nodes
    ```
3.  **Configurer `kubectl` :**
    ```bash
    mkdir -p ~/.kube
    sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
    sudo chown $(id -u):$(id -g) ~/.kube/config
    chmod 600 ~/.kube/config
    ```
    Cela permet à `kubectl` d'utiliser la configuration K3s sans `sudo`.

## 2. Commandes de Base `kubectl`

* **Vérifier le cluster :**
    ```bash
    kubectl cluster-info
    kubectl get pods -A
    ```
* **Voir les ressources :**
    ```bash
    kubectl get deployments
    kubectl get services
    kubectl get pods
    ```
* **Décrire une ressource (pour le débogage) :**
    ```bash
    kubectl describe pod <nom-du-pod>
    kubectl describe service <nom-du-service>
    ```
* **Afficher les logs d'un pod :**
    ```bash
    kubectl logs <nom-du-pod>
    ```
* **Entrer dans un conteneur :**
    ```bash
    kubectl exec -it <nom-du-pod> -- /bin/bash
    ```

## 3. Déploiement de `loginapp` sur Kubernetes

Les fichiers YAML pour le déploiement de `loginapp` se trouvent dans les sous-dossiers `deployments/`, `services/`, `secrets/`, et `ingress/`.

**Ordre de déploiement recommandé :**

1.  **Secrets (si utilisés) :**
    ```bash
    kubectl apply -f secrets/secret.yaml
    ```
2.  **Deployments (et leurs Pods) :**
    ```bash
    kubectl apply -f deployments/deployment.yaml
    ```
3.  **Services (exposition interne) :**
    ```bash
    kubectl apply -f services/service.yaml
    ```
4.  **Ingress (exposition externe) :**
    ```bash
    kubectl apply -f ingress/ingress.yaml
    ```

---

## Comprendre Traefik avec K3s

K3s installe **Traefik** comme Ingress Controller par défaut. Cela signifie que vous n'avez pas à l'installer séparément ; il est déjà en place lorsque vous installez K3s.

### Vérifier l'Installation de Traefik

Pour confirmer que Traefik est bien en cours d'exécution dans votre cluster K3s :

```bash
kubectl get pods -n kube-system -l app.kubernetes.io/name=traefik
kubectl get svc -n kube-system -l app.kubernetes.io/name=traefik

**Vérification après déploiement :**

```bash
kubectl get deployments
kubectl get services
kubectl get pods
kubectl get ingress


