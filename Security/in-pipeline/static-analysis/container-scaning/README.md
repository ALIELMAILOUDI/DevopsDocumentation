# GitLab Container Scanning (propulsé par Trivy)

## Rôle

Le scan de conteneurs est une étape essentielle pour garantir que les images Docker que nous construisons et déployons ne contiennent pas de vulnérabilités connues dans leurs couches logicielles (système d'exploitation et dépendances). Même si notre code applicatif est sécurisé, une vulnérabilité dans une bibliothèque système sous-jacente peut être exploitée.

GitLab Container Scanning utilise **Trivy**, un scanner de vulnérabilités open-source très efficace, pour réaliser cette tâche.

## Fonctionnement

Ce scan s'exécute généralement après la phase de `build` du pipeline, une fois que l'image Docker a été construite. Il analyse les différentes couches de l'image à la recherche de paquets logiciels avec des vulnérabilités connues, en consultant les bases de données CVE.

## Résultats

Les rapports de vulnérabilités des conteneurs sont intégrés dans GitLab :

* **Widgets de Merge Request :** Affichage des nouvelles vulnérabilités découvertes dans l'image construite par la MR.
* **Tableau de Bord de Sécurité GitLab :** Vue centralisée des vulnérabilités d'image.
* **Rapports de Vulnérabilités :** Détails techniques sur chaque vulnérabilité, y compris la sévérité et la version corrigée.

## Personnalisation et Exclusions

* **`allow_failure: true` :** Il est courant de configurer le job de Container Scanning pour qu'il **permette l'échec**. Cela signifie que même si des vulnérabilités sont trouvées, le pipeline peut continuer. Cela est utile pour ne pas bloquer les développeurs sur des vulnérabilités non critiques ou sans solution immédiate, tout en assurant que l'information est collectée et visible. Les vulnérabilités doivent ensuite être triées et traitées séparément.

Pour plus de détails, consultez la [documentation officielle de GitLab Container Scanning](https://docs.gitlab.com/ee/user/application_security/container_scanning/).
