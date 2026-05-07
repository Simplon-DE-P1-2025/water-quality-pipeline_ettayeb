# 💧 Pipeline Data Engineering — Qualité de l’Eau Potable

## 📌 Présentation du projet

Ce projet a pour objectif de construire un pipeline Data Engineering complet permettant :

- l’ingestion des données de qualité de l’eau potable depuis l’API Hub’Eau,
- la transformation et le nettoyage des données,
- la modélisation analytique en architecture Bronze / Silver / Gold,
- l’automatisation du pipeline avec Databricks Jobs,
- l’analyse des indicateurs de conformité de l’eau potable en France.

Le projet a été réalisé avec :

- Apache Spark (PySpark)
- Databricks
- Delta Lake
- GitHub
- API Hub’Eau

---

# 🏗️ Architecture du projet

```text
API Hub’Eau
     ↓
Bronze Layer (raw ingestion)
     ↓
Silver Layer (clean & transformation)
     ↓
Gold Layer (modèle analytique + KPI)
     ↓
Dashboard / Analyse
```

---

# 📂 Structure du repository

```text
water-quality-pipeline/
│
├── notebooks/
│   ├── 01_bronze_ingestion.py
│   ├── 02_silver_transformation.py
│   ├── 03_gold_modelisation.py
│   └── 04_run_pipeline.py
│
├── jobs/
│   └── water_quality_job.yml
│
├── README.md

```

---

# 📥 Source des données

Les données proviennent de l’API publique Hub’Eau :

API :
https://hubeau.eaufrance.fr/

Endpoint utilisé :

```text
/qualite_eau_potable/resultats_dis
```

Les données récupérées contiennent notamment :

- les résultats d’analyses,
- les paramètres chimiques,
- les informations de conformité,
- les communes,
- les installations,
- les réseaux de distribution,
- les limites réglementaires.

---

# 🥉 Bronze Layer — Ingestion

## 🎯 Objectif

Récupérer les données brutes depuis l’API et les stocker dans Delta Lake.

## ✅ Traitements réalisés

- récupération paginée des données,
- gestion des chunks,
- sauvegarde des données brutes,
- ajout de la date d’ingestion,
- stockage dans Delta Table.

## 📌 Table créée

```text
water_catalog.bronze.water_quality
```

---

# 🥈 Silver Layer — Transformation

## 🎯 Objectif

Nettoyer et standardiser les données.

## ✅ Transformations réalisées

### Nettoyage des types

- conversion string → double,
- remplacement des virgules par des points,
- suppression des caractères spéciaux (`<=`, `mg/L`, etc.).

### Gestion des valeurs nulles

- suppression des lignes critiques nulles,
- contrôle qualité.

### Standardisation

- création des colonnes :
  - `annee_prelevement`
  - `mois_prelevement`
  - `trimestre_prelevement`

### Conversion des dates

- conversion de `date_prelevement` en type `date`.

### Enrichissement

- création d’indicateurs :
  - conformité,
  - dépassement,
  - ratios de qualité.

## 📌 Table créée

```text
water_catalog.silver.water_quality_clean
```

---

# 🥇 Gold Layer — Modélisation analytique

## 🎯 Objectif

Construire un modèle analytique optimisé pour l’analyse et le reporting.

---

# 📊 Modèle analytique

## 📌 Dimensions

### `dim_commune`

Informations géographiques :

- commune,
- département,
- distributeur,
- UGE.

### `dim_parametre`

Informations sur les paramètres analysés :

- code paramètre,
- unité,
- type,
- libellé.

### `dim_temps`

Dimension temporelle :

- date,
- année,
- mois,
- trimestre.

### `dim_installation`

Informations techniques :

- installation amont,
- réseau,
- distributeur.

---

## 📌 Table de faits

### `fact_mesures`

Contient les analyses d’eau :

- résultats numériques,
- conformité,
- limites qualité,
- références qualité,
- dates,
- communes,
- paramètres.

---

# 📈 KPI & Agrégations

## `commune_kpi`

KPIs par commune :

- nombre d’analyses,
- taux de conformité,
- moyenne des résultats,
- nombre de dépassements,
- score qualité.

## Analyses possibles

- évolution temporelle,
- comparaison entre communes,
- suivi des non-conformités,
- tendances des contaminants,
- qualité par département.

---

# ⚙️ Orchestration du pipeline

Le pipeline est automatisé avec un Databricks Job.

## 📌 Workflow

```text
01_bronze_ingestion
        ↓
02_silver_transformation
        ↓
03_gold_modelisation
```

---

# 🚀 Exécution du pipeline

## Notebook principal

```python
dbutils.notebook.run("/Workspace/Brief_eau_69/01_bronze_ingestion", 0)

dbutils.notebook.run("/Workspace/Brief_eau_69/02_silver_transformation", 0)

dbutils.notebook.run("/Workspace/Brief_eau_69/03_gold_modelisation", 0)
```

---

# 📅 Job Databricks

Le job exporté est disponible dans :

```text
/jobs/water_quality_job.yml
```

Il permet :

- l’automatisation,
- la planification,
- l’exécution du pipeline complet.

---

# 🛠️ Technologies utilisées

| Technologie | Utilisation |
|---|---|
| Databricks | Développement pipeline |
| PySpark | Traitement Big Data |
| Delta Lake | Stockage |
| API Hub’Eau | Source des données |
| GitHub | Versionning |
| YAML | Configuration Job |

---

# 📌 Points techniques importants

## Optimisations réalisées

- pagination API,
- ingestion par chunks,
- Delta Lake,
- partitionnement,
- architecture medallion.

## Gestion qualité des données

- nettoyage avancé,
- cast sécurisés,
- suppression des nulls,
- standardisation des formats.

---

# 📊 Exemple d’analyses possibles

- communes les plus conformes,
- paramètres les plus problématiques,
- évolution mensuelle des contaminants,
- départements avec le plus de non-conformités,
- suivi réglementaire.

---

# 🔮 Améliorations futures

- dashboard Power BI,
- ingestion incrémentale,
- streaming temps réel,
- alertes automatiques,
- monitoring pipeline,
- CI/CD.

---

# 👨‍💻 Auteur

Mohammed Nour Eddine ETTAYEB

Projet Data Engineering — Databricks / PySpark / Delta Lake