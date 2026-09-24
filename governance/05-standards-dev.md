# 05 – Standards de développement

## 1. Conventions de nommage

| Élément | Convention | Exemple |
|---|---|---|
| Objet custom | Nom métier au singulier, PascalCase avec `_` | `Contract_Amendment__c` |
| Champ custom | PascalCase avec `_`, pas d'abréviation obscure | `Renewal_Date__c` |
| Champ technique | Suffixe `_Tech` ou préfixe `Tech_` (à choisir et s'y tenir) | `Tech_External_Id__c` |
| Description | **Obligatoire** sur objets, champs, Flows, classes (objet + demandeur/US) | « US-123 – Date de renouvellement calculée » |
| Record-triggered Flow | `<Objet>_<Before/After>_<Action>` | `Case_After_NotifyOwner` |
| Screen Flow | `SCR_<Processus>` | `SCR_CreateReturnRequest` |
| Scheduled / Autolaunched Flow | `SCH_` / `AUTO_` + processus | `SCH_CloseStaleCases` |
| Classe Apex | PascalCase + suffixe de rôle | `AccountTriggerHandler`, `AccountService`, `AccountSelector`, `InvoiceBatch` |
| Classe de test | `<Classe>Test` | `AccountServiceTest` |
| Trigger | `<Objet>Trigger` (un par objet) | `AccountTrigger` |
| LWC | camelCase | `invoiceSummary` |
| Permission Set | `PS_<Domaine>_<Niveau>` | `PS_Sales_Manager` |
| Permission Set Group | `PSG_<Persona>` | `PSG_Sales_Representative` |
| Validation rule | `VR_<Règle>` | `VR_CloseDate_Not_In_Past` |
| Custom Metadata Type | `<Domaine>_Setting__mdt` | `Integration_Setting__mdt` |

Préfixe de projet optionnel si plusieurs projets cohabitent dans l'org : `{{PREFIXE}}_`.

## 2. Apex

- **Un trigger par objet**, sans logique, délégant à un handler (framework retenu : {{TRIGGER_FRAMEWORK}}, ex. fflib, Kevin O'Hara, maison).
- Séparation des responsabilités : Handler → Service → Selector (requêtes) / Domain.
- **Bulkification obligatoire** : aucune requête SOQL ni DML dans une boucle.
- Sécurité : classes `with sharing` par défaut (`without sharing` justifié en commentaire) ; requêtes `WITH USER_MODE` ou `Security.stripInaccessible` pour les données exposées à l'utilisateur.
- Pas d'ID, d'URL ou de secret en dur : Custom Metadata, Custom Labels, Named Credentials.
- Gestion des erreurs : exceptions typées, journalisation centralisée ({{OUTIL_LOG}}, ex. Nebula Logger), pas de `catch` vide.
- Asynchrone : préférer Queueable à `@future` ; Batch pour la volumétrie.

### Tests Apex

| Règle | Seuil |
|---|---|
| Couverture globale de l'org (minimum Salesforce) | 75 % |
| **Couverture cible du projet, par classe** | **≥ 85 %** |
| Assertions | Au moins une assertion significative par méthode de test (`Assert.areEqual`…) |
| Données | Créées dans le test (Test Data Factory), jamais `SeeAllData=true` |
| Cas couverts | Nominal, erreur, volumétrie (200 enregistrements), utilisateur restreint (`System.runAs`) |
| Callouts | Mockés (`HttpCalloutMock`) |

## 3. Flow

- Un Flow = une responsabilité ; utiliser des subflows pour la réutilisation.
- Before-save pour la mise à jour de champs de l'enregistrement déclencheur (plus performant).
- Conditions d'entrée systématiques pour limiter les exécutions.
- Aucun élément Get/Update/Create dans une boucle.
- **Fault path** obligatoire sur les éléments DML et les actions, avec journalisation ou notification.
- Pas d'ID en dur (utiliser Custom Metadata ou des requêtes par DeveloperName).
- Description renseignée sur le Flow et ses éléments principaux ; versions obsolètes supprimées régulièrement.
- Ne pas créer de Workflow Rules ni de Process Builder (retirés par Salesforce) ; migrer l'existant vers Flow.

## 4. LWC

- LWC plutôt qu'Aura ou Visualforce pour tout nouveau composant.
- Lightning Data Service / `lightning/ui*Api` avant Apex quand c'est possible.
- Méthodes Apex `@AuraEnabled(cacheable=true)` pour la lecture.
- Accessibilité : composants `lightning-*` de base, libellés via Custom Labels.
- Tests Jest pour les composants avec logique.

## 5. Qualité de code et revue

- Analyse statique : **Salesforce Code Analyzer** (PMD, ESLint, règles Flow) exécuté en CI ; aucune violation de sévérité 1-2 tolérée sur le nouveau code.
- Formatage : Prettier (plugin Apex) avec configuration partagée dans le dépôt.
- **Revue de code obligatoire** (pull request, 1 approbateur minimum, 2 pour les composants structurants).
- Checklist de revue : bulkification, sécurité (CRUD/FLS, sharing), gestion des erreurs, tests, nommage, description, absence de valeurs en dur.

## 6. Documentation

- Chaque US référence les métadonnées modifiées (dans la PR).
- Champ `Description` renseigné sur toute métadonnée qui le permet.
- ApexDoc sur les classes et méthodes publiques.
