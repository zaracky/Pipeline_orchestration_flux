# Pipeline_orchestration_flux
Ce projet permet d'orchestrer un pipeline de traitement de données pour un cas d'utilisation spécifique autour des données de vins, en utilisant Kestra et des services AWS S3 pour la gestion des fichiers. Il comprend plusieurs étapes allant de l'extraction des données à la génération de rapports et d'analyses avancées.

## Description du workflow
Ce pipeline extrait des fichiers depuis un bucket AWS S3 (ERP, Web, et Liaison), les nettoie et les joint, puis effectue diverses analyses et tests, notamment sur les doublons, les valeurs manquantes, et la cohérence des données. Il génère des rapports sous forme de fichiers Excel et CSV, qui sont ensuite téléchargés sur S3.

Le pipeline est conçu pour effectuer les étapes suivantes :

- Téléchargement des fichiers depuis S3 : Récupère les fichiers ERP, Web, et Liaison.)
- Traitement des données dans DuckDB : Nettoie et prépare les données avant de réaliser des tests.
- Tests sur les données : Vérifie l'absence de doublons, les valeurs manquantes, et la cohérence des jointures.
- Fusion des différentes tables pour créer un ensemble de données complet
- Calcul du revenu total pour chaque produit
- Identification des vins premium sur la base du prix
- Génération de rapports : Sauvegarde les résultats dans des fichiers Excel et CSV
- Téléchargement des résultats sur S3 : Télécharge les fichiers générés vers un autre bucket S3.

## Prérequis
 - Kestra : Vous devez avoir Kestra installé et configuré.
 - Plugins Kestra : Ce workflow utilise les plugins suivants :
      - io.kestra.plugin.scripts.python.Script pour les scripts Python.
      - io.kestra.plugin.scripts.sql.duckdb.Script pour les scripts SQL exécutés via DuckDB.
 - DuckDB : Pour les requêtes SQL sur les données.

 - Openpyxl, Pandas : Bibliothèques Python nécessaires à l'analyse des fichiers Excel.

## Installation
1. Clonez ce repository dans votre environnement Kestra.
2. Ajoutez ce workflow à votre orchestration Kestra.
3. Assurez-vous que les fichiers CSV nécessaires sont accessibles au moment de l'exécution (ou modifiez les chemins de fichier dans le script).

      
## Fonctionnement du Pipeline
### Variables d'Environnement
Certaines variables peuvent être configurées directement dans le fichier .env. 

### Structure des Fichiers
Le pipeline travaille avec les fichiers suivants :

    - Fichier ERP : Contient les informations produits.
    - Fichier Web : Contient les informations des produits depuis un site web.
    - Fichier Liaison : Contient les correspondances entre les produits ERP et Web.
Tous ces fichiers sont stockés dans le bucket S3 défini par la variable S3_BUCKET_NAME.

### Orchestration
1. Téléchargement des fichiers depuis S3 : Les fichiers sont récupérés du bucket S3 en utilisant les identifiants AWS.

2. Nettoyage des données avec DuckDB : Les fichiers sont traités pour éliminer les doublons et valeurs manquantes.

3. Tests de qualité des données : Des tests de qualité des données sont effectués (absence de doublons, valeurs manquantes, cohérence des jointures).

4. Calcul du chiffre d'affaires total : Un calcul du chiffre d'affaires total basé sur les données jointes est effectué.

5. Export des résultats : Les résultats des analyses sont sauvegardés sous forme de fichiers Excel et CSV.

6. Téléchargement des rapports sur S3 : Les fichiers générés sont téléchargés dans le bucket S3.


### Accéder aux Fichiers Résultats
Les fichiers générés seront disponibles dans le bucket S3 spécifié, par exemple :

Rapport CA : BottleNeck/rapport_CA.xlsx

Extraction Premium CSV : BottleNeck/extraction_premium.csv

Extraction Ordinaire CSV : BottleNeck/extraction_ordinaire.csv

Extraction Rapport Test : BottleNeck/debug/rapport_test.xlsx

### Exemple de résultat
- revenue_report.xlsx : Un fichier Excel contenant le total des revenus par produit.
- premium_wines.csv : Un fichier CSV avec les vins identifiés comme premium.
- ordinary_wines.csv : Un fichier CSV avec les vins ordinaires.
  
## Planification
Le pipeline est planifié pour s'exécuter automatiquement selon une planification Cron. L'exemple suivant exécute le pipeline tous les 15 du mois à 9h00 :
    `triggers:
      - id: planification
        type: io.kestra.plugin.core.trigger.Schedule
        cron: 0 9 15 * *`

## Conclusion
Ce projet fournit un pipeline automatisé de traitement des données avec des tests de qualité, idéal pour les cas où la gestion de la donnée est essentielle, notamment pour des cas comme l'analyse des ventes de vins. Utilisez ce pipeline pour obtenir des rapports réguliers et fiables sur vos données.

## Contributions
Si vous souhaitez contribuer à ce projet, n'hésitez pas à soumettre une pull request.




