# Outils d'Analyse Statique dans GitLab CI/CD

Les outils d'analyse statique sont conçus pour examiner le code, les dépendances, les images et les fichiers de configuration sans exécuter l'application. Ils sont exécutés tôt dans le pipeline (stage `test_static_security`) pour fournir un retour rapide aux développeurs.

## Outils Inclus :

* **SAST (Static Application Security Testing) :**
    * **GitLab SAST :** Notre outil principal pour l'analyse du code source des applications à la recherche de vulnérabilités. (Voir `sast/README.md`)
* **Scan de Dépendances :**
    * **GitLab Dependency Scanning :** Utilisé pour identifier les vulnérabilités connues dans les bibliothèques et paquets tiers. (Voir `dependency-scanning/README.md`)
* **Détection de Secrets :**
    * **GitLab Secret Detection :** Scanner intégré pour les secrets accidentellement commis.
    * **GitGuardian (ggshield) :** Un outil de détection de secrets supplémentaire et robuste pour une couverture plus large. (Voir `secret-detection/README.md`)
* **Scan de Conteneurs :**
    * **GitLab Container Scanning :** Scanne les images Docker à la recherche de vulnérabilités du système d'exploitation et des dépendances. Il utilise Trivy en interne. (Voir `container-scanning/README.md`)
* **Scan IaC (Infrastructure as Code) :**
    * **Conftest :** Utilisé pour valider les manifestes Kubernetes et les charts Helm par rapport aux politiques OPA personnalisées avant le déploiement. (Voir `iac-scanning/conftest/README.md`)
