# 01 – Charte projet : {{NOM_PROJET}}

| | |
|---|---|
| Client | {{CLIENT}} |
| Sponsor | {{SPONSOR}} |
| Chef de projet | {{CHEF_PROJET}} |
| Version / date | v0.1 – AAAA-MM-JJ |
| Statut | Brouillon / Validé en COPIL du AAAA-MM-JJ |

## 1. Contexte

_Situation actuelle, outils existants, points de douleur, déclencheur du projet._

## 2. Objectifs

| # | Objectif métier | Indicateur (KPI) | Valeur actuelle | Cible | Échéance |
|---|---|---|---|---|---|
| O1 | Ex. Réduire le temps de traitement des demandes | Durée moyenne de résolution des Cases | 72 h | 24 h | T+6 mois |
| O2 | Ex. Adoption de l'outil par les commerciaux | Utilisateurs actifs hebdo / licences | – | 85 % | T+3 mois |
| O3 | | | | | |

## 3. Périmètre

### Inclus

- Clouds / produits : {{CLOUDS}}
- Processus : …
- Populations d'utilisateurs : … (nombre, type de licence)
- Intégrations : …
- Reprise de données : objets, volumétrie, historique repris

### Exclu

- …

### Hypothèses et contraintes

- Licences disponibles : …
- Édition Salesforce : Enterprise / Unlimited
- Contraintes réglementaires : RGPD, hébergement (Hyperforce région …), secteur …
- Date contrainte : go-live {{DATE_GOLIVE}}

## 4. Approche et jalons

Méthode : _agile (sprints de N semaines) / hybride / cycle en V_.

| Jalon | Date cible | Critère de passage |
|---|---|---|
| Lancement (kick-off) | | Charte validée, équipe staffée |
| Fin de cadrage | | Backlog priorisé, architecture cible validée en Design Authority |
| Fin de build (MVP) | | Toutes les user stories MVP « Done » |
| Fin de recette (UAT) | | 0 anomalie bloquante, PV de recette signé |
| Go / No-Go | | Checklist MEP complète, go signé en COPIL |
| Mise en production | {{DATE_GOLIVE}} | |
| Fin d'hypercare | | Transfert au RUN validé |

> Vérifier les dates des releases Salesforce (Spring, Summer, Winter) et des fenêtres de maintenance de l'instance : éviter un go-live dans les 2 semaines autour d'une release majeure.

## 5. Budget et ressources

| Poste | Charge (j/h) | Coût |
|---|---|---|
| Intégrateur / build | | |
| Équipe interne | | |
| Licences Salesforce et AppExchange | | |
| Outillage (DevOps, sauvegarde, tests) | | |
| Formation / conduite du changement | | |
| Provision pour risques ({{PROVISION_RISQUES}} %, cf. [10](10-gestion-risques.md#6-provision-pour-risques)) | | |

## 6. Risques majeurs initiaux

Les 3 à 5 risques principaux issus de l'atelier d'identification ([gestion des risques](10-gestion-risques.md)), tenus à jour dans le [registre RAID](templates/raid-log.csv).

| ID | Risque | Criticité (P × I) | Stratégie | Responsable |
|---|---|---|---|---|
| R-001 | | | | |

## 7. Validation

| Rôle | Nom | Date | Visa |
|---|---|---|---|
| Sponsor | {{SPONSOR}} | | |
| Chef de projet | {{CHEF_PROJET}} | | |
| Product Owner | {{PRODUCT_OWNER}} | | |
