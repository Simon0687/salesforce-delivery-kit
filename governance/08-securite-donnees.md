# 08 – Sécurité, accès & données

## 1. Modèle d'accès

| Principe | Règle |
|---|---|
| Profils | Minimaux (connexion, paramètres de base) ; nombre limité, un par type de licence si possible |
| Permissions | Accordées via **Permission Sets** regroupés en **Permission Set Groups** par persona |
| Permissions sensibles | « Modify All Data », « View All Data », « Author Apex », « Manage Users », « Export Reports » : liste nominative validée par le RSSI |
| OWD | Aussi restrictif que possible (Private par défaut), ouverture via rôles, règles de partage, équipes |
| Utilisateurs externes | Licences et modèle de partage Experience Cloud validés en Design Authority ; OWD externe Private |
| Authentification | SSO ({{IDP}}) ; MFA obligatoire pour tout accès direct à l'interface |
| Utilisateurs d'intégration | Licence *Salesforce Integration*, API Only, permissions minimales, un utilisateur par système |

### Matrice persona → accès

| Persona | Licence | Profil | Permission Set Group | Rôle | Nb utilisateurs |
|---|---|---|---|---|---|
| Commercial | Sales Cloud | `Standard User (min)` | `PSG_Sales_Representative` | Commercial Région X | |
| Manager commercial | | | | | |
| Agent service client | | | | | |
| Admin | | | | | |
| Intégration ERP | Salesforce Integration | `Minimum Access - API Only` | `PSG_Integration_ERP` | – | 1 |

## 2. Contrôles de sécurité récurrents

| Contrôle | Fréquence | Responsable |
|---|---|---|
| Salesforce **Health Check** (score cible ≥ {{SCORE_HEALTH_CHECK}} %) | Trimestrielle | Admin |
| Revue des accès (utilisateurs actifs, permissions sensibles, comptes inactifs > 90 j) | Trimestrielle | Admin + managers |
| Revue des Connected Apps / External Client Apps et Named Credentials | Semestrielle | Architecte |
| Revue du Setup Audit Trail | Mensuelle | Admin |
| Analyse des packages AppExchange installés | Annuelle | Architecte |

## 3. Conformité RGPD

- [ ] Registre des traitements mis à jour avec les traitements portés par Salesforce (DPO)
- [ ] Classification des champs (Data Classification : sensibilité, catégorie de conformité) sur les données personnelles
- [ ] Base légale et durée de conservation par type de donnée
- [ ] Mécanisme de purge / anonymisation automatisé (batch ou Flow planifié)
- [ ] Traitement des demandes d'exercice de droits (accès, effacement, portabilité) documenté
- [ ] Données masquées dans toutes les sandboxes contenant une copie de prod
- [ ] Besoin de chiffrement (Shield Platform Encryption) et de journalisation avancée (Event Monitoring, Field Audit Trail) évalué

## 4. Gouvernance des données

| Rôle | Responsabilité |
|---|---|
| Data owner (métier) | Définit les règles de gestion et la qualité attendue d'un domaine (Comptes, Contacts…) |
| Data steward | Surveille la qualité, traite les doublons et anomalies |
| Admin | Met en œuvre les règles (validation, duplicate rules, picklists) |

Règles :
- Clés externes (`External ID`) sur tout objet alimenté par un système tiers.
- Duplicate Rules et Matching Rules actives sur Account, Contact, Lead.
- Système maître identifié pour chaque donnée (tableau ci-dessous).
- Tableau de bord qualité (complétude, doublons, fraîcheur) revu mensuellement.
- Surveillance du stockage (données et fichiers) et politique d'archivage (Big Objects, archivage externe) au-delà de {{SEUIL_STOCKAGE}} %.

| Donnée | Système maître | Sens de synchro | Fréquence |
|---|---|---|---|
| Comptes clients | | | |
| Produits / prix | | | |
| Commandes / factures | | | |

## 5. Reprise de données

1. Cartographie source → cible validée par les data owners.
2. Règles de nettoyage et de dédoublonnage **avant** chargement.
3. Ordre de chargement respectant les dépendances (utilisateurs → comptes → contacts → opportunités → activités…).
4. Automatisations désactivables pendant le chargement (bypass par Custom Permission / Custom Metadata).
5. Au moins deux répétitions complètes en UAT (Full Copy), avec mesure de la durée.
6. Rapports de contrôle (comptages, sommes de contrôle) validés par le métier.
7. Plan de retour arrière (suppression par External ID / lot de chargement).
