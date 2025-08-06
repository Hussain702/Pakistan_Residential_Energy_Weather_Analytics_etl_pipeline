# Pakistan Residential Energy & Weather Analytics ETL Pipeline

This project is an end-to-end ETL pipeline that automates the extraction, transformation, and loading (ETL) of **energy consumption** and **weather data** for Islamabad city into a **dedicated SQL pool** in **Azure Synapse Analytics**, designed using the **star schema** model.

---

## 🌐 Data Sources

- 📦 [LUMS REWDP Dataset](https://web.lums.edu.pk/~eig/gallery/datasets/rewdp/rewdp_dataset.zip)
- 🌦️ [LUMS Weather Dataset](https://web.lums.edu.pk/~eig/gallery/datasets/rewdp/weather_dataset.zip)

---

## ⚙️ Technologies Used

- **Apache Airflow** (with Astro CLI)
- **Python 3.12**
- **Azure Data Lake (ADLS Gen2)**
- **Azure Synapse Analytics (Dedicated SQL Pool)**
- **ODBC Hook** (for loading)
- **Pandas** (for transformation)
- **Airflow Providers**:
  - `microsoft.azure`
  - `odbc`

---

## 📊 Data Warehouse Design

The data is stored using a **Star Schema** with the following structure:

### Fact Table
- `fact_energy_usage`
  - `d_id` (FK to `dim_date`)
  - `city_id` (FK to `dim_city`)
  - Energy usage metrics
  - Weather metrics

### Dimension Tables
- `dim_date`
  - `full_date`, `year`, `month`, `day`, `day_of_week`, `week_of_year`
- `dim_city`
  - `city_name`

---

## 🧠 Airflow DAG: `pakistan_energy_weather_etl`

This DAG performs:

### 1. **Extract Task**
- Downloads energy and weather ZIP datasets from HTTP endpoints.
- Uploads ZIP files to **Azure Data Lake** (`raw` container).

### 2. **Transform Task**
- Reads and filters Islamabad CSV files from the ZIPs.
- Cleans, merges, and renames columns.
- Combines datasets on `datetime` and `City`.
- Uploads the final cleaned CSV to `Transformed-data/Islamabad.csv` in Data Lake.

### 3. **Load Task**
- Uses `ODBC` to connect with Azure Synapse.
- Executes `COPY INTO` to load the transformed CSV into the staging table.

---


