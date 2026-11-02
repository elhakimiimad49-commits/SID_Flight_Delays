 Espace de travail pour RIDA - Modélisation du DW 
 
# RIDA — Modélisation dimensionnelle / Data Warehouse

**Projet :** Flight Delay & Cancellation Analysis — SID (2015)  
**Membre :** Rida  
**Branche conseillée :** `feature/rida-star-schema`  
**Répertoire de travail :** `Team/02_RIDA_DW_Model/`  
**Destination des livrables validés :** `docs/design/ + sql/ddl/`

> Guide de travail et checklist de livraison. Les dossiers/fichiers listés sont des **livrables à créer**, pas des résultats déjà réalisés.

## Objectif et responsabilités
Concevoir le modèle dimensionnel du Data Warehouse (schéma en étoile) cohérent avec les KPI. **Grain central : une ligne par enregistrement de vol source identifié**; préciser la clé métier et la stratégie de déduplication avec Morad.

## Outils
**Principaux :** diagrams.net (draw.io), éditeur Markdown, PostgreSQL/pgAdmin pour prototypage, Git/GitHub.

## Workflow opérationnel
1. Lire BQ/KPI et dictionnaire de colonnes; recenser granularité, mesures additives/non additives et axes d'analyse.
2. Définir le **grain** de `fact_flight`; justifier l'identité d'un vol (ne pas supposer que FLIGHT_NUMBER seul est unique).
3. Dessiner dimensions `dim_date`, `dim_airline`, `dim_airport` (rôles origin et destination), `dim_cancellation_reason` **si nécessaire**; définir clés substituts ou codes naturels selon choix explicite.
4. Définir mesures et drapeaux dans `fact_flight`: retards départ/arrivée, cancel, divert, causes, clés, champs de filtre. Ne pas multiplier des dimensions sans utilité.
5. Identifier le traitement des références inconnues (unknown member ou table de rejets), notamment codes aéroport numériques vs lookup; ne pas inventer de mapping.
6. Rédiger modèle logique : colonnes, types proposés, PK, FK, nullable, index envisagés; coordonner DDL réel avec Ayoub.
7. Vérifier le modèle contre chaque BQ et les besoins Power BI; vérifier que les causes des retards restent exploitables et ne sont pas confondues avec les annulations.
8. Finaliser diagrammes, data dictionary et chapitre « Modélisation » du rapport.

## Livrables
```text
Team/02_RIDA_DW_Model/
├── README.md
├── diagrams/star_schema.drawio
├── diagrams/star_schema.png
├── specifications/grain_and_keys.md
├── specifications/table_dictionary.md
└── reviews/business_question_coverage.md
```

Publier validé sous `docs/design/`; coordonner scripts DDL validés dans `sql/ddl/` avec Ayoub.

## Vérifications à effectuer
- [ ] Un seul grain clairement défini pour `fact_flight`
- [ ] Schéma exploitable par toutes les Business Questions
- [ ] Clés Airport Origin/Destination distinctes pointent vers la même dimension de référence (ou dimensions rôles documentées)
- [ ] Stratégie unknown codes et NULL formalisée
- [ ] Choix clés, types et cardinalités validé par Ayoub
- [ ] Diagramme, dictionnaire et chapitre rapport disponibles

## Handoff
**Entrées :** besoins Imad + profiling Morad. **Sorties :** mapping de conception à Ayoub et Imadeddine, modèle Power BI à Ziad. Les changements de colonnes après DDL doivent passer par revue de l'équipe.


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
git switch -c feature/rida-star-schema
```

Remplacer `OWNER/REPO` par le dépôt réel. Le propriétaire peut fournir le lien Clone HTTPS.

### B. Travailler et enregistrer
- Modifier seulement `Team/02_RIDA_DW_Model/` et les **livrables validés** sous `docs/design/ + sql/ddl/`; ne pas toucher aux travaux des autres sans accord.
- Avant le commit : `git status` puis `git diff --stat` pour vérifier exactement les fichiers modifiés. Ajouter seulement les chemins pertinents.

```bash
git add Team/02_RIDA_DW_Model/
git status
git commit -m "docs: design dimensional warehouse schema"
git push -u origin feature/rida-star-schema
```

**Important :** pour publier un livrable déjà validé dans un dossier racine, il faut aussi l'ajouter explicitement (ex. `git add docs/requirements/`). Ne pas utiliser aveuglément `git add .` si le dépôt contient des données volumineuses ou privées.

### C. Pull Request / validation
1. Sur GitHub : ouvrir `Pull requests` → `New pull request` (ou `Compare & pull request`).
2. Choisir **base: `main`**, **compare: `feature/rida-star-schema`**.
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

