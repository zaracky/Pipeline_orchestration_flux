# Pipeline_orchestration_flux
Ce workflow Kestra est conçu pour le traitement des données liées aux vins, en se basant sur des fichiers Excel (ERP, Web, Liaison) pour effectuer des analyses et générer des rapports sur les vins premium et ordinaires.

## Description du workflow
Le workflow "Wine Data Processing" réalise plusieurs étapes de traitement de données liées aux ventes de vins, notamment :

- Chargement des données depuis des fichiers CSV (ERP, Web, Liaison)
- Nettoyage des données pour supprimer les valeurs nulles
- Suppression des doublons dans les données
- Fusion des différentes tables pour créer un ensemble de données complet
- Calcul du revenu total pour chaque produit
- Identification des vins premium sur la base du prix
- Export des rapports générés en fichiers CSV et Excel
- Envoi des rapports par email aux destinataires spécifiés
## Structure du workflow
Le workflow est constitué des étapes suivantes :

### 1. Trigger mensuel (15 du mois à 9h) :
- Le workflow est exécuté automatiquement chaque 15 du mois à 9h.

### 2. Chargement des données (load_data) :
- Les données sont chargées depuis des fichiers CSV (erp.xlsx, web.xlsx, liaison.xlsx) dans une base de données temporaire en mémoire.

### 3. Nettoyage des données (clean_data) :
- Les lignes où l'ID est null sont supprimées dans chaque table (ERP, Web, Liaison).

### 4. Suppression des doublons (deduplicate_data) :
- Les doublons sont supprimés dans les tables ERP, Web et Liaison pour garantir que les données sont uniques.

### 5. Fusion des données (join_data) :
- Les tables ERP, Web et Liaison sont jointes pour créer une table complète (merged_data) avec les informations des trois sources.

### 6. Calcul des revenus (calculate_revenue) :
- Un calcul des revenus est effectué en multipliant le prix par la quantité vendue, et les revenus totaux sont également calculés.

### 7. Identification des vins premium (identify_premium_wines) :
- Les vins sont classés en fonction de leur prix avec un seuil défini par le score Z (z-score > 2 pour les vins premium). Les vins premium et ordinaires sont exportés en fichiers CSV distincts.

### 8. Export des rapports (export_reports) :
- Trois fichiers sont générés :
    - revenue_report.xlsx : Rapport des revenus par produit
    - premium_wines.csv : Liste des vins premium
    - ordinary_wines.csv : Liste des vins ordinaires

### 9. Envoi des rapports par email (send_reports) :
- Les rapports sont envoyés par email aux destinataires spécifiés, avec les fichiers attachés.

## Prérequis
### - Kestra : Vous devez avoir Kestra installé et configuré.
### - Plugins Kestra : Ce workflow utilise les plugins suivants :
      - io.kestra.plugin.scripts.python.Script pour les scripts Python.
      - io.kestra.plugin.scripts.sql.duckdb.Script pour les scripts SQL exécutés via DuckDB.
      - io.kestra.plugin.notifications.email.Sendgrid pour l'envoi d'emails.

## Configuration
### Déclencheur
Le workflow est configuré pour se déclencher automatiquement tous les 15 du mois à 9h via le cron suivant :

0 9 15 * *

### Envoi d'email
Les rapports sont envoyés à deux adresses emails :
  - xxx@gmail.com
  - xxx@gmail.com
    
Vous pouvez modifier ces adresses dans le fichier de configuration du workflow selon vos besoins.

### Installation
1. Clonez ce repository dans votre environnement Kestra.
2. Ajoutez ce workflow à votre orchestration Kestra.
3. Assurez-vous que les fichiers CSV nécessaires sont accessibles au moment de l'exécution (ou modifiez les chemins de fichier dans le script).

### Exemple de résultat
- revenue_report.xlsx : Un fichier Excel contenant le total des revenus par produit.
- premium_wines.csv : Un fichier CSV avec les vins identifiés comme premium.
- ordinary_wines.csv : Un fichier CSV avec les vins ordinaires.
