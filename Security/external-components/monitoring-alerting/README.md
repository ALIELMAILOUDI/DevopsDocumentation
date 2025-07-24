# Monitoring et Alerting de Sécurité

Le monitoring et l'alerting ne sont pas seulement pour la performance ou la disponibilité ; ils sont une **composante essentielle de la sécurité runtime**. En surveillant les métriques, les logs et les événements liés à la sécurité, nous pouvons détecter les anomalies, les tentatives d'intrusion, les compromissions et les violations de politiques en temps réel.

Notre pile de monitoring centralisée est basée sur :
* **Prometheus :** Collecte de métriques.
* **Loki :** Agrégation de logs.
* **Grafana :** Visualisation et tableaux de bord.
* **Alertmanager :** Gestion des alertes (intégré à Prometheus).

Cette section détaille comment nous utilisons ces outils spécifiquement pour la sécurité.

---

## Rôle des Composants

* **Prometheus (`prometheus/`) :**
    * Collecte les métriques de sécurité exposées par des services (ex: Falco, Gatekeeper).
    * Évalue les **règles d'alerte de sécurité** définies dans `prometheus/security-alert-rules.yaml`.
    * Envoie les alertes à Alertmanager.
* **Loki :**
    * Centralise tous les logs de nos applications, des pods Kubernetes, et des systèmes d'exploitation (via Promtail).
    * Permet de rechercher des événements de sécurité dans les logs.
* **Grafana (`grafana/`) :**
    * **Tableaux de bord de sécurité :** Visualisation des métriques de sécurité (ex: nombre de violations Gatekeeper, alertes Falco).
    * Analyse des logs de sécurité agrégés par Loki.
    * Offre une vue d'ensemble de la posture de sécurité et des tendances.
* **Alertmanager :**
    * Reçoit les alertes de Prometheus et les achemine vers les canaux de notification appropriés (ex: Slack, email, PagerDuty).
    * Gère le dédoublonnage, le groupement et la suppression des alertes.

## Stratégie de Monitoring de Sécurité

1.  **Collecte des Métriques et Logs Pertinents :** Assurer que les événements et métriques des outils de sécurité (Falco, Gatekeeper, logs d'audit Kubernetes) sont correctement ingérés dans Prometheus et Loki.
2.  **Règles d'Alerte :** Définir des seuils et des conditions dans Prometheus pour déclencher des alertes sur des indicateurs de compromission ou de non-conformité (ex: trop d'alertes Falco de haute criticité, trop de requêtes refusées par Gatekeeper).
3.  **Tableaux de Bord :** Créer des tableaux de bord Grafana dédiés à la sécurité pour avoir une vue d'ensemble en temps réel des activités de sécurité, des tendances et de l'état des systèmes.
4.  **Réponse aux Incidents :** Utiliser les alertes et les capacités d'exploration des logs pour faciliter l'investigation et la réponse rapide aux incidents de sécurité.

Ce setup de monitoring est vital pour une détection proactive et une réponse efficace aux menaces dans notre environnement de production.
