02 — ANALYSE DES BESOINS


Ce document identifie les attentes des utilisateurs et les fonctionnalités attendues du système décisionnel. Il sert de référence à la conception du Data Warehouse, à la préparation des données et à la réalisation du tableau de bord. Il ne présente pas encore de résultats statistiques calculés sur le dataset.
2. Contexte et finalité
Les retards et annulations de vols perturbent l’activité aérienne. Les fichiers historiques de vols contiennent des informations utiles, mais leur lecture directe ne facilite pas les comparaisons entre compagnies, aéroports et périodes. Le projet vise à transformer ces données en indicateurs clairs et en visualisations interactives.

Question centrale : comment suivre la ponctualité et les perturbations des vols domestiques américains de 2015 afin de comparer les performances et d’identifier les tendances principales ?
3. Utilisateurs cibles (profils théoriques)
Profil	Besoin principal
Direction	Voir les indicateurs globaux et les tendances.
Responsable compagnie	Comparer les retards et annulations par compagnie.
Responsable aéroport	Observer les perturbations au départ et à l’arrivée.
Analyste	Filtrer et explorer les données pour expliquer les écarts.


Exemple : un responsable souhaite comparer les taux d’annulation de deux compagnies sur la même période, et pas seulement leurs nombres d’annulations.
4. Besoins décisionnels
Les besoins sont classés par thème afin d’aider les autres membres à construire un modèle de données cohérent.
Code	Besoin	Résultat attendu
BD-01	Suivi global	Nombre de vols, retards, annulations et déroutements.
BD-02	Ponctualité	Taux de ponctualité et taux de retard à l’arrivée.
BD-03	Comparaison des compagnies	Classement avec taux ET volume de vols.
BD-04	Analyse des aéroports	Identifier les aéroports les plus touchés.
BD-05	Analyse temporelle	Étudier les mois, saisons et créneaux horaires.
BD-06	Analyse des causes	Distinguer motifs d’annulation et catégories de retard.


5. Questions métier essentielles
- Quel est le volume total des vols enregistrés en 2015 ?
- Quel est le taux de ponctualité des vols évaluables ?
- Quelles compagnies présentent les taux de retard les plus élevés ?
- Quels aéroports concentrent le plus de perturbations ?
- À quelles périodes les retards sont-ils les plus fréquents ?
- Quels sont les principaux motifs d’annulation ?
- Comment les retards évoluent-ils d’un mois à l’autre ?
  Chaque question sera détaillée dans le fichier 03_business_questions.md. Les formules seront spécifiées dans 04_kpi_dictionary.md pour éviter les répétitions.
6. Principe de lecture des résultats
Une comparaison doit toujours préciser sa population de référence. Par exemple, le nombre de retards seul favorise les grandes compagnies : il doit être accompagné du taux de retard, du nombre de vols et, si nécessaire, d’un seuil minimal de volume à justifier.
7. Exigences fonctionnelles
Ces exigences décrivent ce que la solution doit permettre de faire. « Must » signifie indispensable ; « Should » signifie souhaitable si le temps le permet.
ID	Exigence	Priorité	Vérification
EF-01	Importer les fichiers sources de vols, compagnies et aéroports.	Must	Chargement reproductible.
EF-02	Vérifier et traiter les données selon les règles validées.	Must	Rapport de contrôles et rejets.
EF-03	Organiser les données dans un Data Warehouse PostgreSQL.	Must	Dimensions et table de faits.
EF-04	Afficher les indicateurs globaux dans Power BI.	Must	Cartes KPI fonctionnelles.
EF-05	Filtrer par mois, compagnie et aéroport.	Must	Filtres cohérents.
EF-06	Comparer retards et annulations par compagnie.	Must	Graphiques et volumes.
EF-07	Afficher l’évolution temporelle des perturbations.	Must	Graphiques temporels.
EF-08	Analyser les motifs d’annulation et causes de retard disponibles.	Must	Répartition par catégorie.
EF-09	Proposer des interactions et détails supplémentaires.	Should	Navigation/infobulles.
EF-10	Tester un scénario What-if descriptif.	Should	Simulation clairement indiquée.


8. Exemple de vérification
Pour EF-05, sélectionner une compagnie dans Power BI doit actualiser les indicateurs concernés sans changer leur règle de calcul. Les nombres affichés doivent être vérifiables par une requête SQL sur le même périmètre.

