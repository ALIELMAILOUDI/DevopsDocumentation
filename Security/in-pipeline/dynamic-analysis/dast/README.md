# GitLab DAST (Dynamic Application Security Testing)

## Rôle

GitLab DAST analyse activement votre application web ou API en cours d'exécution à la recherche de vulnérabilités en simulant des attaques depuis l'extérieur. Contrairement au SAST, le DAST peut identifier des problèmes qui ne se manifestent qu'en runtime, tels que :
* Les failles d'authentification
* Les problèmes de gestion de session
* Les failles d'injection (ex: Injection SQL, Injection de Commande)
* Le contrôle d'accès défaillant
* Les mauvaises configurations menant à des vulnérabilités

Il est crucial pour trouver des vulnérabilités dans une application déployée que le SAST pourrait manquer.

## Intégration dans GitLab CI/CD

GitLab DAST est intégré de manière transparente dans votre pipeline en incluant son modèle. Il s'exécute généralement après le déploiement de votre application dans un environnement de test (comme une Review App dans k3s) qui est accessible depuis le GitLab Runner.

**Configuration dans `.gitlab-ci-security-includes.yml` (déjà incluse) :**

```yaml
# ... (extrait) ...

# Dynamic Application Security Testing (DAST)
# DAST nécessite une application en cours d'exécution (ex: une Review App ou un environnement de staging).
# Décommentez et configurez si vous prévoyez d'utiliser DAST.
# - template: Security/DAST.gitlab-ci.yml
# dast:
#   stage: test_dynamic_security # DAST devrait s'exécuter après le déploiement de votre application dans un environnement de test
#   variables:
#     DAST_WEBSITE: "[http://votre-url-review-app.exemple.com](http://votre-url-review-app.exemple.com)" # !!! IMPORTANT: Remplacez par l'URL réelle
#     DAST_BROWSER_SCAN: "true" # Optionnel: activer le scan basé sur le navigateur pour des tests plus complets
#     DAST_EXCLUDE_RULES: "10009" # Optionnel: Exclure des règles ZAP spécifiques (ex: 10009 pour "Timestamp Disclosure")
#   artifacts:
#     reports:
#       dast: gl-dast-report.json
