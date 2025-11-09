# Data Integration Pipeline – From Web to Database

This repository presents a **fully reproducible data integration pipeline** that combines information from multiple open data sources — including **web scraping**, **API-based weather data**, and **World Bank economic indicators** — into a unified SQL database.

The project demonstrates end-to-end **data engineering and analytics skills**, integrating raw data collection, cleaning, database management, and visualization in a transparent and reproducible workflow.

------------------------------------------------------------------------

## 1) Project Overview and Objectives

The main objective of this project is to **build a reproducible global data pipeline**, connecting multiple heterogeneous sources to extract, harmonize, and analyze structured information across countries.

**Global Objective:**\
Integrate data about **countries**, **weather**, and **economic indicators (GDP)** into a harmonized SQL environment that enables consistent cross-country comparisons.

**Specific Components (each notebook = one stage):**

| Notebook | Title | Main Objective |
|----------------------|------------------|--------------------------------|
| **01_scrapethissite.qmd** | *Web Scraping – ScrapeThisSite* | Extract structured data (country, capital, population, area) from an educational webpage. Clean text fields and export a reproducible dataset. |
| **02_weather_api.qmd** | *Weather Data Collection – OpenWeather API* | Retrieve live temperature and humidity data for each capital using OpenWeather. Harmonize ISO-2 country codes and store all results in SQLite. |
| **03_sql_integration.qmd** | *SQL Integration and Analysis – World Bank GDP* | Combine the scraped, weather, and World Bank GDP datasets through SQL joins, compute average GDP (1960–2024), and visualize global economic trends. |

The notebooks are designed as modular components of a single pipeline, where each stage produces the inputs required by the next. Execute them in order (01 → 02 → 03) to reproduce the full workflow

------------------------------------------------------------------------

## 2) Prerequisites

These tools are required to reproduce the project:

