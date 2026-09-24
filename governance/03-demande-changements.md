# 03 – Gestion de la demande & des changements

## 1. Cycle de vie d'une demande

```
Idée / besoin ─► Qualification (PO) ─► Analyse d'impact ─► Priorisation ─► Backlog ─► Sprint ─► Release
                     │                      │
                     └─ Rejet motivé        └─ Design Authority si impact d'architecture
```

| Étape | Responsable | Délai cible | Sortie |
|---|---|---|---|
| Soumission | Demandeur | – | Demande dans l'outil ({{OUTIL_BACKLOG}} : Jira, Azure DevOps, Agile Accelerator…) |
| Qualification | PO | 5 j ouvrés | Type, valeur métier, décision « on étudie / on rejette » |
| Analyse d'impact | Consultant / architecte | 5 j ouvrés | Estimation, impacts (données, sécurité, intégrations, licences) |
| Priorisation | PO (+ COPIL si hors périmètre) | Prochain affinage | Rang dans le backlog |

## 2. Typologie des demandes

| Type | Exemple | Circuit |
|---|---|---|
| Paramétrage simple | Nouvelle valeur de picklist, modification de page layout, rapport | Admin, backlog RUN, pas de Design Authority |
| Évolution fonctionnelle | Nouveau processus, nouvel objet | Backlog projet, Design Authority si nouvel objet, code ou intégration |
| Évolution technique | Refactoring, montée de version d'API, dette technique | Backlog technique, Design Authority |
| Changement de périmètre | Nouveau cloud, nouvelle population | Change request + COPIL |
| Urgence (hotfix) | Anomalie bloquante en production | Circuit accéléré (§ 4) |

## 3. Priorisation

Méthode par défaut : **MoSCoW** pour le périmètre d'une release, **WSJF** (ou valeur/effort) pour ordonner le backlog.

Critères de valeur :
- impact métier (CA, productivité, satisfaction client) ;
- obligation réglementaire ou fin de support (retrait de fonctionnalité par Salesforce) ;
- réduction de risque ou de dette ;
- nombre d'utilisateurs concernés.

Capacité de sprint indicative : **70 %** évolutions, **20 %** dette technique et maintenance, **10 %** imprévus.

## 4. Change request (changement de périmètre, budget ou planning)

Déclenchée dès qu'une demande :
- ajoute ou retire un élément du périmètre de la charte ;
- ou modifie la charge de plus de {{SEUIL_CR}} j/h ;
- ou décale un jalon.

Processus : rédaction avec le [modèle](templates/change-request.md) → analyse d'impact (CP + architecte) → décision en COPIL → mise à jour de la charte, du planning et du registre RAID.

## 5. Hotfix (urgence de production)

1. Qualification de la criticité par l'admin / le support (P1 = blocage de processus critique, sans contournement).
2. Correctif développé sur une branche `hotfix/*` depuis `main`, validé en sandbox de préproduction (UAT ou full copy).
3. Validation orale ou écrite par le PO et l'architecte, puis déploiement par le release manager.
4. Report du correctif dans les branches de développement (back-merge) et régularisation au CAB suivant.

**Aucune modification directe en production**, y compris en urgence (exceptions tracées : données, utilisateurs, gestion des licences).
