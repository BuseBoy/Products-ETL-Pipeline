# ETL Products Data Pipeline


<div align="center">
  
  [![Python](https://img.shields.io/badge/Python-3.13.2-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org)
  [![Pandas](https://img.shields.io/badge/Pandas-2.3.1-150458?style=flat&logo=pandas&logoColor=white)](https://pandas.pydata.org)
  [![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-2.0.28-009688?style=flat)](https://www.sqlalchemy.org)
  [![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16.1-336791?style=flat&logo=postgresql&logoColor=white)](https://www.postgresql.org)
  [![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat&logo=jupyter&logoColor=white)](https://jupyter.org)
  [![Logging](https://img.shields.io/badge/Logging-Standard-yellow?style=flat)](https://docs.python.org/3/library/logging.html)
  [![JSON](https://img.shields.io/badge/JSON-Standard-blue?style=flat)](https://docs.python.org/3/library/json.html)
 [![Postman](https://img.shields.io/badge/Postman-Standard-green?style=flat)](https://docs.python.org/3/library/json.html)
  [![Status](https://img.shields.io/badge/Status-In%20Progress-yellow?style=flat&logo=progress&logoColor=white)](https://github.com)
  [![GitHub](https://img.shields.io/badge/GitHub-Repository-181717?style=flat&logo=github&logoColor=white)](https://github.com)
  
</div>

---

## 📌 Project Overview

This project builds a **robust ETL pipeline** for processing **product JSON data** and loading it into a **PostgreSQL database**.  

**Objectives:**

- Extract product data from multiple JSON files  
- Transform data: clean, normalize, validate, calculate `price_with_discount`  
- Load data into PostgreSQL using **staging-merge pattern**  
- Log all steps and errors for traceability  

---

## 🛠️ Technologies & Libraries Used

- **Python Standard Libraries:**  
  - `json` → Read & parse JSON files  
  - `logging` → Log pipeline activities and errors  
- **Data Handling:**  
  - `pandas` → Data manipulation, explode nested reviews, cleaning  
- **Database & ORM:**  
  - `sqlalchemy` → PostgreSQL connection, transaction management  
- **Database Engine:**  
  - PostgreSQL → Storing clean product data  
- **Notebook:**  
  - Jupyter Notebook → Analysis & documentation  

---

## 🔧 Key Steps

### 1️⃣ Extraction

- Reads multiple JSON files from directory  
- Handles missing files & invalid JSON gracefully  
- Extracts specific key (`products`) if exists, else full JSON  
- Combines all into a single **raw DataFrame**  

### 2️⃣ Transformation

- Explodes nested `reviews` into separate rows  
- Extracts `review_rating`  
- Drops missing essential fields (`id`, `title`, `price`, `review_rating`)  
- Validates prices (`>0`) and discounts (`0–100%`)  
- Calculates `price_with_discount = price * (1 - discount/100)`  
- Converts data types and reorders columns  

### 3️⃣ Loading

- Loads data into **staging table** first  
- Performs **UPSERT** into main table (insert new, update existing)  
- Uses transaction-managed approach with automatic rollback on error  
- Optionally drops staging table after successful merge  

---

## 📂 Dataset

- **Data:** https://dummyjson.com/products API link
- **Fields:** `id`, `title`, `category`, `price`, `discountPercentage`, `rating`, `brand`, `reviews`  

---

## 📊 Example Insights

- Detects negative prices or invalid discounts  
- Normalizes nested reviews into rows  
- Calculates discounted prices for analytics  

---

## 🚀 How to Run

```bash
git clone https://github.com/YourUsername/ETL-Products-Pipeline.git
cd ETL-Products-Pipeline
pip install pandas sqlalchemy psycopg2-binary
jupyter notebook etl_pipeline.ipynb
