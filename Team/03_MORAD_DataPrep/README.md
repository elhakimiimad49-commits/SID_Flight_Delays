Espace de travail pour MORAD - Nettoyage des données
# MORAD — Data Profiling & Data Quality

**Projet :** Flight Delay & Cancellation Analysis — SID (2015)  
**Membre :** Morad  
**Branche conseillée :** `feature/morad-data-quality`  
**Répertoire de travail :** `Team/03_MORAD_DataPrep/`  
**Destination des livrables validés :** `docs/data-quality/ + scripts/profiling/`

> Guide de travail et checklist de livraison. Les dossiers/fichiers listés sont des **livrables à créer**, pas des résultats déjà réalisés.

## Objectif et responsabilités
**Morad analyse et définit les règles**, il ne remplace pas le pipeline Talend d'Imadeddine. Identifier la structure, les anomalies, leur fréquence et les résultats attendus de nettoyage de manière reproductible.

## Outils
**1:** Python 3 + pandas +  VS Code, Git/GitHub.
## Workflow opérationnel
1. Obtenir les 3 CSV , taille, encodage, séparateur, colonnes, type apparent et volume.
2. **Profiling complet / échantillon maîtrisé :** calculer compte de lignes, taux de NULL par colonne, types, valeurs uniques, statistiques, dates, distributions. Pour `flights.csv` volumineux, utiliser chunks (`pd.read_csv(..., chunksize=...)`) plutôt que charger en RAM sans contrôle.
3. **Doublons :** comparer duplications exactes et doublons métier possibles; définir clé métier candidate avec Rida; ne pas supprimer deux vols juste parce qu'ils ont le même numéro.
4. **Référentiels :** contrôler compagnies et aéroports inconnus dans `airlines.csv`/`airports.csv` en distinguant codes numériques et codes textuels; quantifier les non correspondances; ne pas effectuer des mappings imaginaires.
5. **Cohérence temporelle :** `SCHEDULED_DEPARTURE` peut être HHMM numérique (ex. 5 → 00:05); vérifier 0, valeurs hors plage et valeurs spéciales, passage de minuit. Ne pas traiter directement 1230 comme `12:30` sans parsing explicite.
6. **Règles d'activité :** NULL sur `ARRIVAL_DELAY`, `CANCELLED`, `DIVERTED`; retards négatifs peuvent signifier avance et ne sont pas forcément erronés. Définir traitement spécifique par catégorie.
7. **Rapport qualité :** quantifier chaque anomalie (nombre et %, impact KPI, priorité), proposer transformation (conserver, exclure du KPI, joindre à unknown, rejeter avec log, normaliser).
8. **Cas de test :** construire petit dataset de démonstration avec entrée et résultat attendu pour vol normal, retardé ≥15, cancel, divert, airport inconnu, NULL, HHMM. Faire relire à Imad/Imadeddine.
9. Publier notebook reproductible, `requirements.txt` minimal ou instructions d'environnement, screenshots, rapport et section du rapport final.

## Structure livrables
```text
Team/03_MORAD_DataPrep/
├── README.md
├── notebooks/data_profiling.ipynb
├── reports/dataset_inventory.md
├── reports/data_quality_report.md
├── reports/cleaning_rules.md
├── samples/validation_cases.csv
├── samples/expected_results.md
└── environment/requirements.txt
```

Notebook et scripts validés → `scripts/profiling/`. Rapport de qualité → `docs/data-quality/`. Un mini sample sans données sensibles peut être partagé dans `data/sample/`.

## Exemple de code (adapter aux chemins et types réels)
```python
import pandas as pd
p = 'data/raw/flights.csv'
missing_counts = None
total_rows = 0
for chunk in pd.read_csv(p, chunksize=100_000, low_memory=False):
    total_rows += len(chunk)
    counts = chunk.isna().sum()
    missing_counts = counts if missing_counts is None else missing_counts.add(counts, fill_value=0)
print('Rows:', total_rows)
print('Missing %:', (missing_counts / total_rows * 100).round(2))
```

## Critères d'acceptation
- [ ] Inventaire réel des sources + métriques, pas chiffres repris d'un autre projet
- [ ] Rapport NULL/duplicates/refs/horaire et distribution
- [ ] Chaque règle a justification + exemple + impact KPI
- [ ] Cas de test et résultats attendus fournis
- [ ] Notebook relançable et sans chemin absolu privé
- [ ] Imadeddine et Rida ont confirmé le handoff

## Handoff
**À Imadeddine :** `cleaning_rules.md`, `validation_cases.csv`, `expected_results.md`. **À Ayoub/Rida :** risques types / PK / FK / unknown codes. **À Imad/Ziad :** règles d'éligibilité aux KPI.


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
git switch -c feature/morad-data-quality
```

Remplacer `OWNER/REPO` par le dépôt réel. Le propriétaire peut fournir le lien Clone HTTPS.

### B. Travailler et enregistrer
- Modifier seulement `Team/03_MORAD_DataPrep/` et les **livrables validés** sous `docs/data-quality/ + scripts/profiling/`; ne pas toucher aux travaux des autres sans accord.
- Avant le commit : `git status` puis `git diff --stat` pour vérifier exactement les fichiers modifiés. Ajouter seulement les chemins pertinents.

```bash
git add Team/03_MORAD_DataPrep/
git status
git commit -m "quality: document source profiling and cleaning rules"
git push -u origin feature/morad-data-quality
```

**Important :** pour publier un livrable déjà validé dans un dossier racine, il faut aussi l'ajouter explicitement (ex. `git add docs/requirements/`). Ne pas utiliser aveuglément `git add .` si le dépôt contient des données volumineuses ou privées.

### C. Pull Request / validation
1. Sur GitHub : ouvrir `Pull requests` → `New pull request` (ou `Compare & pull request`).
2. Choisir **base: `main`**, **compare: `feature/morad-data-quality`**.
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

