# Earthquake Data Lakehouse & Analytics

An end-to-end data pipeline built on **Microsoft Fabric** that ingests, transforms, and visualizes global earthquake data from the USGS Earthquake API using the **Medallion (Bronze–Silver–Gold) architecture**.

## Architecture


- **Bronze** — Fetches raw earthquake event data (GeoJSON) from the USGS API for a given date range and lands it in the lakehouse Files section.
- **Silver** — Reads and flattens the raw JSON, extracts coordinates/magnitude/time fields, handles nulls, and casts timestamps into a clean Delta table (`earthquake_events_silver`).
- **Gold** — Enriches the silver data with reverse-geocoded **country codes** (via `reverse-geocoder`) and a derived **significance class** (Low/Moderate/High based on the `sig` score), writing to `earthquake_events_gold`.
- **Power BI** — `Main_Earthquake_report.pbix` visualizes the gold table (event locations, magnitude trends, significance distribution).

## Tech Stack

- Microsoft Fabric (Lakehouse, Notebooks, Data Pipelines)
- PySpark
- Power BI
- SQL (Lakehouse/Warehouse SQL project)
- Python libraries: `requests`, `reverse-geocoder`

## Project Structure

| File | Description |
|---|---|
| `Notebook/Bronze_notebook.ipynb` | Pulls raw earthquake GeoJSON from USGS API |
| `Notebook/Silver_notebook.ipynb` | Cleans, reshapes, and type-casts raw data |
| `Notebook/Gold_notebook.ipynb` | Adds country code + significance classification |
| `SQL/earthquake_lakehouse.sqlproj` | SQL project for the lakehouse schema |
| `Data/Parameter.csv` | Pipeline run parameters (e.g., date ranges) |
| `Dashboard/Main_Earthquake_report.pbix` | Power BI dashboard on the gold table |

## How It Works

1. A Fabric Data Pipeline passes `start_date` / `end_date` parameters into the Bronze notebook.
2. Bronze notebook calls the USGS API and saves raw JSON to the lakehouse.
3. Silver notebook parses the JSON, validates/nulls-handles key fields, and appends to a Delta table.
4. Gold notebook reads new Silver records, adds `country_code` (reverse geocoding) and `sig_class`, and appends to the Gold Delta table.
5. Power BI connects to the Gold table for reporting.

## Setup

1. Create a Fabric workspace with a Lakehouse named `earthquake_lakehouse`.
2. Import the three notebooks and attach them to the lakehouse.
3. Set up a Data Pipeline to pass date parameters and run the notebooks in sequence (Bronze → Silver → Gold).
4. Open `Main_Earthquake_report.pbix` in Power BI Desktop and point it to the Gold table.

## Author

Arnab Kar
