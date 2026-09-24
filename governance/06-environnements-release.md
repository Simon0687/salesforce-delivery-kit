# 06 – Environnements & release management

## 1. Stratégie d'environnements

| Environnement | Type | Usage | Rafraîchissement | Données |
|---|---|---|---|---|
| DEV-xx | Scratch org ou Developer sandbox (une par développeur) | Développement | À la demande | Jeu de test minimal (scripts) |
| INT | Developer Pro | Intégration continue des branches | Par sprint | Jeu de test scripté |
| QA | Partial Copy | Tests fonctionnels et de non-régression | Par release | Échantillon anonymisé |
| UAT / PREPROD | Full Copy | Recette métier, tests de performance, répétition de MEP | Avant chaque release majeure | Copie de prod **masquée** (Data Mask) |
| PROD | Production | – | – | – |

Règles :
- Les connexions aux systèmes tiers de chaque sandbox pointent vers les environnements hors-prod correspondants (Named Credentials à reconfigurer après rafraîchissement).
- Script ou checklist de post-rafraîchissement : désactivation des emails (Deliverability = System email only), masquage, reconfiguration des intégrations, désactivation des jobs planifiés.
- Nombre de sandboxes et types disponibles selon l'édition et les licences : vérifier dans Setup > Sandboxes.

## 2. Stratégie de branches (Git)

Modèle par défaut : branches par environnement.

```
feature/US-123-description ─► int ─► qa ─► uat ─► main (= PROD)
hotfix/INC-45 ─────────────────────────────────► main ─► back-merge vers uat/qa/int
```

- Une branche par user story, préfixée par l'identifiant.
- Fusion par pull request uniquement ; `main` et `uat` protégées.
- Messages de commit : `US-123: description courte`.
- Outil DevOps : {{OUTIL_DEVOPS}} (Salesforce DevOps Center, Gearset, Copado, GitHub Actions + Salesforce CLI…).

## 3. Pipeline CI/CD

| Étape | Déclencheur | Contrôles |
|---|---|---|
| Validation PR | Ouverture de PR | Code Analyzer, tests Jest, déploiement en validation (`--dry-run`) avec tests locaux |
| Déploiement INT | Fusion dans `int` | Déploiement + tests Apex |
| Déploiement QA / UAT | Fusion | Déploiement + tests + smoke tests |
| Production | Release validée au CAB | Validation préalable (quick deploy), `RunLocalTests` ou `RunSpecifiedTests` |

## 4. Cycle de release

| Type | Fréquence | Contenu | Validation |
|---|---|---|---|
| Release majeure | Toutes les {{DUREE_SPRINT}} semaines / fin de sprint | Évolutions du backlog | CAB + Go/No-Go si impact métier fort |
| Release mineure | Selon besoin | Correctifs, paramétrage | CAB |
| Hotfix | Urgence | Correctif P1 | PO + architecte, régularisation au CAB |

Fenêtre de déploiement en production : {{FENETRE_MEP}} (ex. mardi/jeudi 19 h–21 h), hors période de gel.

**Périodes de gel** :
- clôtures métier (fin de mois / de trimestre commercial…) ;
- 1 semaine avant et après une release majeure Salesforce sur l'instance de production ;
- {{AUTRES_GELS}}.

## 5. Releases Salesforce (plateforme)

Salesforce livre trois releases par an (Spring, Summer, Winter). Pour chacune :

| Quand | Action | Responsable |
|---|---|---|
| ~6 semaines avant | Lecture des release notes, identification des changements impactants et des retraits de fonctionnalités | Architecte + admin |
| Preview sandbox disponible | Exécution des tests de non-régression sur une sandbox en preview | QA |
| ~2 semaines avant | Revue des Release Updates (Setup > Release Updates) et plan d'activation | Admin |
| Après la release | Smoke tests en production, communication des nouveautés utiles | Admin + PO |

## 6. Traçabilité

Pour chaque release : note de version ([modèle](templates/release-note.md)), liste des US et des métadonnées déployées, résultat du déploiement (ID de déploiement), étapes manuelles pré et post-déploiement exécutées.
