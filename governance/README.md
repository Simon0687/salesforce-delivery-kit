# Template de gouvernance – Projets Salesforce

Kit réutilisable pour cadrer la gouvernance d'un projet Salesforce (build, RUN ou évolution d'une org existante).

## Utilisation

1. Copier le dossier `governance/` dans le dépôt ou l'espace documentaire du nouveau projet.
2. Remplacer les variables `{{...}}` (liste ci-dessous) — une recherche globale sur `{{` permet de n'en oublier aucune.
3. Supprimer les sections non pertinentes (ex. Shield, intégrations) plutôt que de les laisser vides.
4. Faire valider les documents 01 à 03 en COPIL de lancement ; les autres sont validés par la Design Authority.

| Variable | Exemple |
|---|---|
| `{{NOM_PROJET}}` | Refonte Service Client |
| `{{CLIENT}}` | ACME |
| `{{SPONSOR}}` | Directrice Relation Client |
| `{{CHEF_PROJET}}` | … |
| `{{ARCHITECTE}}` | Architecte technique / CTA |
| `{{PRODUCT_OWNER}}` | … |
| `{{CLOUDS}}` | Sales Cloud, Service Cloud, Experience Cloud |
| `{{ORG_PROD_ID}}` | 00D… |
| `{{DATE_GOLIVE}}` | AAAA-MM-JJ |

Paramètres par domaine :

| Domaine | Variables (valeurs suggérées) |
|---|---|
| Outillage | `{{OUTIL_BACKLOG}}` (Jira), `{{OUTIL_DEVOPS}}` (Gearset, DevOps Center…), `{{OUTIL_LOG}}` (Nebula Logger), `{{OUTIL_TEST_AUTO}}` (Provar…), `{{OUTIL_BACKUP}}` (Own, Salesforce Backup…), `{{IDP}}` (Entra ID, Okta) |
| Standards | `{{PREFIXE}}`, `{{TRIGGER_FRAMEWORK}}` |
| Release | `{{DUREE_SPRINT}}` (2-3), `{{FENETRE_MEP}}`, `{{AUTRES_GELS}}`, `{{VERSION}}` |
| Seuils | `{{SEUIL_CR}}` (5 j/h), `{{SEUIL_VOLUMETRIE}}` (1 M), `{{SEUIL_P2}}` (3), `{{SEUIL_REJET}}` (1), `{{SEUIL_STOCKAGE}}` (80), `{{SCORE_HEALTH_CHECK}}` (80) |
| RUN | `{{DUREE_HYPERCARE}}` (4), `{{SEUIL_P2_HYPERCARE}}` (5), `{{SLA_P1_PEC}}` (1 h), `{{SLA_P1_RES}}` (4 h) |

## Contenu

| # | Document | Objet | Valideur |
|---|---|---|---|
| 01 | [Charte projet](01-charte-projet.md) | Contexte, objectifs, périmètre, KPI | COPIL |
| 02 | [Organisation & RACI](02-organisation-raci.md) | Rôles, instances, rituels, escalade | COPIL |
| 03 | [Gestion de la demande & des changements](03-demande-changements.md) | Intake, priorisation, change requests | COPIL |
| 04 | [Design Authority & architecture](04-design-authority.md) | Principes, arbitrages déclaratif/code, ADR | Design Authority |
| 05 | [Standards de développement](05-standards-dev.md) | Nommage, Apex, Flow, LWC, qualité de code | Design Authority |
| 06 | [Environnements & release management](06-environnements-release.md) | Stratégie d'orgs, branches, CI/CD, releases | Design Authority |
| 07 | [Qualité & tests](07-qualite-tests.md) | DoR/DoD, stratégie de test, recette | PO + QA |
| 08 | [Sécurité, accès & données](08-securite-donnees.md) | Modèle d'accès, RGPD, qualité et migration des données | Design Authority + RSSI/DPO |
| 09 | [RUN & amélioration continue](09-run.md) | Support, dette technique, releases Salesforce | COPIL |

### Modèles (`templates/`)

- [ADR – Architecture Decision Record](templates/adr.md)
- [Compte rendu COPIL](templates/cr-copil.md)
- [Change request](templates/change-request.md)
- [Checklist de mise en production](templates/checklist-mep.md)
- [Registre RAID](templates/raid-log.csv) (Risques, Actions, Issues, Décisions)
- [Note de version](templates/release-note.md)

## Principes directeurs

1. **Standard d'abord** : fonctionnalité native > configuration > déclaratif (Flow) > code. Tout écart est justifié par un ADR.
2. **Une seule source de vérité** : les métadonnées vivent dans Git ; aucune modification directe en production.
3. **Sécurité par défaut** : moindre privilège, accès via Permission Sets, revue d'accès trimestrielle.
4. **Décisions tracées** : toute décision structurante est inscrite au registre RAID et, si technique, dans un ADR.
5. **Rythme Salesforce** : le planning intègre les 3 releases annuelles de la plateforme.
