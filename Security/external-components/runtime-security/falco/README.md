# Falco - Sécurité Runtime Cloud-Native

## Rôle dans Notre Projet

Falco est un projet open-source de sécurité runtime natif du cloud, sous l'égide de la Cloud Native Computing Foundation (CNCF). Il agit comme un moniteur d'activité comportemental conçu pour détecter les activités anormales dans vos applications et votre infrastructure.

Pour nos clusters k3s, le rôle de Falco est **critique** pour :

1.  **Détection des Menaces en Temps Réel :**
    * **Surveillance Comportementale :** Falco observe l'activité des conteneurs et des hôtes (via les appels système Linux, les logs d'audit Kubernetes, etc.) pour identifier des comportements suspects.
    * **Exemples de détections :**
        * L'exécution d'un shell (`bash`, `sh`) à l'intérieur d'un serveur web.
        * L'accès non autorisé à des fichiers sensibles (ex: `/etc/shadow`, `/root/.ssh`).
        * Des connexions réseau inattendues depuis un conteneur.
        * L'exécution de conteneurs privilégiés ou de processus avec des capacités Linux inhabituelles.
        * La modification de fichiers système critiques.

2.  **Alertes en Temps Réel :**
    * Lorsqu'une activité suspecte est détectée (en fonction de règles prédéfinies ou personnalisées), Falco génère des alertes.
    * Ces alertes sont envoyées à notre pile de monitoring (Prometheus/Grafana via l'exportateur Falco et Loki pour les logs), permettant une réaction rapide aux incidents de sécurité.

3.  **Application de Politiques au Runtime :**
    * Bien qu'OPA Gatekeeper gère le contrôle d'admission, Falco complète cette approche en appliquant des politiques au niveau du comportement d'exécution.
    * Il s'assure que les applications et les conteneurs se comportent de manière conforme aux meilleures pratiques de sécurité *pendant qu'ils s'exécutent*, et non seulement au moment de leur déploiement.

## Valeur Ajoutée pour le Projet

* **Visibilité Approfondie :** Falco fournit une visibilité granulaire sur ce qui se passe réellement à l'intérieur de nos conteneurs et sur nos nœuds, bien au-delà de ce que les logs applicatifs traditionnels peuvent offrir.
* **Défense en Profondeur :** Il ajoute une couche de sécurité essentielle à notre stratégie de défense en profondeur, en interceptant des menaces qui pourraient avoir échappé aux scans statiques ou au contrôle d'admission.
* **Réponse aux Incidents :** Les alertes précises de Falco sont cruciales pour réduire le temps de détection (MTTD) et le temps de réponse (MTTR) lors d'incidents de sécurité.
* **Conformité :** Il aide à prouver la conformité en surveillant et en alertant sur les écarts par rapport aux politiques de sécurité définies.

En somme, Falco est notre sentinelle de sécurité runtime, offrant une surveillance continue et une capacité d'alerte instantanée contre les comportements malveillants ou non conformes au sein de notre environnement k3s.

---

## Déploiement

Nous déployons Falco via son Helm chart officiel. Les configurations spécifiques sont définies dans `helm-charts/falco-values.yaml`.
