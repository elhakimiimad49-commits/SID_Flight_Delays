# AYOUB — PostgreSQL, DDL & SQL Validation

**Projet :** Flight Delay & Cancellation Analysis — SID (2015)  
**Membre :** Ayoub  
**Branche conseillée :** `feature/ayoub-postgresql`  
**Répertoire de travail :** `Team/04_AYOUB_SQL/`  
**Destination des livrables validés :** `sql/ddl/ + sql/queries/ + sql/validation/`

> Guide de travail et checklist de livraison. Les dossiers/fichiers listés sont des **livrables à créer**, pas des résultats déjà réalisés.

## Objectif et responsabilités
Installer/configurer PostgreSQL et créer le DW physique selon schéma approuvé par Rida, en concertation avec Imadeddine. Garantir contraintes, performances de base, droits et SQL de vérification. **Ayoub ne remplace pas Talend** pour les transformations du pipeline.

## Outils
**1** PostgreSQL, pgAdmin , éditeur SQL, Git/GitHub. *

## Workflow opérationnel
1. Installer une version compatible PostgreSQL; noter version, port, nom DB local (sans mot de passe dans le dépôt); expliquer procédure d'installation.
2. Créer schemas `staging` et `dw` par scripts SQL idempotents lorsque possible (`CREATE SCHEMA IF NOT EXISTS`).
3. Avec Rida, valider définition des dimensions et de la fact : types, PK, FK, NULL, indexes et ordres de création.
4. Construire DDL versionné et exécutable dans un ordre documenté; prévoir `staging` pour CSV et tables `dw` pour analyse. Ne pas mettre des chemins Windows absolus privés dans les scripts livrés.
5. Fournir user/permissions adaptés au besoin (credentials dans environnement local), partager à Imadeddine host/port/db/schema/nom d'utilisateur via canal privé et **jamais le password sur Git**.
6. Tester chargements dimensions puis fact, FK, cardinalités et cas « unknown airport »; ne pas désactiver durablement les contraintes pour charger.
7. SQL de contrôle : row counts source→staging→dw, doublons métiers, checks FK, statuts, agrégations de KPI; documenter écarts dus aux rejets.
8. Optimiser seulement si mesure nécessaire (index, plan SQL), noter durée des requêtes; transmettre schéma DW à Ziad pour Power BI.
9. Mettre SQL et preuves d'exécution dans le rapport + diapositives.

## Structure livrables
```text
Team/04_AYOUB_SQL/
├── README.md
├── ddl/01_create_schemas.sql
├── ddl/02_create_staging.sql
├── ddl/03_create_dimensions.sql
├── ddl/04_create_fact.sql
├── queries/kpi_checks.sql
├── validation/integrity_checks.sql
└── docs/setup_postgresql.md
```

Fichiers validés → `sql/ddl/`, `sql/queries/`, `sql/validation/`. **Ne pas versionner** un dump DB géant ou des fichiers `pgpass`.

## Exemples de contrôles (adapter noms réels)
```sql
SELECT COUNT(*) FROM dw.fact_flight;
SELECT COUNT(*) FROM dw.fact_flight WHERE cancelled = 1;
-- Vérifier les clés manquantes après confirmation des vrais noms de colonnes.
```

## Critères d'acceptation
- [ ] DDL exécutable depuis une base propre et documenté
- [ ] Schemas/tables/PK/FK conformes au modèle validé
- [ ] Connexion Talend testée avec Imadeddine
- [ ] SQL validation + résultat réel daté
- [ ] Aucun identifiant de connexion ni dump confidentiel commité
- [ ] Tables de DW lisibles par Power BI

## Handoff
**Entrées :** star schema Rida, règles data Morad. **Sorties :** DDL + description des connexions (sans secrets) à Imadeddine, schéma/queries à Ziad, chiffres de validation à toute l'équipe.


## Règles communes d’équipe