Attention : les opérations techniques détaillées (jobs Talend, scripts SQL, mesures DAX) relèvent des membres chargés de leur implémentation. Ici, nous définissons le résultat attendu.
9. Exigences non fonctionnelles
ID	Exigence	Attente concrète
ENF-01	Fiabilité	Même définition des KPI dans les documents, SQL et Power BI.
ENF-02	Qualité	Documenter les valeurs manquantes et les anomalies.
ENF-03	Performance	Tableaux de bord utilisables sur le volume prévu.
ENF-04	Traçabilité	Conserver la source, les règles de transformation et les contrôles.
ENF-05	Maintenabilité	Noms de tables et jobs compréhensibles, fichiers versionnés.
ENF-06	Lisibilité	Visuels légendés, filtres visibles et unités explicites.
ENF-07	Réexécution	Éviter les doublons lors d’un nouveau chargement.


10. Indicateurs attendus (aperçu)
Indicateur	Interprétation
Total Flights	Nombre de vols planifiés dans la source.
Delayed Flights	Vols arrivés avec au moins 15 minutes de retard.
On-Time Rate	Part de vols ponctuels parmi les vols éligibles.
Delay Rate	Part de vols retardés parmi les vols éligibles.
Cancellation Rate	Part des vols annulés parmi les vols planifiés.
Average Arrival Delay	Moyenne des valeurs de retard à l’arrivée admissibles.
Diverted Flights	Nombre de vols déroutés.


Règle importante : une valeur ARRIVAL_DELAY manquante ne signifie pas zéro minute de retard. Les vols annulés et déroutés doivent être examinés séparément. Les dénominateurs précis seront fixés dans le dictionnaire KPI puis validés sur les données.
11. Périmètre et limites
11.1 Inclus
- Vols domestiques américains de l’année 2015 présents dans le dataset choisi.
- Retards à l’arrivée et au départ, annulations, déroutements.
- Analyses par compagnie, aéroport, mois et autres dimensions disponibles.
- Chaîne Talend → PostgreSQL → Power BI.
11.2 Hors périmètre
- Prédiction automatique des retards par apprentissage automatique.
- Estimation des pertes financières ou de la satisfaction des passagers.
- Vols internationaux ou autres années non inclus dans les fichiers.
12. Sources et contraintes de données
Source envisagée : Kaggle « 2015 Flight Delays and Cancellations » (USDOT), comprenant flights.csv, airlines.csv et airports.csv. Les fichiers bruts sont conservés sans modification dans le Google Drive partagé.

Les colonnes nécessaires à chaque question métier seront proposées dans 03_business_questions.md. MORAD vérifiera les noms réels, les types, les valeurs manquantes, les doublons et les correspondances de références. Aucun pourcentage ou nombre d’anomalies n’est affirmé avant ce contrôle.
13. Validation des besoins
- Les utilisateurs cibles et les objectifs sont compris par l’équipe.
- Chaque besoin prioritaire est relié à une fonctionnalité vérifiable.
- Les KPI attendus sont listés sans incohérence avec les questions métier.
- Les limites du périmètre sont connues.
- Les points dépendant de la qualité des données sont identifiés « à confirmer ».
14. Coordination entre les membres
Membre	Utilisation de ce document
IMAD — Analyse	Valide besoins, questions métier, KPI et règles fonctionnelles.
RIDA — DW Model	Traduit les axes d’analyse en dimensions et table de faits.
MORAD — Data Prep	Vérifie faisabilité et qualité des colonnes nécessaires.
AYOUB — SQL	Implémente les tables et contrôles dans PostgreSQL.
ZIAD — Power BI	Réalise les visuels répondant aux besoins définis.
IMADEDDINE — ETL	Applique les transformations et règles validées dans Talend.


15. Livrable et passage à l’étape suivante
Ce document sert de spécification des besoins. La suite de la phase 1 détaillera les questions décisionnelles (03), les indicateurs de performance (04) et les règles métier (05). Les décisions dépendant de l’état réel du dataset seront révisées après le rapport de qualité des données fourni par MORAD.
16. Mini-guide pour la soutenance
Question probable	Réponse simple
Pourquoi analyser les besoins ?	Pour définir ce que les utilisateurs veulent savoir avant de concevoir la solution.
Quelle différence avec Data Profiling ?	Nous fixons les attentes ; MORAD inspecte la qualité réelle des fichiers.
Pourquoi des exigences fonctionnelles ?	Elles précisent ce que le système doit permettre de faire.
Comment vérifier que le besoin est satisfait ?	Avec un test précis : KPI, filtre, comparaison ou contrôle SQL.


Référence et statut du document
Source : https://www.kaggle.com/datasets/usdot/flight-delays

Statut : spécification initiale prête pour revue d’équipe. Les résultats chiffrés et règles dépendant de la qualité des données restent à confirmer.
