Espace de travail pour IMADEDDINE - Pipeline ETL

# ZIAD — Power BI & Analyse décisionnelle

**Projet :** Flight Delay & Cancellation Analysis — SID (2015)  
**Membre :** Ziad  
**Branche conseillée :** `feature/ziad-dashboard`  
**Répertoire de travail :** `Team/05_ZIAD_PowerBI/`  
**Destination des livrables validés :** `dashboards/powerbi/ + dashboards/screenshots/`

> Guide de travail et checklist de livraison. Les dossiers/fichiers listés sont des **livrables à créer**, pas des résultats déjà réalisés.

## Objectif et responsabilités
Créer un Power BI decision dashboard fidèle au KPI Dictionary d'Imad. Proposer analyses pertinentes sans inventer de résultats, et valider chaque mesure contre SQL avec Ayoub.

## Outils
**1 :** Power BI Desktop, Power Query, DAX, PostgreSQL connector, Git/GitHub.

## Workflow opérationnel
1. Relire BQ, populations des KPI et Star Schema; préparer wireframe et relation entre pages.
2. Se connecter au PostgreSQL `dw` (données tests puis dataset complet après ETL); confirmer mode Import/DirectQuery selon volume et performances et documenter le choix.
3. Vérifier relations `fact_flight` → dimensions, direction de filtrage, tables dates, rôle Origin/Destination; éviter relations ambiguës.
4. Implémenter des **mesures DAX** (Total Flights, Delayed, Cancelled, Diverted, On-Time Rate, Delay Rate, Average Delay) avec dénominateurs approuvés; ne pas remplacer `BLANK` par zéro sans justification.
5. Créer 4 pages : **Overview**, **Airline Performance**, **Airport & Time**, **Cancellation & Delay Causes**, avec titres, unités, slicers, légendes, nombre de vols contextualisant les classements.
6. Tester des filtres par compagnie, période, origine/destination. Ajouter seuil de volume si décidé après profiling; différencier volume de vols et minutes de retard.
7. Effectuer des comparaisons SQL vs DAX pour 5 cas (global, compagnie, mois, cancelled, 1 aéroport) et documenter écarts.
8. Capturer 4 captures lisibles, raconter 3-5 insights **réels** avec contexte, rédiger chapitre BI / PPT et guide utilisateur.

## Structure livrables
```text
Team/05_ZIAD_PowerBI/
├── README.md
├── mockups/dashboard_wireframe.md
├── dax/kpi_measures.md
├── validation/sql_vs_dax.md
├── screenshots/overview.png
├── screenshots/airline.png
├── screenshots/airport_time.png
└── docs/dashboard_user_guide.md
```

Après revue, `.pbix` si taille raisonnable → `dashboards/powerbi/`; images → `dashboards/screenshots/`. Si fichier `.pbix` dépasse limites GitHub, utiliser stockage externe approuvé et fournir lien + checksum/version (pas pousser dans Git ordinaire).

## Points de contrôle
- [ ] Toutes les mesures correspondent au dictionnaire KPI
- [ ] Modèle et relations vérifiés sans ambiguïtés
- [ ] 4 pages lisibles + filtres testés
- [ ] SQL vs DAX rapprochés, différences justifiées
- [ ] Captures, guide, chapitre rapport, slides disponibles
- [ ] Aucun chiffre présenté sans extraction vérifiable

## Handoff
**Entrées :** Imad KPI, Rida DW, Ayoub PostgreSQL, Imadeddine chargement validé. **Sorties :** dashboard, DAX, analyses vérifiées, captures pour rapport et soutenance.


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
git switch -c feature/ziad-dashboard
```

Remplacer `OWNER/REPO` par le dépôt réel. Le propriétaire peut fournir le lien Clone HTTPS.

### B. Travailler et enregistrer
- Modifier seulement `Team/05_ZIAD_PowerBI/` et les **livrables validés** sous `dashboards/powerbi/ + dashboards/screenshots/`; ne pas toucher aux travaux des autres sans accord.
- Avant le commit : `git status` puis `git diff --stat` pour vérifier exactement les fichiers modifiés. Ajouter seulement les chemins pertinents.

```bash
git add Team/05_ZIAD_PowerBI/
git status
git commit -m "bi: build Power BI KPI dashboard and validation"
git push -u origin feature/ziad-dashboard
```

**Important :** pour publier un livrable déjà validé dans un dossier racine, il faut aussi l'ajouter explicitement (ex. `git add docs/requirements/`). Ne pas utiliser aveuglément `git add .` si le dépôt contient des données volumineuses ou privées.

### C. Pull Request / validation
1. Sur GitHub : ouvrir `Pull requests` → `New pull request` (ou `Compare & pull request`).
2. Choisir **base: `main`**, **compare: `feature/ziad-dashboard`**.
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

