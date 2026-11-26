# Food Delivery Data Analysis — Kagnus Internship

[![Python](https://img.shields.io/badge/Python-3.11%2B-blue.svg)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.0%2B-150458.svg)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/NumPy-1.24%2B-013243.svg)](https://numpy.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 📌 Project Overview
This repository contains the end-to-end coursework and deliverables for the **Food Delivery Data Analysis Internship** at **Kagnus**. Working as a Junior Data Analyst supporting the food-delivery operations team, the multi-week goal is to analyze operational delivery logs, extract actionable business insights, identify supply chain and fulfillment bottlenecks, and build interactive KPI visualizations.

The project follows a 4-week structured progression:
- **Week 1 (Complete)**: Dataset Understanding, Comprehensive Quality Audit & Data Cleaning.
- **Week 2 (Complete)**: Exploratory Data Analysis (EDA) & Operational Driver Analysis.
- **Week 3 (Complete / Current)**: Visualization & Interpretation.
- **Week 4 (Upcoming)**: Business Findings, Strategic Recommendations & Final Executive Report.

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
│   │   ├── Food_delivery_Dataset.csv             # Original unedited raw dataset (45,584 rows)
│   │   └── README.md                             # Raw data source documentation & dictionary
│   └── processed/
│       └── cleaned_food_delivery.csv             # Cleaned, standardized dataset (45,584 rows)
├── notebooks/
│   ├── 01_data_understanding_and_cleaning.ipynb  # Week 1: Data Understanding & Cleaning Pipeline
│   ├── 02_exploratory_data_analysis.ipynb        # Week 2: Comprehensive Exploratory Data Analysis
│   └── 03_visualization.ipynb                    # Week 3: Visualization & Interpretation
├── visualizations/                               # Exported high-resolution figures (150 DPI)
│   ├── 01_delivery_duration_distribution.png     # Week 3: Delivery duration histogram
│   ├── 02_traffic_comparison.png                 # Week 3: Median duration by traffic category
│   ├── 03_weather_comparison.png                 # Week 3: Average duration by weather condition
│   ├── 04_distance_vs_duration.png               # Week 3: Distance vs duration scatter plot
│   └── 05_vehicle_comparison.png                 # Week 3: Duration distribution by vehicle (boxplot)
├── docs/
│   └── cleaning_log.csv                          # Formal audit log recording issues, counts & rationales
├── README.md                                     # Project overview, findings & execution guide
└── requirements.txt                              # Python environment package dependencies
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

## 🔍 Week 2: Exploratory Data Analysis (EDA) & Operational Driver Analysis

### 🎯 Week 2 Objectives
1. Compute delivery duration baseline statistics (central tendency, dispersion, spread).
2. Segment delivery performance across operational factors (traffic, weather, vehicle type, festival).
3. Compute straight-line (Haversine) trip distances and evaluate linear vs non-linear correlation with duration.
4. Conduct confounding checks to prevent spurious conclusions.
5. Synthesize empirical findings to inform operational strategies for Week 3 & 4.

---

### 📈 Baseline Operational Performance Summary
Across all **45,584** completed orders:
- **Mean Duration**: `26.29 minutes`
- **Median Duration**: `26.00 minutes`
- **Standard Deviation**: `9.38 minutes`
- **Interquartile Range (IQR)**: `15.00 minutes` (Q1: 19.00 min, Q3: 34.00 min)
- **Observed Range**: `10.00 – 54.00 minutes`

---

### 🚦 Key Operational Comparisons

| Operational Factor | Category / Level | Delivery Count | Mean Duration (min) | Median Duration (min) | Key Finding & Operational Takeaway |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Traffic Density** | Low | 15,476 | **21.27** | 21.00 | Baseline rapid transit conditions. |
| | Medium | 10,945 | **26.68** | 26.00 | +5.41 min operational delay over Low traffic. |
| | High | 4,424 | **27.21** | 27.00 | +5.94 min delay over Low traffic. |
| | Jam | 14,139 | **31.18** | 32.00 | **+9.91 min (+46.6%)** congestion penalty. Primary system bottleneck. |
| **Weather** | Sunny | 7,282 | **21.86** | 22.00 | Fastest atmospheric transit conditions. |
| | Stormy / Sandstorms / Windy | 22,500 | **25.84 – 26.17** | 26.00 | Moderate atmospheric impact (+4 min). |
| | Fog / Cloudy | 15,186 | **28.92** | 29.00 | **+7.06 min (+32.3%)** adverse weather delay. |
| **Vehicle Type** | Electric Scooter | 3,814 | **24.47** | 24.00 | Fastest vehicle class in urban deliveries. |
| | Scooter | 15,273 | **24.48** | 24.00 | Highly consistent and efficient mid-tier. |
| | Motorcycle | 26,429 | **27.59** | 27.00 | Heavy workload fleet; carries longer congestion exposure. |
| | Bicycle | 68 | **26.44** | 26.00 | Niche short-radius deployment. |
| **Batch Deliveries** | 0 Deliveries | 14,095 | **21.05** | 20.00 | Single dedicated drop-off route. |
| | 1 Delivery | 28,151 | **27.57** | 28.00 | +6.52 min overhead. |
| | 2 Deliveries | 3,400 | **32.84** | 33.00 | +11.79 min overhead. |
| | 3 Deliveries | 890 | **37.28** | 38.00 | **+16.23 min (+77.1%)** multi-drop batching delay. |
| **Festival** | No | 44,460 | **25.90** | 26.00 | Standard operating volume. |
| | Yes | 896 | **45.56** | 46.00 | **+19.66 min (+75.9%)** severe festive demand/kitchen delay. |

---

### 🗺️ Distance vs Duration: What the Data Reveals
- **Sample Size**: 41,944 deliveries with valid non-sentinel coordinates (3,640 sentinel (0,0) rows excluded).
- **Haversine Distance Range**: `1.46 km – 20.97 km` (Mean: `9.70 km`).
- **Correlation**:
  - Pearson Linear Correlation: $r = 0.320$ ($r^2 = 0.102$)
  - Spearman Rank Correlation: $\rho = 0.319$
  - Fitted Linear Trend: $\text{Duration (min)} = 21.05 + 0.54 \times \text{Distance (km)}$
- **Analytical Takeaway**: While distance has a statistically significant positive association with duration, $r^2 = 0.102$ proves that **distance alone explains only ~10% of total delivery time variation**. The remaining ~90% is driven by operational factors: traffic congestion, dispatch delays, kitchen preparation, and weather conditions.

---

### 🛡️ Confounding Checks
To verify that vehicle differences were not merely artifacts of trip length:
- Mean distance for **motorcycles**: `9.69 km`
- Mean distance for **scooters**: `9.76 km`
- Mean distance for **electric scooters**: `9.44 km`
Because distance is evenly balanced across fleets, the observed delivery speed advantage of electric scooters and scooters is operational, not distance-driven.

---

## 📊 Week 3: Visualization & Interpretation

All Week 3 visualizations and written interpretations are implemented in [`notebooks/03_visualization.ipynb`](notebooks/03_visualization.ipynb) using `cleaned_food_delivery.csv` (`N = 45,584`). Each chart includes clear axis units, actual calculated values, and a cautious 2–3 sentence interpretation with documented limitations.

1. **Delivery Duration Distribution — [`01_delivery_duration_distribution.png`](visualizations/01_delivery_duration_distribution.png)**
   - **Purpose:** Inspect the overall frequency distribution, central tendency, and tail behavior of delivery durations.
   - **Main Pattern:** 74.3% of orders cluster between 15 and 35 minutes (mean = `26.29 min`, median = `26.00 min`), with a mild right tail extending to 45–54 minutes (`3.96%` of orders).

2. **Traffic Comparison — [`02_traffic_comparison.png`](visualizations/02_traffic_comparison.png)**
   - **Purpose:** Compare median delivery duration across road traffic density levels (`Low`, `Medium`, `High`, `Jam`) without extreme-value distortion.
   - **Main Pattern:** Median duration rises from `20.0 min` (`Low`, `n=15,476`) to `27.0 min` (`Medium`/`High`) and `31.0 min` (`Jam`, `n=14,139`), an 11-minute gap associated with congestion (601 missing traffic rows excluded).

3. **Weather Comparison — [`03_weather_comparison.png`](visualizations/03_weather_comparison.png)**
   - **Purpose:** Evaluate average delivery duration across all six weather categories.
   - **Main Pattern:** `Cloudy` (`28.92 min`) and `Fog` (`28.91 min`) show the highest average durations, whereas `Sunny` (`21.86 min`) is roughly 7 minutes faster on average (616 missing weather rows excluded).

4. **Distance vs. Delivery Duration — [`04_distance_vs_duration.png`](visualizations/04_distance_vs_duration.png)**
   - **Purpose:** Assess the direction, strength, and unusual points in the relationship between straight-line Haversine distance and delivery duration (`n=41,944` valid GPS rows).
   - **Main Pattern:** Positive but weak-to-moderate correlation (`Pearson r = 0.320`, `r² = 0.103`, `Spearman ρ = 0.319`), showing that distance alone explains ~10.3% of duration variance and unusual short-slow / long-fast trips occur due to non-distance factors.

5. **Vehicle Comparison — [`05_vehicle_comparison.png`](visualizations/05_vehicle_comparison.png)**
   - **Purpose:** Compare delivery-duration spread, median, interquartile range (IQR), and outliers across vehicle types.
   - **Main Pattern:** `electric_scooter` and `scooter` share a `24.0 min` median (IQR = `13.0 min`, with 1 and 3 upper outliers > 49.5 min), compared to `26.0 min` for `motorcycle` (IQR = `14.0 min`) and `bicycle` (`n=68`, imbalanced sample size).

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
