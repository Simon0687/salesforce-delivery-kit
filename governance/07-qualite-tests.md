# 07 – Qualité & tests

## 1. Definition of Ready (US prête pour un sprint)

- [ ] Rédigée au format « En tant que… je veux… afin de… »
- [ ] Critères d'acceptation testables (Given / When / Then)
- [ ] Persona et profil / permission set concernés identifiés
- [ ] Impacts identifiés : données, sécurité, intégrations, rapports, automatisations existantes
- [ ] Passage en Design Authority effectué si nécessaire
- [ ] Estimée par l'équipe
- [ ] Dépendances levées

## 2. Definition of Done

- [ ] Développée dans Git, PR revue et approuvée
- [ ] Standards respectés ([05](05-standards-dev.md)) : nommage, descriptions, pas de valeurs en dur
- [ ] Tests Apex ≥ 85 % sur les classes modifiées, Code Analyzer sans violation bloquante
- [ ] Accès donnés via Permission Set (et non par profil)
- [ ] Déployée et testée en QA, critères d'acceptation validés par le PO
- [ ] Étapes manuelles de déploiement documentées (le cas échéant)
- [ ] Documentation mise à jour (inventaire des automatisations, ERD, guide utilisateur)

## 3. Stratégie de test

| Niveau | Objet | Qui | Où | Outil |
|---|---|---|---|---|
| Unitaire | Apex, LWC | Développeurs | DEV / CI | Tests Apex, Jest |
| Fonctionnel | US et critères d'acceptation | Consultants / QA | QA | Cahier de test |
| Intégration | Flux avec les systèmes tiers | QA + équipes tierces | QA / UAT | Scénarios bout en bout |
| Non-régression | Processus critiques | QA | QA / UAT | {{OUTIL_TEST_AUTO}} (Provar, Copado Robotic Testing, Playwright…) |
| Recette (UAT) | Processus métier réels | Key users | UAT | Scénarios métier |
| Sécurité / accès | Visibilité par persona | QA + architecte | UAT | Tests par profil (login as) |
| Performance / volumétrie | Traitements de masse, pages critiques | Architecte / devs | UAT (Full) | Batch de charge, mesures de temps |
| Reprise de données | Complétude et qualité | Data + key users | UAT | Rapports de contrôle |

## 4. Gestion des anomalies

| Criticité | Définition | Délai de correction cible |
|---|---|---|
| Bloquante (P1) | Processus critique impossible, pas de contournement | Immédiat / avant go |
| Majeure (P2) | Fonction dégradée, contournement coûteux | Dans le sprint |
| Mineure (P3) | Gêne, contournement simple | Backlog priorisé |
| Cosmétique (P4) | Libellé, mise en page | Backlog |

## 5. Critères de sortie de recette (Go)

- 100 % des scénarios critiques exécutés et passés
- 0 anomalie P1, P2 ≤ {{SEUIL_P2}} avec contournement accepté par le PO
- Reprise de données validée (taux de rejet < {{SEUIL_REJET}} %)
- PV de recette signé par le PO
