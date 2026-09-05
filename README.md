# 🌍 Earthquake Data Lakehouse & Analytics

_Building an end-to-end Medallion (Bronze-Silver-Gold) data pipeline on Microsoft Fabric to ingest, transform, and visualize global earthquake data using PySpark, SQL, and Power BI._

---

## 📌 Table of Contents
- <a href="#overview">Overview</a>
- <a href="#business-problem">Business Problem</a>
- <a href="#dataset">Dataset</a>
- <a href="#tools--technologies">Tools & Technologies</a>
- <a href="#project-structure">Project Structure</a>
- <a href="#data-pipeline--architecture">Data Pipeline & Architecture</a>
- <a href="#data-cleaning--transformation">Data Cleaning & Transformation</a>
- <a href="#key-findings">Key Findings</a>
- <a href="#dashboard">Dashboard</a>
- <a href="#how-to-run-this-project">How to Run This Project</a>
- <a href="#final-recommendations">Final Recommendations</a>
- <a href="#author--contact">Author & Contact</a>

---
<h2><a class="anchor" id="overview"></a>Overview</h2>

This project builds a cloud-based data lakehouse to ingest live earthquake data from the USGS Earthquake API and transform it through a Medallion architecture (Bronze → Silver → Gold) on Microsoft Fabric. The final enriched dataset is visualized in Power BI to surface patterns in seismic activity, location, and significance.

---
<h2><a class="anchor" id="business-problem"></a>Business Problem</h2>

Raw earthquake feeds are unstructured, ungeolocated, and hard to interpret at scale. This project aims to:
- Ingest and structure real-time global earthquake event data
- Enrich raw coordinates with country-level location context
- Classify events by significance to prioritize monitoring/reporting
- Provide a visual, explorable dashboard of seismic trends over time and geography

---
<h2><a class="anchor" id="dataset"></a>Dataset</h2>

- Source: [USGS Earthquake API](https://earthquake.usgs.gov/fdsnws/event/1/) (GeoJSON, pulled by date range)
- Fields: coordinates, elevation, magnitude, magnitude type, significance score, event time
- `Parameter.csv` — drives the date-range parameters for pipeline runs

---
<h2><a class="anchor" id="tools--technologies"></a>Tools & Technologies</h2>

- Microsoft Fabric (Lakehouse, Notebooks, Data Pipelines)
- PySpark (data transformation)
- Python (`requests`, `reverse-geocoder`)
- SQL (Lakehouse/Warehouse schema)
- Power BI (dashboard/visualization)
- GitHub

---
<h2><a class="anchor" id="project-structure"></a>Project Structure</h2>


---
<h2><a class="anchor" id="data-pipeline--architecture"></a>Data Pipeline & Architecture</h2>


- **Bronze** — Fetches raw earthquake GeoJSON from the USGS API for a given date range and lands it in the lakehouse Files section.
- **Silver** — Flattens the raw JSON, extracts coordinates/magnitude/time fields, handles nulls, and casts timestamps into a clean Delta table (`earthquake_events_silver`).
- **Gold** — Enriches Silver data with reverse-geocoded **country codes** and a derived **significance class** (Low/Moderate/High), writing to `earthquake_events_gold`.
- **Power BI** — Connects to the Gold table for reporting.

---
<h2><a class="anchor" id="data-cleaning--transformation"></a>Data Cleaning & Transformation</h2>

- Handled null/missing latitude, longitude, and time values
- Converted Unix epoch time fields to proper timestamps
- Reshaped nested GeoJSON (`geometry.coordinates`, `properties.*`) into flat tabular columns
- Reverse-geocoded coordinates into country codes using `reverse-geocoder`
- Classified events into significance tiers based on the `sig` score:
  - Low: `sig < 100`
  - Moderate: `100 ≤ sig < 500`
  - High: `sig ≥ 500`

---
<h2><a class="anchor" id="key-findings"></a>Key Findings</h2>

1. Earthquake events cluster heavily around known tectonic boundaries (Pacific Ring of Fire regions)
2. A notable share of events fall into the "High" significance tier despite moderate magnitude, driven by depth/impact factors in the `sig` score
3. Country-level enrichment enables geographic drill-down not available in the raw feed
4. Weekly ingestion windows are sufficient to capture global seismic activity without excessive data volume

---
<h2><a class="anchor" id="dashboard"></a>Dashboard</h2>

- Power BI Dashboard shows:
  - Global map of earthquake events by location and magnitude
  - Significance distribution (Low/Moderate/High)
  - Trends in event frequency over time
  - Country-level breakdown of seismic activity

---
<h2><a class="anchor" id="how-to-run-this-project"></a>How to Run This Project</h2>

1. Clone the repository:
```bash
git clone https://github.com/Arnab-710/earthquake-analysis-fabric-pyspark-powerbi.git
```
2. Create a Fabric workspace with a Lakehouse named `earthquake_lakehouse`.
3. Import the notebooks from `Notebook/` and attach them to the lakehouse.
4. Run notebooks in sequence:
   - `Bronze_notebook.ipynb`
   - `Silver_notebook.ipynb`
   - `Gold_notebook.ipynb`
5. Open the dashboard:
   - `Dashboard/Main_Earthquake_report.pbix`

---
<h2><a class="anchor" id="final-recommendations"></a>Final Recommendations</h2>

- Automate the pipeline with a scheduled Fabric Data Pipeline for continuous ingestion
- Add alerting for newly ingested "High" significance events
- Extend enrichment with tectonic plate boundary data for deeper geographic context
- Track historical trend baselines to flag anomalous spikes in activity

---
<h2><a class="anchor" id="author--contact"></a>Author & Contact</h2>

**Arnab Kar**

📧 Email: arnabk734@gmail.com
🔗 [LinkedIn](https://www.linkedin.com/in/arnab-kar10/)
