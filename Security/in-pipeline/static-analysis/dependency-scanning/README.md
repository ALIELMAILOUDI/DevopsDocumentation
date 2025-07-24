# GitLab Dependency Scanning (Analyse de Dépendances)

## Rôle

GitLab Dependency Scanning, également connu sous le nom de Software Composition Analysis (SCA), est crucial pour identifier les **vulnérabilités connues dans les bibliothèques et dépendances tierces** que notre projet utilise. La plupart des applications modernes s'appuient fortement sur des composants open-source, et ces composants peuvent contenir des failles de sécurité.

Cet outil aide à :
* Détecter les CVE (Common Vulnerabilities and Exposures) associées à des versions spécifiques de dépendances.
* Fournir des informations sur les chemins de remédiation (mises à jour de version).
* Réduire le risque lié à l'utilisation de composants avec des vulnérabilités publiques.

## Fonctionnement

Le scan de dépendances analyse les fichiers de gestion de dépendances (ex: `package.json`, `Gemfile.lock`, `pom.xml`, `requirements.txt`) pour identifier les bibliothèques et leurs versions. Il compare ensuite ces informations avec des bases de données de vulnérabilités publiques pour signaler les correspondances.

## Résultats

Comme pour le SAST, les résultats sont intégrés directement dans GitLab :

* **Widgets de Merge Request :** Affichage des nouvelles vulnérabilités de dépendances introduites.
* **Tableau de Bord de Sécurité GitLab :** Vue consolidée des vulnérabilités de dépendances à travers le projet.
* **Rapports de Vulnérabilités :** Détails sur chaque vulnérabilité, y compris la CVE, la version affectée et la version corrigée si disponible.

## Personnalisation

Le template par défaut de GitLab est généralement suffisant. Cependant, vous pouvez affiner le comportement en ajustant des variables dans `.gitlab-ci-security-includes.yml`, par exemple pour inclure ou exclure certains répertoires de scan.

Pour plus de détails, consultez la [documentation officielle de GitLab Dependency Scanning](https://docs.gitlab.com/ee/user/application_security/dependency_scanning/).

