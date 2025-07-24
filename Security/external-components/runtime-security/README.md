# Sécurité Runtime

La sécurité runtime est la dernière ligne de défense dans notre stratégie DevSecOps. Alors que la sécurité in-pipeline se concentre sur la prévention des vulnérabilités avant le déploiement, la sécurité runtime protège nos applications et notre infrastructure **pendant qu'elles sont en cours d'exécution** dans l'environnement de production.

Elle est cruciale pour détecter :
* Les attaques "zero-day" (exploits inconnus)
* Les activités malveillantes qui contournent les contrôles statiques.
* Les comportements anormaux ou non autorisés.
* Les mouvements latéraux ou les escalades de privilèges.

---

## Outils Inclus :

* **Falco :**
    * Notre outil principal pour la détection des menaces comportementales en temps réel au sein de notre cluster k3s.
    * Il surveille les appels système et les événements Kubernetes pour alerter sur les activités suspectes. (Voir `falco/README.md`)

* **OPA Gatekeeper :**
    * Un contrôleur d'admission pour Kubernetes basé sur Open Policy Agent (OPA).
    * Il applique des politiques au moment de l'admission (lorsqu'une ressource est créée ou mise à jour), garantissant que seuls les déploiements conformes sont autorisés. (Voir `opa-gatekeeper/README.md`)

Ces outils fonctionnent en synergie pour fournir une surveillance et une application des politiques continues, renforçant ainsi la résilience de notre environnement face aux menaces en direct.
