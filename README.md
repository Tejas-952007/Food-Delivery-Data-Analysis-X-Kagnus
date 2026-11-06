# Food Delivery Data Analysis — Kagnus Internship

[![Python](https://img.shields.io/badge/Python-3.11%2B-blue.svg)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.0%2B-150458.svg)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/NumPy-1.24%2B-013243.svg)](https://numpy.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 📌 Project Overview
This repository contains the end-to-end coursework and deliverables for the **Food Delivery Data Analysis Internship** at **Kagnus**. Working as a Junior Data Analyst supporting the food-delivery operations team, the multi-week goal is to analyze operational delivery logs, extract actionable business insights, identify supply chain and fulfillment bottlenecks, and build interactive KPI visualizations.

The project follows a 4-week structured progression:
- **Week 1 (Current)**: Dataset Understanding, Comprehensive Quality Audit & Data Cleaning.
- **Week 2**: Exploratory Data Analysis (EDA) & Feature Engineering.
- **Week 3**: Core Operational KPI Analysis & Visualizations.
- **Week 4**: Business Findings, Strategic Recommendations & Final Executive Report.

---

## 🎯 Week 1: Dataset Understanding & Cleaning Objectives
The objective of Week 1 is to establish a verified, standardized, and fully reproducible baseline dataset:
1. **Ingest and inspect** the raw food delivery operational logs (`Food_delivery_Dataset.csv`).
2. **Audit data quality**:
   - Quantify missing value rates across demographic, temporal, and spatial attributes.
   - Verify transaction key uniqueness on `ID` and audit for duplicate transactions.
   - Identify inconsistent category labels and spelling variations (e.g., `Metropolitian`).
   - Audit numerical ranges for impossible ratings (> 5.0), negative coordinates, and sentinel GPS placeholders.
   - Detect corrupted timestamp formats caused by Excel day-fraction exports and rollover hours (`24:xx`).
3. **Execute non-destructive data cleaning**:
   - Keep the raw dataset completely untouched.
   - Apply rule-based transformations on a copy and export to `data/processed/cleaned_food_delivery.csv`.
   - Maintain a complete audit trail in `docs/cleaning_log.csv`.
4. **Verify processed dataset fidelity** and confirm exact row counts before and after cleaning.

---

## 🗂️ Repository Structure
```text
food-delivery-data-analysis/
├── data/
│   ├── raw/
│   │   ├── Food_delivery_Dataset.csv     # Original unedited raw dataset (45,584 rows)
│   │   └── README.md                     # Raw data source documentation & dictionary
│   └── processed/
│       └── cleaned_food_delivery.csv     # Cleaned, standardized dataset (45,584 rows)
├── notebooks/
│   └── 01_data_understanding_and_cleaning.ipynb  # Fully executed, documented Jupyter notebook
├── docs/
│   └── cleaning_log.csv                  # Formal audit log recording issues, counts & rationales
├── README.md                             # Root project overview & Week 1 findings
└── requirements.txt                      # Python environment package dependencies
```

---

## 📊 Dataset Summary & What One Row Represents
- **Source**: Kagnus Food Delivery Operations Portal.
- **Total Records**: 45,584 rows.
- **Total Attributes**: 20 columns.
- **Unit of Analysis**: Each row represents a **single discrete food delivery order transaction** fulfilled by a delivery executive from restaurant pickup to customer doorstep.

### Key Columns & Data Types:
| Column Name | Raw Type | Cleaned Type | Business Meaning |
| :--- | :--- | :--- | :--- |
| `ID` | `object` | `object` | Unique alphanumeric order transaction ID (hexadecimal format). |
| `Delivery_person_ID` | `object` | `object` | Unique identifier for delivery partner (city code prefix). |
| `Delivery_person_Age` | `float64` | `float64` | Age of delivery partner (15–50 years; 1,854 missing). |
| `Delivery_person_Ratings` | `float64` | `float64` | Delivery partner rating (scale 1.0–5.0; 53 capped at 5.0). |
| `Restaurant_latitude` | `float64` | `float64` | Restaurant latitude (sign corrected; 3,640 sentinel (0,0) masked to NaN). |
| `Restaurant_longitude` | `float64` | `float64` | Restaurant longitude (sign corrected; 3,640 sentinel (0,0) masked to NaN). |
| `Delivery_location_latitude` | `float64` | `float64` | Delivery drop latitude (sentinel 0.01 masked to NaN). |
| `Delivery_location_longitude` | `float64` | `float64` | Delivery drop longitude (sentinel 0.01 masked to NaN). |
| `Order_Date` | `object` | `object` (ISO) | Calendar date of order placement (`YYYY-MM-DD`). |
| `Time_Orderd` | `object` | `object` (HH:MM:SS) | Standardized timestamp when order was placed. |
| `Time_Order_picked` | `object` | `object` (HH:MM:SS) | Standardized timestamp when order was picked up. |
| `Weather_conditions` | `object` | `object` | Weather condition during delivery (Sunny, Fog, Stormy, etc.). |
| `Road_traffic_density` | `object` | `object` | Traffic congestion level (Low, Medium, High, Jam). |
| `Vehicle_condition` | `int64` | `int64` | Vehicle mechanical condition index (0–3). |
| `Type_of_order` | `object` | `object` | Meal type (Snack, Meal, Drinks, Buffet). |
| `Type_of_vehicle` | `object` | `object` | Transport mode (motorcycle, scooter, electric_scooter, bicycle). |
| `multiple_deliveries` | `float64` | `float64` | Count of bundled deliveries (0–3). |
| `Festival` | `object` | `object` | Festival period flag (`Yes` / `No`). |
| `City` | `object` | `object` | Urban classification (`Metropolitan`, `Urban`, `Semi-Urban`). |
| `Time_taken (min)` | `int64` | `int64` | Total order fulfillment duration in minutes (10–54 min). |

---

## 🔍 Key Data Quality Observations & Cleaning Decisions

### 1. Inconsistent Category Labeling
- **Observation**: 34,093 records in the `City` column had the spelling variation `Metropolitian`.
- **Action**: Standardized all occurrences to the standard spelling `Metropolitan`.

### 2. Geodetic Coordinate Sign Errors & Sentinel Values
- **Observation**: 431 restaurant latitudes and 162 restaurant longitudes were negative. Since India is situated entirely in the Northern (+Lat) and Eastern (+Lon) hemispheres, negative signs are entry errors. Additionally, 3,640 records contained `(0.0, 0.0)` for restaurant coordinates and `(0.01, 0.01)` for drop-off locations (denoting missing GPS telemetry).
- **Action**: Applied `abs()` to restore valid positive coordinates. Replaced placeholder `(0.0, 0.0)` and `(0.01, 0.01)` values with `np.nan` to prevent severe distortion of spatial distance calculations, while keeping transaction records intact.

### 3. Outlier Ratings
- **Observation**: Exactly 53 delivery partner ratings were recorded as `6.0`, exceeding the 1.0–5.0 platform rating scale.
- **Action**: Capped ratings > 5.0 to 5.0, treating them as upper-bound ratings or calibration typos.

### 4. Excel Day-Fraction Timestamps & Midnight Rollovers
- **Observation**: In `Time_Orderd` (4,068 rows) and `Time_Order_picked` (4,127 rows), timestamps were stored as Excel decimal day fractions (e.g., `0.458333` = 11:00 AM, `0.958333` = 11:00 PM). Furthermore, 880 records in `Time_Order_picked` had `24:05:00`, `24:10:00`, and `24:15:00` representing pickup times 5–15 minutes past midnight.
- **Action**: Developed a vectorized conversion parser to transform decimal serials and rollover hours into standardized `HH:MM:SS` strings (e.g. `24:05:00` -> `00:05:00`).

### 5. Row Preservation & Missing Value Handling
- **Row Count Before**: 45,584
- **Row Count After**: 45,584
- **Difference**: 0 rows removed.
- **Rationale**: In strict compliance with task instructions (*"Do not automatically remove outliers, duplicates, or records without checking whether removal is justified"*), all rows were retained because every row represents an authentic unique delivery transaction with valid trip duration and vehicle data. Missing attributes were preserved as `NaN` rather than synthetically imputed to avoid introducing artificial bias before feature modeling.

---

## 🛠️ Tools & Libraries Used
- **Python 3.11+**: Primary runtime environment.
- **Pandas (2.0+)**: Data manipulation, inspection, and cleaning transformations.
- **NumPy (1.24+)**: Vectorized operations and sentinel value handling.
- **Matplotlib & Seaborn**: Quality audit visualization (missing value distribution chart).
- **Jupyter Notebook**: Interactive documentation and reproducible pipeline execution.

---

## 🚀 Setup & Execution Instructions

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/food-delivery-data-analysis.git
cd food-delivery-data-analysis
```

### 2. Create and Activate a Virtual Environment
```bash
python3 -m venv .venv
source .venv/bin/activate  # On Linux/macOS
# or .venv\Scripts\activate on Windows
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Run the Jupyter Notebook
```bash
jupyter notebook notebooks/01_data_understanding_and_cleaning.ipynb
```
You can also run all cells from top to bottom; the notebook executes cleanly without errors and will generate `data/processed/cleaned_food_delivery.csv` and verify it automatically.

---

## 📝 Assumptions & Limitations
1. **Missing GPS Coordinates**: 3,640 trips have missing GPS coordinates (`NaN`). For distance-based speed analysis in Week 2, these rows must be analyzed separately or excluded specifically from distance-dependent models.
2. **Delivery Partner Minors**: 38 delivery executives have an age of 15. These records were retained to prevent data fabrication, but will be treated as flagged anomalies in demographic analyses.
3. **Imputation Policy**: Imputation was intentionally deferred to Week 2 so that exploratory analysis can inform whether median, mean, or model-based imputation is most appropriate for each feature.
