# Analyse IaC (Infrastructure as Code) - Conftest

## Rôle

L'Infrastructure as Code (IaC), comme nos manifestes Kubernetes et nos charts Helm, est la base de notre infrastructure. Il est crucial de s'assurer que cette IaC est non seulement fonctionnelle, mais aussi **conforme à nos politiques de sécurité et aux meilleures pratiques** avant d'être déployée.

**Conftest** est l'outil que nous utilisons pour valider nos configurations IaC. Il permet de :

* **Détecter les mauvaises configurations de sécurité** dans les manifestes Kubernetes (ex: conteneurs privilégiés, secrets en clair, images sans tag, ports exposés inutilement).
* **Appliquer des standards de conformité** spécifiques à notre organisation ou à l'industrie.
* **Prévenir le déploiement** d'infrastructures non sécurisées ou non conformes.

## Fonctionnement

Conftest utilise le langage de politique **Rego** (issu d'Open Policy Agent - OPA) pour définir des règles. Le job Conftest dans notre pipeline :

1.  Rend les templates Helm en manifestes Kubernetes YAML.
2.  Exécute Conftest sur ces manifestes, en les comparant à nos politiques Rego définies dans `security/in-pipeline/static-analysis/iac-scanning/conftest/policies/`.
3.  Signale toute non-conformité.

## Politiques (Rego)

Nos politiques Rego sont stockées dans le sous-répertoire `conftest/policies/`. Elles sont organisées par type de ressource ou domaine de sécurité (ex: `kubernetes/`, `general/`).

**Exemples de politiques :**
* Interdire l'utilisation de conteneurs privilégiés.
* S'assurer que les images Docker référencées utilisent des tags immuables (SHA) plutôt que `latest`.
* Vérifier la présence de limites de ressources pour les pods.
* Assurer que les secrets ne sont pas stockés en texte clair dans les manifestes.

## Résultats

Le job Conftest est configuré avec `allow_failure: false`. Cela signifie que si une violation de politique est détectée, le pipeline **échouera**, empêchant le déploiement d'une IaC non conforme. Les logs du job détailleront les violations trouvées.

## Intégration dans le Pipeline

Voir les lignes de configuration dans `security/in-pipeline/.gitlab-ci-security-includes.yml` pour le job `conftest_iac_scan`.

Pour plus de détails sur Conftest et Rego, consultez la [documentation officielle de Conftest](https://www.conftest.dev/).
