IMADEDDINE — ETL Talend & Intégration des données
Projet : Flight Delay & Cancellation Analysis — SID (2015)
Mmbre : Imadeddine
Répertoire de travail : Team/06_IMADEDDINE_ETL/
1. Objectif et responsabilités
Concevoir et réaliser un processus ETL (Extract, Transform, Load) avec Talend afin d'intégrer les données des vols dans un Data Warehouse PostgreSQL.
Le pipeline doit être reproductible, respecter les règles de nettoyage validées et permettre le calcul correct des KPI définis par IMAD.
Responsabilités principales :
Extraire les données des fichiers CSV.
Contrôler et transformer les données.
Appliquer les Cleaning Rules validées.
Charger les dimensions et la table de faits dans PostgreSQL.
Gérer les erreurs et produire des logs.
Vérifier les résultats du chargement.
2. Outils
Talend Open Studio / Talend Studio disponible — ETL.
PostgreSQL — Data Warehouse.
pgAdmin — Vérification des données.
GitHub / Google Drive — Partage des livrables.
3. Documents nécessaires avant l'intégration
Responsable
Documents nécessaires
IMAD
04_kpi_dictionary.md et 05_business_rules.md
MORAD
cleaning_rules.md, validation_cases.csv et expected_results.md
RIDA
star_schema.drawio, table_dictionary.md et source_to_target_mapping.md
AYOUB
Scripts DDL PostgreSQL et informations de connexion sécurisées


4. Workflow opérationnel
Étape 1 — Préparation de l'environnement
Installer et configurer Talend.
Créer un projet Talend pour Flight Delay Analysis.
Vérifier l'accès aux trois fichiers :
flights.csv
airlines.csv
airports.csv
Configurer une connexion PostgreSQL.
Tester la lecture d'un petit échantillon CSV.
Livrable : environnement Talend fonctionnel et test de connexion documenté.
Étape 2 — Extraction des données
Créer les Metadata CSV dans Talend.
Vérifier les noms et les types des colonnes.
Créer un Job d'extraction avec tFileInputDelimited.
Tester la lecture des données.
Prévoir le traitement adapté au volume important de flights.csv.
Livrable : Job d'extraction testé.
Étape 3 — Transformation et nettoyage
Appliquer uniquement les règles validées avec MORAD et IMAD :
Traitement des valeurs NULL.
Vérification des indicateurs CANCELLED et DIVERTED.
Conservation correcte des retards négatifs valides.
Contrôle des dates et horaires.
Gestion des références aéroport ou compagnie inconnues.
Traitement des doublons selon la stratégie validée.
Préservation des données nécessaires au calcul des KPI.
Attention : une valeur ARRIVAL_DELAY = NULL ne doit pas être remplacée automatiquement par zéro.
Livrable : Jobs de transformation et règles appliquées documentées.
Étape 4 — Chargement des dimensions
Après validation du Star Schema et création des tables par AYOUB :
Charger dim_airline.
Charger dim_airport.
Charger dim_date.
Charger les autres dimensions éventuellement validées.
Vérifier les clés et les références inconnues.
Livrable : dimensions correctement alimentées dans PostgreSQL.
Étape 5 — Chargement de la table de faits
Lire les données transformées.
Réaliser les lookups vers les dimensions.
Récupérer les clés des dimensions.
Charger fact_flight.
Contrôler les erreurs de chargement.
Vérifier les volumes et éviter les doublons lors des réexécutions.
Livrable : table de faits chargée et contrôlée.
Étape 6 — Tests et validation
Effectuer les contrôles suivants :
Comparer les volumes source et destination.
Vérifier les lignes rejetées ou exclues.
Contrôler l'intégrité référentielle.
Tester les cas préparés par MORAD.
Vérifier la cohérence des vols annulés, déroutés et retardés.
Comparer les résultats de certains KPI avec les requêtes SQL d'AYOUB.
Vérifier qu'une réexécution ne produit pas de duplications inattendues.
Livrable : rapport de validation ETL.
5. Structure des livrables
Team/06_IMADEDDINE_ETL/
├── README.md
├── jobs/
│   ├── extraction/
│   ├── transformation/
│   ├── load_dimensions/
│   └── load_facts/
├── configuration/
│   └── environment_example.md
├── tests/
│   └── etl_validation.md
├── screenshots/
└── reports/
    └── etl_execution_report.md
6. Critères d'acceptation
Projet Talend créé et fonctionnel.
Extraction des trois sources CSV testée.
Connexion PostgreSQL vérifiée.
Cleaning Rules validées appliquées.
Dimensions chargées correctement.
Table de faits chargée correctement.
Logs et erreurs documentés.
Tests de qualité et de cohérence réussis.
Résultats du chargement validés avec AYOUB.
Jobs et documentation partagés avec l'équipe.
.

