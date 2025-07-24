# OPA Gatekeeper - Contrôle d'Admission Kubernetes

## Rôle dans Notre Projet

**OPA Gatekeeper** est un contrôleur d'admission dynamique pour Kubernetes qui applique des politiques basées sur **Open Policy Agent (OPA)**. Il est notre première ligne de défense pour garantir que seules les ressources Kubernetes **conformes à nos politiques de sécurité et de gouvernance** sont admises dans le cluster.

Contrairement à Falco qui surveille le comportement *runtime*, Gatekeeper intervient au moment où une ressource est soumise à l'API Kubernetes.

### Fonctionnement

Quand un utilisateur ou un service tente de créer, mettre à jour ou supprimer une ressource Kubernetes (Pod, Deployment, Service, ConfigMap, etc.), l'API Server envoie cette requête à Gatekeeper. Gatekeeper évalue la ressource par rapport à un ensemble de **contraintes** (définitions de politiques) que nous avons déployées.

* Si la ressource viole une contrainte, Gatekeeper peut **rejeter la requête d'admission** (mode `Deny`).
* Il peut aussi simplement **auditer la violation** (mode `Audit`) pour que nous puissions la corriger manuellement ou l'utiliser pour la conformité.

## Avantages Clés

* **Prévention des Mauvaises Configurations :** Empêche les déploiements non sécurisés ou non conformes d'atteindre le cluster.
* **Gouvernance Centralisée :** Toutes les politiques sont définies en code (Rego) et gérées de manière centralisée.
* **Flexibilité :** Le langage Rego est très puissant et permet de définir des politiques très spécifiques et complexes.
* **Conformité :** Aide à maintenir la conformité avec les normes de sécurité (ex: NIST, CIS Kubernetes Benchmark) en appliquant des règles strictes.

---

## Contenu du Répertoire

* **`helm-charts/gatekeeper-values.yaml` :** Contient les valeurs personnalisées pour le déploiement du Helm Chart de Gatekeeper.
* **`scripts/install-gatekeeper.sh` :** Script pour l'installation initiale de Gatekeeper.
* **`constraints/` :** Ce répertoire contient nos **`ConstraintTemplates`** et **`Constraints`** personnalisées.
    * **`ConstraintTemplates`** : Définissent le "moule" ou le schéma d'une politique, spécifiant les paramètres et la logique Rego.
    * **`Constraints`** : Sont des instances de `ConstraintTemplates`, où nous spécifions les valeurs pour les paramètres et les portées (quels namespaces, quels types de ressources sont affectés).

## Exemples de Politiques Appliquées

* Interdire les conteneurs privilégiés.
* Exiger des limites de CPU/mémoire pour tous les pods.
* Forcer l'utilisation de registres d'images approuvés.
* Interdire l'utilisation d'images avec le tag `latest`.
* Exiger des étiquettes (labels) spécifiques sur les ressources.

Pour plus de détails, visitez le [site officiel d'OPA Gatekeeper](https://open-policy-agent.github.io/gatekeeper/website/docs/).
