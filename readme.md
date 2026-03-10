# Superstore Data Pipeline & Analysis

## 📌 Project Overview

This project aims to build a data pipeline and analytical environment for the **Superstore dataset** using **PostgreSQL, Python, and SQLAlchemy**.
The goal is to structure the raw data, load it into a relational database, and create analytical views that allow exploration of **sales and profit performance**.

---

# 🗄️ 1. Database Connection

The project connects to a **PostgreSQL database** using SQLAlchemy.

Purpose of this step:

* Establish a connection between Python and PostgreSQL
* Verify that the database server is running
* Prepare the environment for database operations

---

# 🧱 2. Normalized Database Design

A normalized relational model was designed to reduce redundancy and improve data integrity.

The following tables were created:

* **customer** → stores customer information
* **location** → stores geographical data (country, city, region)
* **orders** → stores order information and references customers and locations
* **product** → stores product information
* **order_details** → stores transactional details (quantity, sales, cost)

Key design features:

* Primary Keys ensure unique records
* Foreign Keys maintain relationships between tables
* The `order_details` table resolves the **many-to-many relationship** between orders and products

---

# 📥 3. Data Loading

The dataset was imported from a CSV file using **Pandas**.

Main steps:

* Load the dataset using pandas
* Clean column names (lowercase, remove spaces)
* Split the dataset into multiple DataFrames
* Remove duplicate records
* Insert data into PostgreSQL tables using `to_sql()`

---

# 📊 4. Analytical Transformations

Several **SQL Views** were created to support analysis.

These views include:

* **sales_par_produit** → total sales by product
* **sales_par_categorie** → total sales by product category
* **sales_par_region** → total sales by region
* **profit_par_commande** → average profit per order
* **profit_par_client** → average profit per customer

Views allow simplified queries and are useful for BI dashboards.

---

# 📈 5. Data Analysis

The created views are queried from Python using Pandas to analyze results and prepare the data for visualization or dashboards.

---

# 🛠️ Technologies Used

* Python
* PostgreSQL
* SQLAlchemy
* Pandas
* SQL

---

# 📊 Future Improvements

Possible improvements for the project include:

* Building an interactive dashboard using **Streamlit**
* Adding advanced sales analytics
* Creating additional performance metrics
