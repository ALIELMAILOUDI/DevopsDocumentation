# GitLab SAST (Static Application Security Testing)

## Rôle

GitLab SAST est un outil essentiel pour la **détection précoce des vulnérabilités** dans le code source de nos applications. Il analyse le code avant même son exécution, nous permettant d'identifier et de corriger des failles de sécurité courantes telles que :

* Injections (SQL, Commandes, etc.)
* Cross-Site Scripting (XSS)
* Mauvaise gestion de l'authentification/session
* Exposition de données sensibles
* Mauvaises configurations de sécurité

L'intégration de SAST dans le pipeline CI/CD permet aux développeurs de recevoir un retour rapide sur les vulnérabilités introduites, favorisant ainsi une approche "shift-left" de la sécurité.

## Fonctionnement

GitLab SAST s'intègre automatiquement via l'inclusion de son template. Il exécute différents analyseurs (selon le langage de programmation détecté) et agrège les résultats.

## Résultats

Les vulnérabilités détectées par SAST sont affichées de manière centralisée :

* **Widgets de Merge Request :** Les développeurs voient immédiatement les nouvelles vulnérabilités introduites par leurs modifications de code.
* **Tableau de Bord de Sécurité GitLab :** Vue d'ensemble agrégée de toutes les vulnérabilités SAST à travers le projet.
* **Rapports de Vulnérabilités :** Accès détaillé aux informations sur chaque vulnérabilité, y compris sa gravité et des liens vers des solutions potentielles.

## Personnalisation

Bien que le template par défaut soit très efficace, des personnalisations peuvent être ajoutées dans `.gitlab-ci-security-includes.yml` pour, par exemple :

* Exclure certains analyseurs (`SAST_EXCLUDED_ANALYZERS`).
* Modifier les chemins de scan.
* Définir des règles personnalisées (bien que cela soit plus avancé).

Pour plus de détails, référez-vous à la [documentation officielle de GitLab SAST](https://docs.gitlab.com/ee/user/application_security/sast/).
