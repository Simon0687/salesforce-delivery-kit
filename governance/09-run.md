# 09 – RUN & amélioration continue

## 1. Hypercare

- Durée : {{DUREE_HYPERCARE}} semaines après le go-live.
- Équipe projet mobilisée, point quotidien de 15 min sur les incidents.
- Critères de sortie : aucun incident P1 depuis 2 semaines, backlog d'incidents P2 < {{SEUIL_P2_HYPERCARE}}, documentation transférée, équipe RUN formée.

## 2. Organisation du support

| Niveau | Acteur | Périmètre |
|---|---|---|
| N0 | Key users / documentation | Questions d'usage, FAQ |
| N1 | Admin Salesforce / service desk | Accès, paramétrage simple, rapports |
| N2 | Équipe de maintenance (intégrateur / TMA) | Anomalies, évolutions mineures |
| N3 | Support Salesforce / éditeurs AppExchange | Incidents plateforme |

| Priorité | Délai de prise en charge | Délai de résolution |
|---|---|---|
| P1 | {{SLA_P1_PEC}} | {{SLA_P1_RES}} |
| P2 | | |
| P3 | | |

## 3. Indicateurs de pilotage (revus en COPIL RUN mensuel)

| Domaine | Indicateur |
|---|---|
| Adoption | Utilisateurs actifs (connexion sur 30 j) / licences attribuées ; usage des fonctionnalités clés |
| Valeur | KPI de la [charte](01-charte-projet.md) |
| Qualité | Incidents par priorité, délai de résolution, taux de régression après release |
| Delivery | US livrées par release, lead time demande → production |
| Plateforme | Health Check, stockage, consommation API, licences inutilisées |
| Dette technique | Nombre de violations Code Analyzer, Flows/Process Builder hérités restant à migrer, champs inutilisés |

## 4. Dette technique

- Registre de la dette dans le backlog (label `tech-debt`).
- 20 % de la capacité réservée (cf. [03](03-demande-changements.md)).
- Revue semestrielle de l'org : champs et objets inutilisés, rapports orphelins, permission sets redondants, versions de Flow obsolètes, versions d'API anciennes (Salesforce Optimizer ou outil équivalent).

## 5. Revue de gouvernance

Une fois par an (ou à chaque changement majeur), la gouvernance elle-même est revue :
- les instances sont-elles utiles et tenues ?
- les standards sont-ils respectés et à jour avec la plateforme ?
- le RACI reflète-t-il l'organisation réelle ?

Mettre à jour ce kit en conséquence.
