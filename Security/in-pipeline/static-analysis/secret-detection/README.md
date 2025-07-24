# Détection de Secrets (GitLab & GitGuardian)

## Rôle

La détection de secrets est une pratique essentielle pour empêcher l'exposition accidentelle de données sensibles (clés API, identifiants, jetons, certificats, etc.) dans notre code source, nos fichiers de configuration ou l'historique de notre dépôt Git. Un secret exposé peut entraîner des compromissions graves.

Nous utilisons une approche à deux niveaux pour maximiser la couverture :

1.  **GitLab Secret Detection :** Le scanner intégré de GitLab pour une première couche de défense.
2.  **GitGuardian (ggshield) :** Un outil tiers plus robuste pour une détection avancée, y compris dans l'historique Git et avec des heuristiques plus sophistiquées.

## Fonctionnement

Ces outils analysent les modifications de code et l'historique du dépôt à la recherche de motifs (regex) qui correspondent à des structures de secrets connues.

### GitLab Secret Detection

* S'intègre via le template GitLab officiel.
* Scan sur les nouvelles modifications et l'historique.

### GitGuardian (ggshield)

* **Installation :** Nécessite l'installation de `ggshield` dans l'image du runner.
* **Authentification :** Utilise une clé API GitGuardian configurée comme variable CI/CD (`GG_API_KEY`).
* **Configuration Personnalisée :** Le fichier `.ggshield.yml` (situé dans ce répertoire) permet de définir des exclusions de chemins ou des règles spécifiques pour réduire les faux positifs et cibler précisément les scans.

## Résultats

Les résultats des deux outils sont visibles :

* **Widgets de Merge Request :** Alertes directes en cas de secrets détectés dans les modifications.
* **Tableau de Bord de Sécurité GitLab :** Vue consolidée.
* **Sortie du Pipeline :** Pour GitGuardian, les détails sont affichés dans les logs du job CI/CD.

## Importance

La détection de secrets est vitale. Un seul secret exposé peut compromettre l'ensemble de notre infrastructure. Il est **fortement recommandé de configurer ces jobs pour qu'ils fassent échouer le pipeline (`allow_failure: false`)** si des secrets sont détectés, forçant ainsi leur remédiation immédiate.

Pour plus de détails :
* [Documentation officielle GitLab Secret Detection](https://docs.gitlab.com/ee/user/application_security/secret_detection/)
* [Documentation officielle GitGuardian](https://docs.gitguardian.com/ggshield/)
