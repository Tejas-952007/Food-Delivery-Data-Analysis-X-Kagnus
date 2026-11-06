# Raw Dataset Documentation: Food Delivery Dataset

## Dataset Overview
- **File Name**: `Food_delivery_Dataset.csv`
- **Provided by**: Kagnus (Internship Task Brief - Week 1)
- **Domain**: Food Delivery Logistics and Operational Analytics
- **Total Records**: 45,584 rows
- **Total Attributes**: 20 columns
- **Original Format**: CSV (Comma Separated Values)

## Dataset Description
This dataset captures granular operational records of food delivery orders across multiple metropolitan and urban cities in India. Each record represents a single unique food delivery transaction, tracking timestamps, delivery executive demographics, ratings, geolocation coordinates (restaurant and customer drop-off), traffic conditions, weather conditions, order characteristics, vehicle types, and final delivery duration.

## Columns & Data Dictionary
| Column Name | Raw Data Type | Description |
| :--- | :--- | :--- |
| `ID` | object | Unique alphanumeric identifier for each order transaction (e.g., `0xcdcd`). |
| `Delivery_person_ID` | object | Unique alphanumeric identifier for the delivery partner (e.g., `DEHRES17DEL01`). |
| `Delivery_person_Age` | float64 | Age of the delivery partner in years (contains missing values). |
| `Delivery_person_Ratings` | float64 | Average rating of the delivery partner on a scale of 1.0 to 5.0 (outlier 6.0 detected). |
| `Restaurant_latitude` | float64 | Latitude coordinate of the restaurant (contains 0.0 placeholders and negative sign errors). |
| `Restaurant_longitude` | float64 | Longitude coordinate of the restaurant (contains 0.0 placeholders and negative sign errors). |
| `Delivery_location_latitude` | float64 | Latitude coordinate of the delivery drop location (contains 0.01 placeholders). |
| `Delivery_location_longitude` | float64 | Longitude coordinate of the delivery drop location (contains 0.01 placeholders). |
| `Order_Date` | object | Date the order was placed (DD-MM-YYYY format). |
| `Time_Orderd` | object | Timestamp when the order was placed (contains missing values and Excel decimal time serials). |
| `Time_Order_picked` | object | Timestamp when the delivery partner picked up the order from the restaurant. |
| `Weather_conditions` | object | Atmospheric weather condition during delivery (e.g., Sunny, Fog, Stormy, Sandstorms, Windy, Cloudy). |
| `Road_traffic_density` | object | Traffic density level during delivery (Low, Medium, High, Jam). |
| `Vehicle_condition` | int64 | Numerical index representing the mechanical health of the delivery vehicle (0 to 3). |
| `Type_of_order` | object | Category of order items (Snack, Meal, Drinks, Buffet). |
| `Type_of_vehicle` | object | Vehicle utilized for the delivery (motorcycle, scooter, electric_scooter, bicycle). |
| `multiple_deliveries` | float64 | Count of concurrent orders handled by the delivery person in the same trip (0 to 3). |
| `Festival` | object | Binary indicator whether the order occurred during a major festive period (`Yes` / `No`). |
| `City` | object | Type of urban agglomeration (`Metropolitian`, `Urban`, `Semi-Urban`). |
| `Time_taken (min)` | int64 | Target variable representing total delivery time in minutes from order to doorstep. |

## Raw Data Preservation Policy
In strict compliance with data integrity protocols:
1. The raw dataset (`Food_delivery_Dataset.csv`) remains completely untouched and unmodified.
2. All data cleaning transformations are executed on copies and written to `data/processed/cleaned_food_delivery.csv`.
3. An audit trail of all anomalies, rows affected, and cleaning rationales is maintained in `docs/cleaning_log.csv`.
