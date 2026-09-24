# 10 – Gestion des risques

## 1. Principes

- **Un seul registre** : tous les risques, actions, problèmes et décisions sont tenus dans le [registre RAID](templates/raid-log.csv). Aucun risque n'est suivi uniquement dans un CR ou un e-mail.
- **Un responsable par risque** : une personne nommée (pas une équipe), chargée du plan de réponse et de la mise à jour du registre.
- **Tout le monde peut remonter un risque** : via le CP ou directement en COPROJ ; l'identification n'est pas réservée au chef de projet.
- **Risque ≠ problème** : un risque est incertain et futur ; un problème (issue) est avéré. Un risque qui se réalise est clos au statut « Réalisé » et donne lieu à une issue liée.
- **Risque accepté = décision** : accepter un risque est une décision tracée (type « D »), prise par le niveau correspondant à sa criticité.

## 2. Processus

| Étape | Quand | Qui | Résultat |
|---|---|---|---|
| 1. Identifier | Atelier de lancement, fin de cadrage, COPROJ, rétrospectives, ADR, change requests | Toute l'équipe | Risque inscrit au RAID (statut « Ouvert ») |
| 2. Évaluer | Sous 1 semaine (prochain COPROJ) | CP + responsable | Probabilité, impact, criticité, catégorie, déclencheur |
| 3. Traiter | Selon la criticité (§ 4) | Responsable | Stratégie, plan de mitigation, plan de contingence si criticité ≥ 8 |
| 4. Suivre | COPROJ hebdomadaire | CP | Criticité et tendance mises à jour, top 5 présenté en COPIL |
| 5. Clore | Risque disparu, réalisé ou accepté | Instance du § 4 | Statut « Clos » ou « Réalisé », date de clôture renseignée |

