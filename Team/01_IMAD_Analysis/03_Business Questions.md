 Business Questions

1. Objectif du document
Ce document transforme les besoins décisionnels du projet Flight Delay & Cancellation Analysis en questions précises à traiter dans le Data Warehouse et le tableau de bord Power BI. Il ne présente pas de résultats calculés : ces résultats seront obtenus après le traitement et la validation des données.
2. Contexte de l’étude
Source prévue : jeu de données Kaggle « 2015 Flight Delays and Cancellations », constitué notamment des fichiers flights.csv, airlines.csv et airports.csv. La période étudiée est l’année 2015 et le périmètre annoncé concerne les vols domestiques américains.
3. Méthode de lecture
Pour chaque question, nous précisons l’objectif, les indicateurs, les colonnes attendues dans flights.csv, le type d’analyse et un visuel Power BI envisagé. Les noms des colonnes sont des hypothèses de travail à confirmer par MORAD lors de la vérification du dataset.
4. Catalogue des questions décisionnelles
BQ-01 — Combien de vols ont été programmés en 2015 ?
Objectif : Mesurer le volume global d’activité.
KPI / Mesure : Total Flights
Colonnes attendues : YEAR, MONTH, DAY
Analyse : Descriptive
Visualisation proposée : Carte KPI + courbe mensuelle
Disponibilité des données : À confirmer par MORAD.
BQ-02 — Quel est le taux de ponctualité à l’arrivée ?
Objectif : Évaluer la ponctualité globale.
KPI / Mesure : On-Time Rate (%)
Colonnes attendues : ARRIVAL_DELAY, CANCELLED, DIVERTED
Analyse : Descriptive
Visualisation proposée : Carte KPI + tendance mensuelle
Disponibilité des données : À confirmer par MORAD.
BQ-03 — Quelles compagnies ont les taux de retard les plus élevés ?
Objectif : Comparer les performances avec un classement équitable.
KPI / Mesure : Delay Rate (%), Eligible Flights
Colonnes attendues : AIRLINE, ARRIVAL_DELAY, CANCELLED, DIVERTED
Analyse : Comparative
Visualisation proposée : Barres horizontales + nombre de vols
Disponibilité des données : À confirmer par MORAD.
BQ-04 — Quelles compagnies présentent les meilleures performances ?
Objectif : Identifier les compagnies les plus ponctuelles.
KPI / Mesure : On-Time Rate (%), Eligible Flights
Colonnes attendues : AIRLINE, ARRIVAL_DELAY, CANCELLED, DIVERTED
Analyse : Comparative
Visualisation proposée : Classement des compagnies
Disponibilité des données : À confirmer par MORAD.
BQ-05 — Quels aéroports sont les plus affectés par les retards ?
Objectif : Détecter les aéroports à surveiller.
KPI / Mesure : Delay Rate (%), Average Arrival Delay
Colonnes attendues : ORIGIN_AIRPORT, DESTINATION_AIRPORT, ARRIVAL_DELAY, CANCELLED, DIVERTED
Analyse : Géographique / comparative
Visualisation proposée : Barres par aéroport + filtres origine/destination
Disponibilité des données : À confirmer par MORAD.
BQ-06 — Comment la ponctualité évolue-t-elle selon les mois ?
Objectif : Repérer des tendances saisonnières.
KPI / Mesure : On-Time Rate (%), Average Arrival Delay
Colonnes attendues : YEAR, MONTH, ARRIVAL_DELAY, CANCELLED, DIVERTED
Analyse : Temporelle
Visualisation proposée : Courbe par mois
Disponibilité des données : À confirmer par MORAD.
BQ-07 — Quelles sont les principales causes des retards ?
Objectif : Évaluer les contributions aux minutes de retard signalées.
KPI / Mesure : Delay Minutes by Cause
Colonnes attendues : AIRLINE_DELAY, WEATHER_DELAY, AIR_SYSTEM_DELAY, SECURITY_DELAY, LATE_AIRCRAFT_DELAY
Analyse : Causale descriptive
Visualisation proposée : Barres par cause
Disponibilité des données : À confirmer par MORAD.
BQ-08 — Quelles sont les raisons les plus fréquentes d’annulation ?
Objectif : Comprendre la répartition des annulations.
KPI / Mesure : Cancelled Flights by Reason
Colonnes attendues : CANCELLED, CANCELLATION_REASON
Analyse : Catégorielle
Visualisation proposée : Barres A/B/C/D
Disponibilité des données : À confirmer par MORAD.
BQ-09 — À quels horaires de départ les retards sont-ils les plus fréquents ?
Objectif : Étudier les créneaux horaires problématiques.
KPI / Mesure : Delay Rate (%) by scheduled hour
Colonnes attendues : SCHEDULED_DEPARTURE, ARRIVAL_DELAY, CANCELLED, DIVERTED
Analyse : Temporelle / comparative
Visualisation proposée : Histogramme par tranche horaire
Disponibilité des données : À confirmer par MORAD.
BQ-10 — Quelle est la répartition des vols annulés et déroutés ?
Objectif : Suivre les perturbations d’exploitation.
KPI / Mesure : Cancellation Rate (%), Diversion Rate (%)
Colonnes attendues : CANCELLED, DIVERTED, YEAR, MONTH
Analyse : Descriptive / temporelle
Visualisation proposée : Cartes KPI + barres mensuelles
Disponibilité des données : À confirmer par MORAD.
5. Principes communs de calcul
- Un vol à l’arrivée est considéré comme retardé lorsque ARRIVAL_DELAY ≥ 15 minutes.
- Pour les taux de ponctualité et de retard à l’arrivée, la population de référence proposée comprend les vols non annulés, non déroutés et disposant d’une valeur ARRIVAL_DELAY valide ; cette règle sera validée avec l’équipe.
- Cancellation Rate = nombre de vols annulés / nombre de vols programmés × 100. Diversion Rate = nombre de vols déroutés / nombre de vols programmés × 100.
- Une arrivée en avance peut avoir un retard négatif ; elle ne doit pas être considérée comme une valeur invalide par défaut.
- Pour les classements d’aéroports et de compagnies, afficher également le volume de vols admissibles ; un seuil minimal pourra être défini après examen du volume des groupes.
- Les colonnes de causes de retard ne sont pas nécessairement renseignées pour tous les vols : les comparaisons par cause doivent afficher clairement leur population et leur unité (minutes), sans assimiler les valeurs manquantes à zéro automatiquement.

6. Critères de validation
- Les dix Business Questions sont formulées clairement, sans ambiguïté.
- Chaque question possède un objectif, au moins un indicateur, des colonnes attendues et une visualisation envisagée.
- Les KPI communs ont une définition cohérente avec le dictionnaire KPI (Étape 04).
- Les colonnes et traitements nécessitant vérification restent marqués « à confirmer par MORAD ».
- Le document est relu par les responsables du Data Warehouse et du dashboard avant validation.


Remarque : aucune statistique réelle sur les vols n’est affirmée dans ce document.
