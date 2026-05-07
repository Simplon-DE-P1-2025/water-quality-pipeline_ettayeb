# 💧 Water Quality Data Pipeline

## 📌 Description

Projet Data Engineering permettant de collecter, transformer et analyser les données de qualité de l’eau potable depuis l’API HubEau.

Le projet suit une architecture Medallion :

- Bronze → données brutes
- Silver → données nettoyées
- Gold → données analytiques

Le pipeline est développé avec Databricks, PySpark et Delta Lake.

---

# 🏗️ Architecture du projet

```text
API HubEau
     ↓
Bronze Layer (Raw Data)
     ↓
Silver Layer (Cleaned Data)
     ↓
Gold Layer (Analytics & KPIs)

⚙️ Stack Technique
Python
PySpark
Databricks
Delta Lake
GitHub
API HubEau

🌍 Source des données

Données provenant de l’API publique HubEau :

https://hubeau.eaufrance.fr

API utilisée :

https://hubeau.eaufrance.fr/api/v1/qualite_eau_potable/resultats_dis

🥉 Bronze Layer — Ingestion

Le notebook Bronze permet :

l’extraction des données depuis l’API HubEau
la gestion de la pagination API
le stockage brut des données
l’écriture en format Delta

Fonctionnalités :

ingestion par pages
limitation du nombre de pages
sauvegarde par chunks
gestion des erreurs API

Table créée :

water_catalog.bronze.water_quality

🥈 Silver Layer — Transformation

Le notebook Silver réalise le nettoyage et l’enrichissement des données.

Transformations effectuées :

suppression des colonnes inutiles
suppression des lignes nulles
correction des types
conversion String → Double
extraction des valeurs numériques
traitement des valeurs comme :
<=50 mg/L
6,5
enrichissement temporel

Colonnes temporelles créées :

annee_prelevement
mois_prelevement
trimestre_prelevement

Gestion qualité :

conformité
limites qualité
références qualité

Table créée :

water_catalog.silver.water_quality_clean

🥇 Gold Layer — Modélisation Analytique

Le notebook Gold construit un modèle analytique orienté BI et reporting.

Dimensions
dim_commune

Informations géographiques :

commune
département
distributeur
UGE
dim_parametre

Informations analytiques :

paramètre
unité
type paramètre
dim_temps

Dimension temporelle :

date
mois
trimestre
année
dim_installation

Informations réseau et installation.

📊 Tables de faits

fact_mesures

Table centrale contenant :

résultats d’analyses
conformité
seuils qualité
références
dates

📈 KPIs et Agrégats

Le projet calcule plusieurs indicateurs :

taux de conformité
nombre d’analyses
communes les plus conformes
communes les moins conformes
analyses par département
tendances temporelles
paramètres critiques
dépassements qualité

Table KPI :

water_catalog.gold.commune_kpi

🚀 Orchestration Pipeline

Le pipeline complet est orchestré avec :

dbutils.notebook.run()

Ordre d’exécution :

Bronze ingestion
Silver transformation
Gold modelisation

📂 Structure du projet

water-quality-pipeline/
│
├── notebooks/
│   ├── bronze_ingestion
│   ├── silver_transfo
│   ├── gold_modelisation
│   └── run_pipeline
│
├── screenshots/
│
├── README.md
│
└── requirements.txt

▶️ Exécution

1. Lancer Bronze
dbutils.notebook.run("bronze_ingestion", 0)
2. Lancer Silver
dbutils.notebook.run("silver_transfo", 0)
3. Lancer Gold
dbutils.notebook.run("gold_modelisation", 0)
4. Pipeline complet
dbutils.notebook.run("run_pipeline", 0)

📌 Fonctionnalités Data Engineering

Ce projet met en pratique :

ingestion API REST
architecture Medallion
Delta Lake
PySpark transformations
nettoyage de données
gestion des types
modélisation analytique
orchestration pipeline
Git/GitHub
Databricks

👨‍💻 Auteur

Mohammed Nour Eddine ETTAYEB

Projet réalisé dans le cadre d’un apprentissage Data Engineering.