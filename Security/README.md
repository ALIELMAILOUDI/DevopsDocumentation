# Documentation Sécurité

Ce répertoire contient l'ensemble de la documentation, des configurations et des scripts liés à la sécurité de notre projet DevOps. Notre approche consiste à intégrer la sécurité à chaque étape du cycle de vie du développement logiciel (SDLC), une pratique souvent appelée **DevSecOps**, dans le but de "déplacer la sécurité vers la gauche" en l'incorporant tôt et de manière continue.

Notre stratégie de sécurité s'appuie à la fois sur les fonctionnalités natives de nos outils principaux (GitLab, k3s) et sur des solutions de sécurité spécialisées pour couvrir divers vecteurs d'attaque.

---

### Sécurité In-Pipeline (`in-pipeline/`)

Cette section regroupe les outils et configurations directement intégrés dans nos pipelines GitLab CI/CD. Il s'agit principalement de contrôles automatisés exécutés à chaque modification de code, construction ou phase de déploiement. L'objectif est de détecter et de prévenir les vulnérabilités le plus tôt possible dans le processus de développement.

* **Analyse Statique :** SAST, Scan de Dépendances, Détection de Secrets, Scan de Conteneurs, Scan IaC.
* **Analyse Dynamique :** DAST (souvent exécuté sur des environnements de revue/staging déployés).

Toutes les principales fonctionnalités de sécurité in-pipeline sont principalement fournies par les **Scans de Sécurité Intégrés de GitLab**, qui s'intègrent parfaitement aux widgets des Merge Requests et au Tableau de Bord de Sécurité centralisé.

---

### Composants Externes (`external-components/`)

Cette section couvre les outils et services qui opèrent en dehors du pipeline CI/CD immédiat. Ils assurent une protection continue en temps réel, une gestion sécurisée des secrets pour notre infrastructure et nos applications, ainsi qu'un monitoring et des alertes continues.

* **Gestion des Secrets :** HashiCorp Vault pour une gestion centralisée, sécurisée et auditable des secrets.
* **Sécurité Runtime :** Falco pour la détection des menaces en temps réel dans Kubernetes.
* **Application des Politiques :** OPA Gatekeeper pour le contrôle d'admission dans k3s, garantissant que seules les ressources conformes sont déployées.
* **Monitoring et Alerting :** Utilisation de Prometheus, Loki et Grafana pour visualiser la posture de sécurité et alerter sur les anomalies ou incidents.

---


