# Checklist de mise en production – Release {{VERSION}}

Date de MEP : AAAA-MM-JJ · Release manager : … · Fenêtre : …

## J-5 : CAB

- [ ] Contenu de la release figé (liste des US) et note de version rédigée
- [ ] Toutes les US au statut Done, recette validée par le PO
- [ ] Déploiement validé en UAT ; validation (dry-run) en production réussie, ID de validation : …
- [ ] Tests Apex en production OK, couverture ≥ 75 % (org) / ≥ 85 % (classes du projet)
- [ ] Étapes manuelles pré / post-déploiement listées avec leur responsable
- [ ] Impacts sur les intégrations identifiés, équipes tierces prévenues
- [ ] Pas de conflit avec une période de gel ni une release Salesforce
- [ ] Plan de retour arrière défini

## J-1

- [ ] Communication aux utilisateurs envoyée (nouveautés, éventuelle interruption)
- [ ] Supports de formation / guides à jour
- [ ] Sauvegarde des données et métadonnées de production (outil : {{OUTIL_BACKUP}})
- [ ] Équipe disponible pendant la fenêtre et le lendemain matin

## Jour J

- [ ] Étapes manuelles pré-déploiement exécutées
- [ ] Jobs planifiés / intégrations suspendus si nécessaire
- [ ] Déploiement (quick deploy) lancé, ID : …
- [ ] Étapes manuelles post-déploiement exécutées (permission sets assignés, données de config, activation de Flows…)
- [ ] Jobs et intégrations réactivés
- [ ] Smoke tests réalisés par le PO / key users
- [ ] **Go / retour arrière** décidé par : …

## J+1

- [ ] Surveillance des erreurs (logs, emails d'exception Apex, erreurs de Flow, intégrations)
- [ ] Communication de fin de MEP
- [ ] Branche `main` taguée `v{{VERSION}}`, back-merge effectué
- [ ] Registre RAID et tableau de bord mis à jour
