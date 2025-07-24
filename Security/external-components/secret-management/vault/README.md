# HashiCorp Vault - Déploiement et Configuration

Ce répertoire contient les détails complets pour le déploiement, l'initialisation et la configuration de HashiCorp Vault. Vault est un composant **critique** de notre infrastructure de sécurité pour la gestion des secrets.

---

## Architecture de Déploiement

Nous déployons Vault en mode **HA (Haute Disponibilité)** au sein de notre cluster k3s en utilisant le **Helm Chart officiel de HashiCorp**. Cette approche assure la résilience et la scalabilité de notre service de gestion des secrets.

Le stockage backend pour Vault est configuré pour utiliser **Raft**, une solution de consensus intégrée qui ne nécessite pas de base de données externe (comme Consul ou un stockage persistant) pour la haute disponibilité.

---

## Processus d'Installation et de Configuration

Le processus global d'installation et de configuration de Vault suit les étapes suivantes :

1.  **Déploiement du Helm Chart :** Installation des composants de Vault dans le cluster k3s.
2.  **Initialisation de Vault :** La première fois que Vault est déployé, il doit être initialisé. Cela génère des clés de déverrouillage (unseal keys) et le jeton root initial.
3.  **Déverrouillage de Vault (Unseal) :** Après chaque redémarrage du Pod Vault ou de l'application Vault, il doit être déverrouillé avec un certain nombre de clés de déverrouillage (seuil de partage de clés).
4.  **Configuration des Méthodes d'Authentification :** Configuration des méthodes par lesquelles les utilisateurs et les applications (Pods Kubernetes) peuvent s'authentifier auprès de Vault.
5.  **Configuration des Engines Secrets :** Activation et configuration des backends de secrets (ex: `kv` pour les paires clé-valeur, `database` pour les identifiants dynamiques de DB).
6.  **Définition des Politiques :** Création des politiques d'accès qui définissent ce que les utilisateurs/applications peuvent lire, écrire ou gérer.

---

## Contenu du Répertoire

* **`helm-charts/vault-values.yaml` :**
    Ce fichier contient les valeurs personnalisées que nous utilisons pour déployer le Helm Chart de Vault. Cela inclut la configuration du stockage Raft, des ressources, des services, et d'autres paramètres spécifiques à notre environnement.
* **`scripts/` :**
    Contient des scripts d'automatisation ou des instructions pas à pas pour les tâches post-déploiement :
    * `install-vault.sh` : Script pour l'installation initiale du Helm Chart de Vault.
    * `init-unseal-vault.sh` : Instructions ou un script pour le processus d'initialisation et de déverrouillage manuel/semi-automatique. C'est une étape cruciale pour la sécurité.
    * `configure-k8s-auth.sh` : Script pour configurer la méthode d'authentification Kubernetes de Vault, permettant aux Pods de s'authentifier via leurs Service Accounts.
* **`policies/` :**
    Contient les fichiers HCL (HashiCorp Configuration Language) définissant les politiques d'accès de Vault. Chaque politique spécifie les chemins et les capacités (lecture, écriture, création, etc.) pour les secrets.

## Gestion des Clés de Déverrouillage (Unseal Keys)

Les clés de déverrouillage sont **extrêmement sensibles**. Elles ne doivent jamais être stockées dans un système de contrôle de version ou dans un endroit facilement accessible. Il est impératif de suivre les meilleures pratiques de sécurité pour la distribution et le stockage de ces clés (par exemple, un coffre-fort physique, un gestionnaire de mots de passe sécurisé pour l'équipe, ou une procédure de quorum).

---

Pour la documentation officielle détaillée, visitez le [site de HashiCorp Vault](https://www.vaultproject.io/docs).