Le CP anime un atelier d'identification au lancement et en fin de cadrage, en s'appuyant sur le [catalogue Salesforce](#7-catalogue-de-risques-salesforce).

## 3. Échelles d'évaluation

### Probabilité

| Note | Libellé | Repère |
|---|---|---|
| 1 | Rare | < 10 % |
| 2 | Possible | 10 à 40 % |
| 3 | Probable | 40 à 70 % |
| 4 | Quasi certain | > 70 % |

### Impact

On retient l'axe le plus défavorable.

| Note | Libellé | Planning | Budget | Périmètre / métier | Sécurité / conformité |
|---|---|---|---|---|---|
| 1 | Faible | < 1 semaine, sans effet sur les jalons | < 2 % | Gêne mineure, contournement simple | Aucun |
| 2 | Modéré | 1 sprint | 2 à 5 % | Fonctionnalité secondaire dégradée ou reportée | Écart mineur corrigeable en interne |
| 3 | Majeur | 1 sprint à 1 mois, jalon intermédiaire décalé | 5 à 10 % | Fonctionnalité MVP dégradée, adoption menacée | Non-conformité à corriger avant go-live |
| 4 | Critique | > 1 mois ou go-live {{DATE_GOLIVE}} compromis | > 10 % | Objectif de la [charte](01-charte-projet.md) non atteint | Fuite de données, violation RGPD, incident de sécurité |

Les seuils à 10 % sont alignés sur le [circuit d'escalade](02-organisation-raci.md#3-circuit-descalade).

### Matrice de criticité (P × I)

| Probabilité ↓ / Impact → | 1 | 2 | 3 | 4 |
|---|---|---|---|---|
| **4** | 🟠 4 | 🟠 8 | 🔴 12 | 🔴 16 |
| **3** | 🟢 3 | 🟠 6 | 🟠 9 | 🔴 12 |
| **2** | 🟢 2 | 🟠 4 | 🟠 6 | 🟠 8 |
| **1** | 🟢 1 | 🟢 2 | 🟢 3 | 🟠 4 |

## 4. Seuils de criticité et escalade

| Criticité | Niveau | Instance de suivi | Exigences |
|---|---|---|---|
| 12 – 16 | 🔴 Critique | COPIL (exceptionnel si besoin) | Sponsor informé sous 48 h ; plans de mitigation **et** de contingence ; revue hebdomadaire ; seul le COPIL peut l'accepter |
| 8 – 9 | 🟠 Élevé | COPROJ, présenté en COPIL | Plans de mitigation et de contingence ; revue hebdomadaire |
| 4 – 6 | 🟠 Modéré | COPROJ | Plan de mitigation ; revue toutes les 2 semaines |
| 1 – 3 | 🟢 Faible | COPROJ | Surveillance ; revue mensuelle |

Tout impact noté 4 est traité comme un risque élevé, quelle que soit sa probabilité. Les risques techniques (architecture, performance, dette) sont instruits en [Design Authority](04-design-authority.md) avant d'être arbitrés.

## 5. Stratégies de réponse

| Stratégie | Principe | Exemple Salesforce |
|---|---|---|
| **Éviter** | Supprimer la cause, souvent en changeant le périmètre ou la solution | Remplacer un développement spécifique par une fonctionnalité standard |
| **Réduire** | Baisser la probabilité ou l'impact | POC de volumétrie en sandbox Full avant de valider le modèle de données |
| **Transférer** | Confier le risque à un tiers (contrat, éditeur, assurance) | SLA et pénalités dans le contrat d'un package AppExchange |
| **Accepter** | Ne rien faire de plus, en connaissance de cause | Accepter une limite standard documentée dans un ADR |

- Le **plan de mitigation** agit avant la survenue du risque ; le **plan de contingence** décrit ce qu'on fait s'il survient, avec son **déclencheur** (le signal qui l'active).
- La **criticité résiduelle** est la criticité visée une fois la mitigation réalisée. Si elle reste ≥ 12, le risque est présenté en COPIL pour acceptation ou changement de stratégie.
- Les actions de mitigation sont inscrites au RAID (type « A ») ou au backlog, et chiffrées.

## 6. Provision pour risques

- Montant : {{PROVISION_RISQUES}} % du budget du projet, inscrit dans la [charte](01-charte-projet.md#5-budget-et-ressources).
- Engagement : le CP engage la provision pour une mitigation ou une contingence jusqu'à 10 % de dérive (niveau 2 d'escalade) ; au-delà, décision du COPIL.
- Suivi : consommation de la provision présentée à chaque COPIL (météo « Budget » du [CR](templates/cr-copil.md)).
- Toute consommation est tracée au RAID (type « D ») avec le risque concerné.

## 7. Catalogue de risques Salesforce

Liste d'amorçage pour l'atelier d'identification. Retenir uniquement les risques pertinents et les chiffrer.

| Catégorie | Risque | Signal d'alerte | Réponse type |
|---|---|---|---|
| Plateforme | Release Salesforce (Spring, Summer, Winter) proche d'un jalon | Go-live ou recette à moins de 2 semaines d'une release | Tests de régression en sandbox preview, décaler la date ([06](06-environnements-release.md)) |
| Plateforme | Dépassement des governor limits (CPU, SOQL, DML) | Erreurs `LimitException` en recette, tests de charge absents | Revue de code, patterns bulkifiés, tests avec volumes réels ([05](05-standards-dev.md)) |
| Plateforme | Fonctionnalité retirée ou non disponible dans l'édition | Dépendance à une fonctionnalité en fin de vie (Process Builder, Workflow Rules) ou en bêta | Vérifier la roadmap et l'édition en cadrage, ADR |
| Données | Volumétrie (LDV) sous-estimée | > {{SEUIL_VOLUMETRIE}} enregistrements sur un objet, data skew sur un compte ou un propriétaire | POC de volumétrie, index, archivage, stratégie de partage adaptée |
| Données | Qualité des données sources | Taux de doublons ou de champs obligatoires vides lors du profilage | Audit en cadrage, nettoyage avant reprise, répétitions de reprise ([08](08-securite-donnees.md)) |
| Données | Stockage insuffisant | Stockage > {{SEUIL_STOCKAGE}} % | Politique d'archivage, achat de stockage prévu au budget |
| Intégrations | Indisponibilité ou retard du système tiers | Environnement de test tiers indisponible, contrat d'interface non figé | Mocks, contrat d'interface signé en cadrage, jalon d'intégration dédié |
| Intégrations | Dépassement des limites d'API | Consommation API journalière > 70 % | Appels groupés (Bulk / Composite), revue des flux, surveillance |
| Sécurité | Modèle de partage inadapté ou trop permissif | OWD « Public » par défaut, profils avec « Modify All Data » | Revue en Design Authority, Health Check ≥ {{SCORE_HEALTH_CHECK}} |
| Sécurité | Non-conformité RGPD | Données personnelles sans base légale ni durée de conservation | Validation DPO en cadrage, registre des traitements |
| Licences | Licences insuffisantes ou mal dimensionnées | Nombre d'utilisateurs ou type de licence non confirmés | Inventaire en cadrage, validation avec le compte Salesforce avant commande |
| Tiers | Dépendance à un package AppExchange | Éditeur peu établi, package non « Security Reviewed », pas de SLA | Due diligence, clause contractuelle, plan de sortie |
| Architecture | Dette déclarative ou technique | Nombre de Flows par objet, violations Code Analyzer en hausse | Standards, revue en Design Authority, capacité réservée à la dette ([09](09-run.md)) |
| Release | Déploiement en échec ou dérive entre environnements | Modifications directes en production, échec de validation | Git comme source de vérité, validation (dry-run) en production, plan de retour arrière |
| Organisation | Indisponibilité des key users | Ateliers ou sessions de recette annulés | Créneaux réservés dès le lancement, engagement des managers |
| Organisation | Dépendance à une personne clé | Connaissance d'un sujet portée par une seule personne | Binômage, documentation, revue de code croisée |
| Adoption | Faible adoption à la mise en production | Faible participation aux formations, retours négatifs en recette | Plan de conduite du changement, réseau de key users, suivi d'adoption |

## 8. Revues aux jalons

| Jalon | Revue des risques |
|---|---|
| Lancement | Atelier d'identification ; 3 à 5 risques majeurs reportés dans la [charte](01-charte-projet.md#6-risques-majeurs-initiaux) |
| Fin de cadrage | Nouvel atelier avec l'architecture cible et le catalogue ; provision confirmée |
| Chaque release (CAB) | Risques propres à la release et plan de retour arrière ([checklist MEP](templates/checklist-mep.md)) |
| Go / No-Go | Aucun risque critique ouvert sans plan de contingence validé ni acceptation formelle du COPIL |
| Fin d'hypercare | Risques résiduels transférés au registre RUN avec un nouveau responsable ([09](09-run.md)), risques projet clos |

## 9. Indicateurs

Présentés en COPIL avec le top 5 des risques :
- nombre de risques ouverts par niveau de criticité et tendance ;
- risques sans mise à jour depuis plus de 2 semaines ;
- actions de mitigation en retard ;
- risques réalisés depuis le dernier COPIL ;
- provision consommée / provision totale.
