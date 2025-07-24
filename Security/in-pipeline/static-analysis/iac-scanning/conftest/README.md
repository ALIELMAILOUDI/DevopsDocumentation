# Conftest - Outil de Test de Politiques

## Rôle Spécifique

Conftest est un outil de ligne de commande qui nous permet d'écrire des tests pour des fichiers de configuration, y compris les fichiers YAML pour Kubernetes, les modèles Terraform, etc. Il utilise le langage de politique **Rego** d'Open Policy Agent (OPA).

Dans notre projet, Conftest est spécifiquement utilisé pour :

* **Valider la conformité de nos manifestes Kubernetes et de nos charts Helm** par rapport à nos politiques de sécurité et de conformité internes.
* **Intégrer cette validation directement dans notre pipeline CI/CD** afin de détecter les problèmes d'IaC le plus tôt possible ("shift-left").

## Structure des Politiques

Les politiques Rego que Conftest utilise sont stockées dans le sous-répertoire `policies/`. Nous organisons ces politiques pour une meilleure gestion :

* `policies/kubernetes/` : Contient les règles spécifiques aux ressources Kubernetes (Pods, Deployments, Services, etc.).
* `policies/general/` : Contient des règles plus génériques qui peuvent s'appliquer à divers types de fichiers IaC.

## Comment Ajouter une Nouvelle Politique

1.  Créez un nouveau fichier `.rego` dans le répertoire approprié (`policies/kubernetes/` ou `policies/general/`).
2.  Définissez votre règle en utilisant la syntaxe Rego.
3.  Testez votre politique localement avec `conftest test -p <chemin/vers/policies> <votre-fichier-config.yaml>`.
4.  Assurez-vous que le job `conftest_iac_scan` dans le pipeline couvre le chemin des fichiers de configuration que votre nouvelle politique est censée valider.

## Exemple de Politique (simplifié)

Un exemple simple de politique Rego pour interdire les conteneurs privilégiés dans Kubernetes :

```rego
package kubernetes.privileged

deny[msg] {
  input.kind == "Pod"
  some i
  input.spec.containers[i].securityContext.privileged == true
  msg := sprintf("Le conteneur '%v' du Pod '%v' ne doit pas être privilégié.", [input.spec.containers[i].name, input.metadata.name])
}
