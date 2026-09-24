# 04 – Design Authority & principes d'architecture

## 1. Mandat

La Design Authority (DA) garantit la cohérence, la maintenabilité et la scalabilité de l'org Salesforce. Elle :
- valide l'architecture cible et ses évolutions ;
- arbitre les choix déclaratif / code et les écarts aux standards ;
- tient le registre des ADR (`governance/adr/ADR-NNN-titre.md`) ;
- surveille les limites de l'org (limites de gouverneur, stockage, API, licences).

**Présidence** : {{ARCHITECTE}}. **Quorum** : architecte + 1 lead technique + admin client.

## 2. Ce qui passe obligatoirement en Design Authority

- [ ] Nouvel objet custom ou modification du modèle de données partagé (relations, master-detail, record types)
- [ ] Tout développement Apex, LWC ou Visualforce nouveau (hors correctif mineur)
- [ ] Nouvelle intégration ou modification d'une intégration existante
- [ ] Installation d'un package AppExchange ou d'un package non géré
- [ ] Modification du modèle de partage (OWD, rôles, règles de partage)
- [ ] Nouveau type de licence ou nouvelle population d'utilisateurs externes (Experience Cloud)
- [ ] Automatisation déclenchée sur un objet à fort volume (> {{SEUIL_VOLUMETRIE}} enregistrements)
- [ ] Tout écart aux [standards de développement](05-standards-dev.md)

## 3. Principes d'architecture

| # | Principe | Implication |
|---|---|---|
| P1 | **Standard avant custom** | Utiliser les objets standards (Account, Contact, Case, Opportunity…) avant d'en créer ; justifier chaque objet custom. |
| P2 | **Déclaratif avant code** | Suivre l'arbre de décision (§ 4). Le code n'est pas un échec, mais doit être justifié. |
| P3 | **Une automatisation lisible par objet** | Un seul trigger Apex par objet (framework de handler) ; Flows déclenchés par enregistrement ordonnés (Trigger Order) et documentés. |
| P4 | **Conçu pour la volumétrie** | Code bulkifié, requêtes sélectives, anticiper le data skew (> 10 000 enfants par parent). |
| P5 | **Intégrations découplées** | Privilégier les événements (Platform Events, Change Data Capture) et un middleware quand il y a plusieurs systèmes ; Named Credentials obligatoires. |
| P6 | **Sécurité par défaut** | Moindre privilège, `WITH USER_MODE` / `with sharing` par défaut, aucun secret dans le code. |
| P7 | **Configuration, pas de valeurs en dur** | Custom Metadata Types pour la configuration, Custom Labels pour les textes ; pas d'ID en dur. |
| P8 | **Tout est dans Git** | Les métadonnées de l'org sont versionnées ; Git est la référence, pas l'org. |
| P9 | **Réversibilité** | Chaque fonctionnalité peut être désactivée (feature flag via Custom Metadata / Custom Permission). |

## 4. Arbre de décision déclaratif / code

```
Le besoin est-il couvert par une fonctionnalité standard ou une configuration ?
 ├─ Oui ─► Configuration
 └─ Non ─► Un Flow peut-il le faire de façon maintenable ?
            (< ~50 éléments, pas de logique complexe sur collections, pas de volumétrie massive)
            ├─ Oui ─► Flow (record-triggered avant/après, screen flow, scheduled)
            └─ Non ─► Existe-t-il un package AppExchange mature et supporté ?
                       ├─ Oui ─► Évaluation (coût, sécurité, dépendance) ─► ADR
                       └─ Non ─► Apex / LWC ─► ADR si composant structurant
```

Critères poussant vers le code : traitement de masse (batch, > 2 000 enregistrements), logique transactionnelle complexe, callouts avec gestion d'erreurs avancée, performance critique, réutilisation forte.

## 5. Livrables d'architecture tenus à jour

| Livrable | Contenu | Mise à jour |
|---|---|---|
| Vue d'ensemble du système | Clouds, systèmes tiers, flux | À chaque nouvelle intégration |
| Modèle de données (ERD) | Objets, relations, record types, volumétrie | À chaque nouvel objet |
| Catalogue des intégrations | Système, sens, protocole, fréquence, volume, authentification, responsable | À chaque changement |
| Inventaire des automatisations par objet | Triggers, Flows, validation rules, dans l'ordre d'exécution | À chaque sprint |
| Modèle de sécurité | Voir [08](08-securite-donnees.md) | À chaque changement |
| Registre des ADR | Décisions structurantes | Continu |
| Tableau de suivi des limites | Stockage données/fichiers, appels API/24 h, licences | Mensuel |

## 6. Déroulé d'une séance

1. Suivi des actions de la séance précédente (5 min)
2. Présentation des sujets inscrits (demandeur, 10 min max chacun, ADR en brouillon envoyé 48 h avant)
3. Décision : **Approuvé** / **Approuvé avec réserves** / **Refusé** / **Complément demandé**
4. Mise à jour du registre ADR et du RAID
