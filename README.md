# IoT Sensor Data Analysis with PySpark SQL

## Description
This project analyzes synthetic IoT sensor data using Apache Spark (PySpark). The data includes temperature and humidity readings from sensors placed across different building floors. The goal is to explore, filter, aggregate, rank, and pivot this data using Spark SQL and DataFrame APIs.

---

##  Technologies Used
- **Python**
- **Apache Spark (PySpark)**
- **Spark SQL**
- **CSV** (Input/Output)

---

## Files in the Repository

| File               | Description                                                           |
|--------------------|-----------------------------------------------------------------------|
| `data_generator.py`| Script to generate `sensor_data.csv` with 1000 records using Faker    |
| `sensor_data.csv`  | Sample IoT sensor dataset (can be generated using the script)         |
| `iot_sensor_data.py`| Complete analysis script that runs all 5 Spark SQL tasks             |
| `task1_output.csv` | Output for Task 1: Basic exploration result                           |
| `task2_output.csv` | Output for Task 2: Filtered and aggregated results by location        |
| `task3_output.csv` | Output for Task 3: Hourly average temperature                         |
| `task4_output.csv` | Output for Task 4: Top 5 sensors ranked by average temperature        |
| `task5_output.csv` | Output for Task 5: Pivoted table with average temperature per location/hour |

---

## Tasks Breakdown & Outputs

---

### Task 1: Load & Basic Exploration

**Goal:** Load and inspect the sensor dataset.

**Steps:**
- Load `sensor_data.csv` into Spark
- Create a temporary view `sensor_readings`
- Display:
  - First 5 records
  - Total number of records
  - Distinct locations

**Output File:** `task1_output.csv`
```
sensor_id,timestamp,temperature,humidity,location,sensor_type
1039,2025-04-04T13:52:16.000Z,19.71,75.47,BuildingA_Floor1,TypeC
1024,2025-04-06T12:46:03.000Z,34.13,36.07,BuildingA_Floor1,TypeC
1063,2025-04-07T17:23:15.000Z,27.13,42.27,BuildingB_Floor1,TypeA
```
---

### Task 2: Filtering & Aggregations

**Goal:** Identify temperature outliers and compute averages.

**Steps:**
- Filter records where `temperature` is **between 18–30°C** (in-range)
- Count in-range vs out-of-range
- Group by `location` to calculate:
  - Average `temperature`
  - Average `humidity`

**Output File:** `task2_output.csv`
```
location,avg_temperature,avg_humidity
BuildingB_Floor1,25.844703557312247,55.60403162055335
BuildingA_Floor2,25.243760330578517,54.622272727272716
BuildingA_Floor1,25.19927710843374,55.49389558232933
```
---

### Task 3: Time-Based Analysis

**Goal:** Understand hourly temperature trends.

**Steps:**
- Convert `timestamp` string to Spark timestamp type
- Extract the `hour_of_day` from each timestamp
- Group by hour and calculate the **average temperature**

**Output File:** `task3_output.csv`
```
hour_of_day,avg_temp
0,24.567241379310346
1,25.035555555555554
2,25.1104347826087
```
---

### Task 4: Sensor Ranking by Temperature

**Goal:** Identify the top 5 sensors with the highest average temperature.

**Steps:**
- Group by `sensor_id` and calculate `avg_temp`
- Use a **Window Function (`RANK()`)** to rank sensors in descending order
- Show top 5 sensors

**Output File:** `task4_output.csv`
```
sensor_id,avg_temp,rank_temp
1013,29.512222222222224,1
1073,28.481111111111115,2
1067,28.477500000000003,3
```
---

### Task 5: Pivot Table by Location and Hour

**Goal:** Visualize temperature variation by hour and location.

**Steps:**
- Use `location` as rows and `hour_of_day` (0–23) as columns
- Use **average temperature** as values
- Create a **pivot table**

**Output File:** `task5_output.csv`
```
location,0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23
BuildingA_Floor1,26.271111111111114,23.512222222222224,26.218461538461536,24.326666666666664,23.823636363636364,22.29714285714286,25.225555555555555,25.9225,26.551,24.365000000000002,23.69666666666667,24.653999999999996,26.389999999999997,25.294285714285714,28.670000000000005,25.094166666666666,23.352,25.15,23.0325,25.780833333333334,25.030909090909095,22.947499999999998,28.288181818181815,26.630833333333328
BuildingA_Floor2,24.382,27.27692307692308,20.727999999999998,23.533333333333335,24.737272727272725,22.073333333333334,25.573076923076925,25.381538461538465,23.30818181818182,25.346666666666668,28.640909090909098,30.28125,24.648,26.08,21.466,25.09625,25.920714285714286,25.701666666666668,23.961666666666662,24.215555555555557,23.34545454545454,28.08888888888889,29.938,25.757142857142856
```
---
## Running the Project

### 1. Install Dependencies
Install the necessary Python libraries:
```bash
pip install pyspark faker