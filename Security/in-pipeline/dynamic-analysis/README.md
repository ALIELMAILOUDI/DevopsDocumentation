# Outils d'Analyse Dynamique dans GitLab CI/CD

Les outils d'Analyse Dynamique de la Sécurité des Applications (DAST) analysent une application en cours d'exécution à la recherche de vulnérabilités en sondant activement ses interfaces (par exemple, interface web, APIs). Ces tests sont généralement effectués plus tard dans le pipeline (stage `test_dynamic_security`), souvent sur une Review App ou un environnement de staging déployé.

## Outils Inclus :

* **DAST :**
    * **GitLab DAST :** Notre outil principal pour les tests de sécurité dynamiques des applications web. (Voir `dast/README.md`)