- `Team/<votre dossier>/` = **espace de travail** (brouillons, tests, preuves). Les dossiers racine (`docs/`, `sql/`, `talend_jobs/`, `dashboards/`, `scripts/`) = **livrables validés**. `main` désigne **la branche Git**, pas un dossier.
- Pas de modification directe de `main`. Une branche par tâche; Pull Request (PR), revue, puis merge. Les autres membres peuvent travailler en parallèle avec des interfaces convenues.
- Ne jamais publier les identifiants, mots de passe, tokens, `.env`, sauvegardes DB, logs sensibles ou données de connexion personnelles. Partager des modèles sans secret (`.env.example`, `db.example`).
- Ne pas versionner les énormes CSV (`data/raw/`, `data/cleaned/`), ni un dump PostgreSQL volumineux. Un `data/sample/` non sensible et petit est possible; conserver URL, licence et méthode de récupération du dataset.
- Tout résultat doit être **reproductible** : version d'outil, entrées, commande/job/notebook, règles, résultats attendus et preuves. Ne pas déclarer un résultat réel sans mesure.
- Les vols annulés et déroutés ainsi que les `NULL` ne doivent pas devenir automatiquement des vols « à l'heure ». Définir explicitement le dénominateur de chaque KPI; les codes aéroport doivent être confrontés au référentiel sans supprimer aveuglément les non-correspondances.
- Livrable « terminé » = fichier(s) présents, tests consignés, documentation à jour, PR relue et approuvée.


## Guide GitHub — étape par étape

### A. Première installation (une seule fois)
1. Demander l'accès **Collaborator** au propriétaire du dépôt, accepter l'invitation GitHub, installer **Git**.
2. Ouvrir le terminal (Git Bash ou terminal de VS Code) et exécuter :

```bash
git clone https://github.com/OWNER/REPO.git
cd REPO
git switch main
git pull --ff-only origin main
git switch -c feature/ayoub-postgresql
```

Remplacer `OWNER/REPO` par le dépôt réel. Le propriétaire peut fournir le lien Clone HTTPS.

### B. Travailler et enregistrer
- Modifier seulement `Team/04_AYOUB_SQL/` et les **livrables validés** sous `sql/ddl/ + sql/queries/ + sql/validation/`; ne pas toucher aux travaux des autres sans accord.
- Avant le commit : `git status` puis `git diff --stat` pour vérifier exactement les fichiers modifiés. Ajouter seulement les chemins pertinents.

```bash
git add Team/04_AYOUB_SQL/
git status
git commit -m "sql: implement PostgreSQL warehouse and validation"
git push -u origin feature/ayoub-postgresql
```

**Important :** pour publier un livrable déjà validé dans un dossier racine, il faut aussi l'ajouter explicitement (ex. `git add docs/requirements/`). Ne pas utiliser aveuglément `git add .` si le dépôt contient des données volumineuses ou privées.

### C. Pull Request / validation
1. Sur GitHub : ouvrir `Pull requests` → `New pull request` (ou `Compare & pull request`).
2. Choisir **base: `main`**, **compare: `feature/ayoub-postgresql`**.
3. Décrire les changements, fichiers, tests et points à valider; demander au moins 1 revue par un membre concerné.
4. Corriger les commentaires, pousser de nouveaux commits sur la même branche, puis fusionner seulement après validation.
5. Pour la tâche suivante, repartir d'un `main` actualisé avec **une nouvelle branche**.

```bash
git switch main
git pull --ff-only origin main
git switch -c feature/nom-nouvelle-tache
```

### D. Alternative sans terminal (GitHub web)
Sur la page du dépôt, sélectionner la branche de travail (la créer depuis `main`), naviguer au bon chemin → `Add file` → `Upload files` → choisir les fichiers → message de commit → commit **sur votre branche**, puis créer une Pull Request vers `main`. Cette méthode convient aux Markdown et petits fichiers, pas aux gros datasets ni aux projets Talend complexes.

### E. Erreurs courantes
- `Permission denied` : vérifier invitation Collaborator et authentification GitHub.
- `non-fast-forward` : la branche distante a avancé; récupérer les modifications et résoudre les conflits plutôt que forcer un push.
- Fichier absent : Git n'enregistre pas un dossier vide; ajouter `.gitkeep` si nécessaire.
- PR en conflit : prévenir les autres; résoudre soigneusement, jamais écraser le travail d'autrui par `--force` sur branche partagée.

Espace de travail pour AYOUB - Base de données SQL
