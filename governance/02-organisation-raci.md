# 02 – Organisation, instances & RACI

## 1. Rôles

| Rôle | Titulaire | Responsabilités clés |
|---|---|---|
| Sponsor | {{SPONSOR}} | Porte la vision, arbitre budget et périmètre, préside le COPIL |
| Chef de projet | {{CHEF_PROJET}} | Planning, budget, risques, reporting, animation des instances |
| Product Owner | {{PRODUCT_OWNER}} | Backlog, priorisation, acceptation des user stories, recette |
| Key users / référents métier | | Expression du besoin, tests, relais auprès des utilisateurs |
| Architecte Salesforce | {{ARCHITECTE}} | Architecture cible, anime la Design Authority, garant des standards |
| Business analyst / consultant fonctionnel | | Ateliers, spécifications, configuration |
| Développeurs | | Apex, LWC, intégrations, tests unitaires |
| Release manager | | Environnements, pipeline CI/CD, déploiements, calendrier de releases |
| Admin Salesforce (client) | | Administration courante, utilisateurs, futur RUN |
| QA / testeur | | Stratégie de test, tests de non-régression |
| RSSI / DPO | | Validation sécurité et conformité RGPD |
| Responsable conduite du changement | | Communication, formation, mesure de l'adoption |

## 2. Instances de gouvernance

| Instance | Fréquence | Durée | Participants | Objet | Livrable |
|---|---|---|---|---|---|
| **COPIL** | Mensuelle + jalons | 1 h | Sponsor, CP, PO, direction intégrateur | Avancement, budget, risques majeurs, arbitrages de périmètre, Go/No-Go | [CR COPIL](templates/cr-copil.md) |
| **COPROJ** | Hebdomadaire | 1 h | CP, PO, architecte, leads | Planning, RAID, dépendances, préparation des arbitrages | RAID à jour |
| **Design Authority** | Bi-mensuelle + à la demande | 1 h | Architecte (président), leads tech, admin client, RSSI si besoin | Validation des solutions, écarts aux standards, ADR | [ADR](templates/adr.md) |
| **CAB (Change Advisory Board)** | Avant chaque release | 30 min | Release manager, PO, architecte, admin | Validation du contenu de release et du plan de déploiement | [Checklist MEP](templates/checklist-mep.md) |
| **Rituels agiles** | Selon sprint | – | Équipe | Planning, daily, review, rétrospective, affinage du backlog | Incrément, backlog priorisé |

### Règles de fonctionnement

- Ordre du jour envoyé 48 h avant ; CR diffusé sous 48 h après.
- Une décision est valable si le décideur (A du RACI) est présent ou a délégué par écrit.
- Toute décision est consignée dans le registre RAID (type « D »).

## 3. Circuit d'escalade

| Niveau | Instance | Délai de résolution attendu | Exemples |
|---|---|---|---|
| 1 | Équipe / PO | 2 jours | Clarification fonctionnelle, priorité intra-sprint |
| 2 | COPROJ / Design Authority | 1 semaine | Choix technique, conflit de ressources, dérive < 10 % |
| 3 | COPIL / Sponsor | Prochain COPIL ou COPIL exceptionnel | Changement de périmètre, dérive budget/planning > 10 %, risque go-live |

Les risques sont escaladés selon leur criticité (P × I) : voir [Gestion des risques](10-gestion-risques.md#4-seuils-de-criticité-et-escalade).

## 4. Matrice RACI

R = Réalise · A = Approuve (un seul par ligne) · C = Consulté · I = Informé

| Activité | Sponsor | CP | PO | Key users | Architecte | Consultants/Devs | Release mgr | Admin client | RSSI/DPO |
|---|---|---|---|---|---|---|---|---|---|
| Charte et budget | A | R | C | I | C | I | I | I | I |
| Priorisation du backlog | C | C | A/R | C | C | I | I | C | I |
| Rédaction des user stories | I | I | A | C | C | R | I | C | I |
| Architecture cible | I | C | C | I | A/R | C | C | C | C |
| Écart aux standards (ADR) | I | I | C | I | A | R | C | C | C |
| Modèle de sécurité et de partage | I | I | C | C | A | R | I | C | C |
| Build (config / code) | I | I | C | I | C | R | I | I | I |
| Revue de code | I | I | I | I | A | R | I | I | I |
| Tests unitaires et d'intégration | I | I | I | I | A | R | C | I | I |
| Recette (UAT) | I | C | A | R | I | C | I | C | I |
| Plan de reprise de données | I | C | C | C | A | R | C | C | C |
| Déploiement en production | I | C | C | I | C | C | R | A | I |
| Go / No-Go | A | R | C | C | C | I | C | C | C |
| Formation et communication | C | A | C | R | I | C | I | C | I |
| Changement de périmètre | A | R | C | I | C | I | I | I | I |
| Tenue du registre RAID | I | A/R | C | C | C | C | C | I | I |
| Acceptation d'un risque critique | A | R | C | I | C | I | I | I | C |
| Conformité RGPD | I | C | C | I | C | R | I | C | A |
| Gestion des accès utilisateurs | I | I | C | I | C | I | I | A/R | C |