| Tool | Purpose | Installation |
|------------------|----------------------|--------------------------------|
| **Quarto (≥ 1.5)** | Render and document notebooks | [quarto.org/docs/get-started](https://quarto.org/docs/get-started) |
| **Python (≥ 3.11)** | Run `.qmd` notebooks | [python.org/downloads](https://www.python.org/downloads/) |
| **Jupyter** | Required kernel backend for Quarto | `pip install jupyter` |
| **Git (optional)** | Clone and version the repository | [git-scm.com/downloads](https://git-scm.com/downloads) |

### 2.1 Environment Verification (Before Installing)

Before installing anything, verify that **Python**, **pip**, **Jupyter**, and **Quarto** are already available on your system.

This quick check helps avoid unnecessary reinstallation or version conflicts.

#### Windows (PowerShell or CMD)

``` powershell
python --version
pip --version
jupyter --version
quarto check
```

#### macOS / Linux (Terminal)

```         
python3 --version
pip3 --version
jupyter --version
quarto check
```

**Expected output:**

Each command should return a version number, for example:

```         
Python 3.11.6
pip 24.0
jupyter 7.0.4
[✓] Checking Jupyter engine... OK
```

If any command returns an error such as “command not found” or “not recognized”, install the missing tool using the official links listed in the table above.

### 2.2 PDF rendering

If rendering to PDF, install **TinyTeX**:

```         
quarto install tinytex
quarto check
```

### 2.3 Recommended IDEs

| Tool | Compatibility | Notes |
|------------------|------------------|------------------------------------|
| **VS Code** (Quarto extension) | Full | Recommended for editing and rendering |
| **Positron IDE** | Full | Supports Python and SQL cells seamlessly |
| **Terminal / PowerShell** | Full | Use `quarto render <file>.qmd` for any OS |

### 2.4 API Key Setup

To run the **OpenWeather API** notebook:

1.  Create a personal API key at [openweathermap.org/api](https://openweathermap.org/api)

2.  Create a `.env` file in the project root

3.  Add your key inside it:

```         
OPENWEATHER_API_KEY=your_personal_key_here
```

The notebooks automatically load this variable using python-dotenv.

------------------------------------------------------------------------

## 3) Repository Structure

The repository mirrors the entire workflow — from data collection to integration and visualization — ensuring full reproducibility.

```         
data-integration-pipeline/
├── data/
│   ├── raw/               # Original, unmodified data (scraped or downloaded)
│   ├── processed/         # Cleaned, harmonized datasets
│   └── global_data.db     # Central SQLite database
│
├── figures/               # Visual outputs (e.g., GDP trend plots)
├── reports/               # Rendered Quarto outputs (HTML/PDF)
├── notebooks/             # Quarto notebooks (Python + SQL)
│   ├── 01_scrapethissite.qmd
│   ├── 02_weather_api.qmd
│   └── 03_sql_integration.qmd
│
├── requirements.txt       # Python dependencies
├── .gitignore             # Excludes caches, environments, and generated data
└── README.md              # Project documentation
```

**Highlights**

-   `/data/raw/` → unmodified inputs (HTML, CSV)
-   `/data/processed/` → cleaned datasets aligned on ISO-2 codes
-   `global_data.db` → unified SQL database
-   `/notebooks/` → reproducible Quarto source files
-   `/reports/` → rendered results
-   `/figures/` → generated visualizations

> **Note:** Some datasets (e.g., `.csv`, `.db`, `.html`) are generated automatically during notebook execution and excluded from version control for efficiency.

------------------------------------------------------------------------

## 4) Running to Project

### 4.1 Environment Setup (run in terminal)

**Windows (PowerShell):**

```         
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux**

```         
python3 -m venv .venv
source .venv/bin/activate
```

**Install dependencies**

```         
pip install -r requirements.txt
```

### 4.2 Notebook Execution (run in terminal)

Render each notebook sequentially to rebuild the pipeline:

```         
quarto render notebooks/01_scraping_scrapethissite.qmd --to html --output-dir ../reports

quarto render notebooks/02_weather_api_openweather.qmd --to html --output-dir ../reports

quarto render notebooks/03_worldbank_gdp.qmd --to html --output-dir ../reports
```

> Add `--to pdf` if TinyTeX is installed and you prefer PDF reports.

After execution, you’ll have:

```         
data/raw/             → Original inputs
data/processed/       → Cleaned datasets
data/global_data.db   → Centralized database
figures/              → Generated plots
reports/              → Rendered notebooks (HTML/PDF)
```

## 5) Reproducibility Principles

-   **Reproducibility:** All datasets and outputs are generated programmatically through the notebooks.
-   **Traceability:** Every artifact is stored in structured folders (`data/`, `figures/`, `reports/`).
-   **Integrity:** The SQLite database consolidates all datasets using ISO-2 country codes.
-   **Automation:** Scripts detect existing files to avoid redundant downloads or API calls.

> Running all notebooks once is enough to rebuild the entire project from scratch.

------------------------------------------------------------------------

## 6) Troubleshooting

The following table summarizes common issues encountered during execution and their recommended solutions.

| Issue | Possible Solution |
|-------------------------|-----------------------------------------------|
| PDF render fails | Install TinyTeX or render to HTML instead. |
| API key missing | Ensure `.env` contains `OPENWEATHER_API_KEY`. |
| Database not found | Run all notebooks sequentially (01 → 02 → 03). |
| Duplicate SQL columns | Use explicit aliases (`AS`) when joining tables. |
| Permission denied (file locked) | Close the file and re-run notebook. |
| Matplotlib backend error | Add `matplotlib.use('Agg')` for headless rendering. |

------------------------------------------------------------------------

## 7) Data Sources

All sources are open, educational, and ethically accessed.

| Source | Description | Link |
|-------------------------|------------------------|-----------------------|
| **ScrapeThisSite.com** | Structured tables of countries, capitals, population, and area. | [scrapethissite.com/pages/simple](https://www.scrapethissite.com/pages/simple/) |
| **OpenWeather API** | Real-time weather data (temperature and humidity) for global capitals. | [openweathermap.org/api](https://openweathermap.org/api) |
| **World Bank API – World Development Indicators** | GDP data (current USD) by country and year (1960–2024). | [data.worldbank.org/indicator/NY.GDP.MKTP.CD](https://data.worldbank.org/indicator/NY.GDP.MKTP.CD) |
| **World Bank Country Metadata** | ISO-3 → ISO-2 mapping for harmonization. | [api.worldbank.org/v2/country](https://api.worldbank.org/v2/country) |

> All data were retrieved through authorized public endpoints.

------------------------------------------------------------------------

## 8) Author

**Ignacio Vélez Ocampo**\
*MSc Management – Business Analytics*\
Focused on integrating analytics, automation, and reproducible workflows for data-driven insights.

------------------------------------------------------------------------