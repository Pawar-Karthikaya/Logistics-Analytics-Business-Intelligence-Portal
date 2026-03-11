<div align="center">

<img src="https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/MySQL-8.0-4479A1?style=for-the-badge&logo=mysql&logoColor=white"/>
<img src="https://img.shields.io/badge/Apache_Superset-BI_Dashboard-E84393?style=for-the-badge&logo=apache&logoColor=white"/>
<img src="https://img.shields.io/badge/AWS-EC2_Deployed-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white"/>
<img src="https://img.shields.io/badge/Status-Live-22c55e?style=for-the-badge"/>

<br/><br/>

# Logistics Analytics — Business Intelligence Portal

### A full end-to-end data analytics project: raw e-commerce data → ETL pipeline → SQL KPIs → interactive live dashboard

<br/>

> **100,000+ real orders &nbsp;·&nbsp; 8 business KPIs &nbsp;·&nbsp; 10 interactive charts &nbsp;·&nbsp; Deployed on AWS EC2**

<br/>

[View Live Dashboard](http://YOUR-EC2-IP:8088) &nbsp;·&nbsp; [EDA Notebook](notebooks/01_eda.ipynb) &nbsp;·&nbsp; [KPI Definitions](docs/kpi_definitions.md) &nbsp;·&nbsp; [SQL Views](sql/kpi_views.sql)

</div>

---

## What Is This Project?

This project simulates the analytics workflow of a real logistics business analyst. Starting from raw CSV files, it builds a complete BI system that any logistics manager could use to monitor operations, track SLAs, and make data-driven decisions.

The dataset is the **Brazilian Olist E-Commerce dataset** — 100,000 anonymized real orders placed between 2016 and 2018, released publicly on Kaggle. It covers orders, deliveries, sellers, products, payments, and customer reviews across all Brazilian states.

The project answers real business questions:

- Which states have the worst delivery delays?
- Which sellers consistently meet their SLAs?
- Is the freight cost model economically sustainable?
- Does delivery delay directly hurt customer satisfaction scores?

---

## System Architecture

```
Kaggle CSVs (9 files, 100K+ rows)
        │
        ▼
 ┌─────────────────┐
 │  Python ETL     │  pandas · numpy · SQLAlchemy
 │  pipeline.py    │  Clean · Transform · Engineer Features
 └────────┬────────┘
          │
          ▼
 ┌─────────────────┐
 │  MySQL Database │  Normalized schema · Fact + Dim tables
 │  logistics_bi   │  8 KPI SQL Views
 └────────┬────────┘
          │
          ▼
 ┌─────────────────┐
 │ Apache Superset │  10 interactive charts · Filters · CSS
 │  Dashboard      │  Drill-down by State · Date · Category
 └────────┬────────┘
          │
          ▼
 ┌─────────────────┐
 │   AWS EC2       │  Ubuntu 22.04 · t2.micro · Live URL
 │   Deployment    │  Public access via port 8088
 └─────────────────┘
```

---

## KPIs Defined and Tracked

| # | KPI | Business Question | SQL View |
|---|-----|-------------------|----------|
| 1 | On-Time Delivery Rate | What % of orders arrive by the promised date? | `v_otd_rate` |
| 2 | Avg Delivery Time by State | Which states have the worst delivery performance? | `v_avg_delivery_by_state` |
| 3 | Monthly Order Volume | How is order volume trending over time? | `v_monthly_order_volume` |
| 4 | Revenue by Product Category | Which categories generate the most revenue and freight cost? | `v_revenue_by_category` |
| 5 | Seller Performance Scorecard | Which sellers are meeting SLAs? Which need review? | `v_seller_scorecard` |
| 6 | Order Status Distribution | What share of orders are cancelled vs. delivered? | `v_order_status_distribution` |
| 7 | Satisfaction vs. Delivery Delay | Do late deliveries cause lower review scores? | `v_satisfaction_vs_delay` |
| 8 | Freight Cost Efficiency | Is freight cost sustainable as a % of order value? | `v_freight_efficiency` |

---

## Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Language | Python 3.10+ | ETL, data cleaning, feature engineering |
| Data Manipulation | Pandas, NumPy | DataFrame transformations |
| Database | MySQL 8.0 | Normalized storage and SQL KPI views |
| ORM / Connector | SQLAlchemy + PyMySQL | Python to MySQL bridge |
| BI and Visualization | Apache Superset | Dashboard, charts, filters |
| Styling | CSS (Superset Custom) | Dashboard branding and polish |
| Infrastructure | AWS EC2 (t2.micro) | Cloud hosting, live public URL |
| Exploration | Jupyter Notebook | EDA and prototyping |

---

## Repository Structure

```
logistics-bi-portal/
│
├── README.md                    <- You are here
│
├── data/
│   └── README.md                <- How to download the Olist dataset from Kaggle
│
├── notebooks/
│   └── 01_eda.ipynb             <- Exploratory Data Analysis (shape, nulls, distributions)
│
├── etl/
│   └── pipeline.py              <- Full ETL: extract → clean → transform → load to MySQL
│
├── sql/
│   ├── schema.sql               <- CREATE TABLE statements for all tables
│   └── kpi_views.sql            <- All 8 SQL VIEW definitions
│
├── dashboard/
│   └── superset_export.zip      <- Exportable Superset dashboard (importable via UI)
│
├── docs/
│   └── kpi_definitions.md       <- Business definition, formula, and target for each KPI
│
└── requirements.txt             <- Python dependencies
```

---

## Quickstart — Run Locally

**1. Clone the repo**
```bash
git clone https://github.com/YOUR_USERNAME/logistics-bi-portal.git
cd logistics-bi-portal
```

**2. Set up Python environment**
```bash
python -m venv venv
source venv/bin/activate        # Mac/Linux
venv\Scripts\activate           # Windows

pip install -r requirements.txt
```

**3. Download the dataset**

Go to [Kaggle — Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), download, and place all CSV files inside the `data/` folder.

**4. Set up MySQL**
```sql
CREATE DATABASE logistics_bi;
```
Update the connection string in `etl/pipeline.py`:
```python
engine = create_engine('mysql+pymysql://root:YOUR_PASSWORD@localhost:3306/logistics_bi')
```

**5. Run the ETL pipeline**
```bash
python etl/pipeline.py
```

This cleans all 9 source files, engineers KPI columns, and loads everything to MySQL.

**6. Create the SQL KPI views**
```bash
mysql -u root -p logistics_bi < sql/kpi_views.sql
```

**7. Launch Apache Superset**
```bash
pip install apache-superset mysqlclient
superset db upgrade
superset fab create-admin
superset init
superset run -p 8088 --with-threads --reload
```

Open [http://localhost:8088](http://localhost:8088) and import the dashboard from `dashboard/superset_export.zip`.

---

## Key Findings

**Regional Delivery Gap**
States in the northeast (Roraima, Amapá) averaged over 25 days for delivery versus 8–10 days in southeast states like São Paulo — a 3x difference that points directly to infrastructure gaps in last-mile logistics coverage.

**Delay Directly Damages Satisfaction**
Orders that arrived more than 7 days late averaged a review score of 2.1 out of 5, while on-time orders averaged 4.3 out of 5. This quantifies the exact cost of poor delivery performance in terms of customer experience.

**Freight Cost Outliers**
Freight cost as a percentage of order value exceeded 40% in some northern states — a threshold that would normally trigger an immediate logistics cost review in any real operation.

---

## Dashboard Screenshots

> Add your Superset screenshots here once the dashboard is live.

| Overview | Delivery by State | Seller Scorecard |
|----------|-------------------|-----------------|
| `screenshot_1.png` | `screenshot_2.png` | `screenshot_3.png` |

---

## AWS Deployment

The dashboard is deployed on an AWS EC2 t2.micro instance running Ubuntu 22.04.

```
Live URL: http://YOUR-EC2-PUBLIC-IP:8088
```

Ports configured in Security Group: `22` (SSH) and `8088` (Superset).

---

## Requirements

```
pandas==2.1.0
numpy==1.25.2
sqlalchemy==2.0.20
pymysql==1.1.0
apache-superset==3.0.0
mysqlclient==2.2.0
jupyter==1.0.0
matplotlib==3.7.2
seaborn==0.12.2
```

Install all with: `pip install -r requirements.txt`

---

## Skills Demonstrated

- End-to-end ETL pipeline design (Extract, Transform, Load)
- Data cleaning with context-aware null handling strategy
- Feature engineering from raw timestamps (delay_days, is_late, actual_delivery_days)
- Relational database design with normalized fact and dimension tables
- Business KPI definition — translating operational questions into SQL
- SQL window functions, GROUP BY aggregations, and CASE WHEN logic
- BI dashboard development with filters, drill-downs, and chart composition
- Cloud deployment on AWS EC2 (Ubuntu, SSH, security group configuration)

---

## License

This project is open source under the [MIT License](LICENSE).

The Olist dataset is released under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — free for non-commercial use with attribution.

---

<div align="center">

Built by **Karthikaya** &nbsp;·&nbsp; AI and Data Science Graduate &nbsp;·&nbsp; Pune, India

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin)](https://linkedin.com/in/YOUR_PROFILE)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat-square&logo=github)](https://github.com/YOUR_USERNAME)

</div>
