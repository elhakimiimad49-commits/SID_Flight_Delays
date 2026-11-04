DICTIONNAIRE DES KPI

1. Objectif du document
Ce dictionnaire fixe une définition commune des indicateurs (KPI) à utiliser dans PostgreSQL et Power BI. Il précise pour chaque calcul les vols concernés, les colonnes nécessaires et les points à vérifier. Il ne présente aucun résultat statistique calculé.
2. Règles communes de comptage
- Vol programmé : une ligne correspondant à un vol dans flights.csv (à confirmer après contrôle des doublons réels).
- Vol annulé : CANCELLED = 1.
- Vol dérouté : DIVERTED = 1.
- Vol admissible à la ponctualité à l'arrivée : CANCELLED = 0, DIVERTED = 0 et ARRIVAL_DELAY non NULL. Ce choix facilite une comparaison cohérente ; il sera validé avec MORAD.
- Vol retardé à l'arrivée : vol admissible avec ARRIVAL_DELAY >= 15 minutes.
- Vol à l'heure à l'arrivée : vol admissible avec ARRIVAL_DELAY < 15 minutes. Une valeur négative signifie une arrivée avant l'heure prévue.
- NULL : absence d'information ; ne jamais convertir systématiquement en zéro.
Attention : les indicateurs de ponctualité portent sur un sous-ensemble des vols. Leurs dénominateurs sont donc différents de celui du taux d'annulation.
3. Tableau de synthèse des KPI
Code	KPI	Définition / formule	Unité
KPI-01	Total Flights	Nombre de vols programmés	vols

KPI-02	Cancelled Flights	Nombre de lignes avec CANCELLED = 1	vols

KPI-03	Diverted Flights	Nombre de lignes avec DIVERTED = 1	vols

KPI-04	Eligible Arrival Flights	Nombre de vols admissibles à la ponctualité à l'arrivée	vols

KPI-05	Delayed Arrival Flights	Vols admissibles avec ARRIVAL_DELAY >= 15	vols

KPI-06	On-Time Arrival Flights	Vols admissibles avec ARRIVAL_DELAY < 15	vols

KPI-07	Arrival Delay Rate	KPI-05 / KPI-04 × 100	%

KPI-08	On-Time Arrival Rate (OTP)	KPI-06 / KPI-04 × 100	%

KPI-09	Cancellation Rate	KPI-02 / KPI-01 × 100	%

KPI-10	Average Arrival Delay	Moyenne de ARRIVAL_DELAY sur les vols admissibles	minutes

KPI-11	Average Departure Delay	Moyenne de DEPARTURE_DELAY lorsque la valeur existe pour les vols non annulés	minutes

KPI-12	Average Delay of Delayed Flights	Moyenne de ARRIVAL_DELAY sur les vols du KPI-05	minutes

KPI-13	Diversion Rate	Diverted Flights / Total Flights × 100	%


Condition de calcul : si le dénominateur vaut zéro, afficher « N/A », jamais 0 % arbitrairement. Pour KPI-11, les vols déroutés disposant d'une valeur de départ restent inclus ; cette règle doit être documentée dans le rapport.
4. Exemple simple pour comprendre les formules
Supposons 100 vols programmés : 5 annulés, 2 déroutés, 3 autres avec ARRIVAL_DELAY manquant, et 90 vols admissibles. Parmi ces 90, 18 arrivent avec au moins 15 min de retard et 72 sont à l'heure.
- Arrival Delay Rate = 18 / 90 × 100 = 20 %
- On-Time Arrival Rate = 72 / 90 × 100 = 80 %
- Cancellation Rate = 5 / 100 × 100 = 5 %
Il s'agit d'un exemple pédagogique fictif, et non de résultats issus du dataset Kaggle.
5. Indicateurs et analyses complémentaires
5.1 Causes des annulations
Pour les vols annulés dont CANCELLATION_REASON est renseigné, analyser les catégories : A (Carrier), B (Weather), C (National Air System), D (Security). Présenter le nombre de vols par cause. Signaler séparément les codes manquants ou inconnus, s'il y en a.
5.2 Causes des retards
Les colonnes envisagées sont AIRLINE_DELAY, WEATHER_DELAY, AIR_SYSTEM_DELAY, SECURITY_DELAY et LATE_AIRCRAFT_DELAY. Leur somme par catégorie peut servir à comparer les minutes de retard attribuées aux causes, pour les enregistrements où les données sont renseignées. Ne pas interpréter une valeur NULL comme une absence de cause avant vérification ; ces colonnes ne sont pas nécessairement renseignées pour tous les vols.
5.3 Comparaisons
Analyser KPI-07, KPI-08 et KPI-09 par compagnie aérienne, mois et aéroport (départ ou arrivée selon la question). Toujours afficher le nombre de vols avec un pourcentage. Un seuil minimal de volume pour les classements pourra être choisi après examen du dataset.
6. Traçabilité : indicateurs et questions décisionnelles
Question métier	KPI / analyse principale	Colonnes attendues
Quelle est l'activité globale ?	KPI-01, KPI-02, KPI-03	CANCELLED, DIVERTED
Quel est le niveau de ponctualité ?	KPI-04 à KPI-08	ARRIVAL_DELAY, CANCELLED, DIVERTED
Quelles compagnies sont les plus ponctuelles ?	KPI-08 par compagnie	AIRLINE et colonnes de ponctualité
Quand observe-t-on plus de retards ?	KPI-07 par mois / heure	MONTH, SCHEDULED_DEPARTURE et colonnes de ponctualité
Quels aéroports sont les plus affectés ?	KPI-07 par aéroport	ORIGIN_AIRPORT, DESTINATION_AIRPORT et colonnes de ponctualité
Pourquoi les vols sont-ils annulés ?	Nombre d'annulations par cause	CANCELLATION_REASON, CANCELLED
Quelles causes contribuent aux retards ?	Minutes attribuées par cause	Colonnes de causes de retard



7. Critères d'acceptation
Le dictionnaire sera validé lorsque chaque KPI a un nom, une définition, une population de calcul, une formule, une unité, des colonnes sources et une règle de traitement des valeurs manquantes. Les règles en attente de vérification seront explicitement confirmées avec MORAD avant la mise en production des calculs.
8. Conclusion et suite
Ce document constitue le référentiel fonctionnel commun des KPI pour les membres du groupe.
