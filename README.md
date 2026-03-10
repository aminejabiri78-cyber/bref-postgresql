# 🏪 Superstore — Pipeline ETL & Analyse des Ventes

> Projet de modélisation, chargement et analyse de données de vente avec PostgreSQL et SQLAlchemy.

---

## 📋 Table des matières

- [Aperçu du projet](#aperçu-du-projet)
- [Architecture de la base de données](#architecture-de-la-base-de-données)
- [Prérequis](#prérequis)
- [Installation et configuration](#installation-et-configuration)
- [Étapes du pipeline ETL](#étapes-du-pipeline-etl)
- [Vues analytiques](#vues-analytiques)
- [Structure des fichiers](#structure-des-fichiers)

---

## 🔍 Aperçu du projet

Ce projet met en place un pipeline **ETL complet** (Extraction, Transformation, Chargement) pour les données de vente du dataset Superstore. Il comprend :

- La **conception d'un modèle de données normalisé** (schéma relationnel)
- Le **chargement des données** depuis un fichier CSV vers une base PostgreSQL
- La **création de vues analytiques** pour faciliter l'analyse des performances

---

## 🗄️ Architecture de la base de données

Le schéma est normalisé en **5 tables** interconnectées :

```
customer ──────────────── orders ────────────── order_details
  │                         │                        │
  │ (Customer_ID FK)         │ (Postal_Code FK)       │ (Product_ID FK)
  │                          ▼                        │
  │                       location                    ▼
  │                                               product
```

### Tables

| Table | Description | Clé primaire |
|---|---|---|
| `customer` | Informations clients | `Customer_ID` |
| `location` | Données géographiques | `Postal_Code` |
| `orders` | En-têtes de commandes | `Order_ID` |
| `product` | Catalogue produits | `Product_ID` |
| `order_details` | Lignes de commandes (ventes, coûts) | `(Order_ID, Product_ID)` |

---

## ⚙️ Prérequis

- **Python** 3.8+
- **PostgreSQL** 13+
- Packages Python :

```bash
pip install sqlalchemy pandas psycopg2-binary
```

---

## 🚀 Installation et configuration

### 1. Créer la base de données PostgreSQL

```sql
CREATE DATABASE superstore_db;
```

### 2. Configurer la connexion

Dans le script principal, modifiez l'URL de connexion si nécessaire :

```python
db_url = "postgresql://postgres:admin@localhost:5432/superstore_db"
engine = create_engine(db_url)
```

### 3. Préparer le fichier source

Placez le fichier `superstore_clean.csv` à la racine du projet. Il doit contenir les colonnes suivantes :

`customer_id`, `customer_name`, `segment`, `cat_client`, `postal_code`, `country`, `city`, `state`, `region`, `order_id`, `order_date`, `ship_date`, `ship_mode`, `product_id`, `product_name`, `category`, `sub_category`, `quantite`, `sales`, `cost`

---

## 📦 Étapes du pipeline ETL

### Étape 1 — Connexion à la base

Teste la connectivité avec PostgreSQL et affiche la version du moteur de base de données.

### Étape 2 — Création du schéma

Crée les 5 tables normalisées avec leurs clés primaires et clés étrangères.

### Étape 3 — Chargement des données

Lit le CSV avec `pandas`, extrait chaque sous-ensemble de données et charge chaque table via `to_sql()` en mode `append`.

```python
df = pd.read_csv("superstore_clean.csv")

customer_df.to_sql('customer', con=engine, if_exists='append', index=False)
location_df.to_sql('location', con=engine, if_exists='append', index=False)
orders_df.to_sql('orders',     con=engine, if_exists='append', index=False)
product_df.to_sql('product',   con=engine, if_exists='append', index=False)
order_details_df.to_sql('order_details', con=engine, if_exists='append', index=False)
```

### Étape 4 — Création des vues analytiques

Cinq vues SQL sont créées pour simplifier les requêtes d'analyse.

---

## 📊 Vues analytiques

| Vue | Description |
|---|---|
| `sales_par_produit` | Chiffre d'affaires total par produit |
| `sales_par_categorie` | Chiffre d'affaires total par catégorie |
| `sales_par_region` | Chiffre d'affaires total par région géographique |
| `profit_par_commande` | Profit moyen par commande (`Sales - cost`) |
| `profit_par_client` | Profit moyen par client |

### Exemple d'utilisation

```python
import pandas as pd

df_region = pd.read_sql("SELECT * FROM sales_par_region ORDER BY total_sales DESC", engine)
print(df_region)
```

---

## 📁 Structure des fichiers

```
superstore-etl/
│
├── superstore_clean.csv       # Données source (non versionné)
├── bref-postgre.ipynb         # Script principal ETL
└── README.md                  # Documentation du projet
```

---

## 📝 Notes

- Les doublons sont supprimés à l'aide de `drop_duplicates()` avant le chargement de chaque table.
- Les colonnes du DataFrame sont normalisées automatiquement (minuscules, espaces et tirets remplacés par des underscores).
- Le profit est calculé dynamiquement dans les vues comme `Sales - cost` (pas de colonne stockée).

---

*Projet réalisé avec SQLAlchemy, pandas et PostgreSQL.*
