# MetroScan: NYC Subway Ridership Analysis and Delay Detection

This project was developed as part of CS GY 6513 – Big Data at NYU. It analyzes historical subway ridership data using PySpark to detect patterns, identify anomalies, model usage trends, and cluster stations by behavior.

## How to Run

### 1. Setup Spark in Google Colab

```bash
!apt-get install openjdk-11-jdk-headless -qq > /dev/null
!wget -q https://archive.apache.org/dist/spark/spark-3.3.2/spark-3.3.2-bin-hadoop3.tgz
!tar xf spark-3.3.2-bin-hadoop3.tgz
!pip install -q findspark
```


### 2. Initialize Spark and Load Data
```
import os  
import findspark

os.environ["JAVA_HOME"] = "/usr/lib/jvm/java-11-openjdk-amd64"  
os.environ["SPARK_HOME"] = "/content/spark-3.3.2-bin-hadoop3"  
findspark.init()

from pyspark.sql import SparkSession  
spark = SparkSession.builder\  
    .master("local[*]")\  
    .appName("MetroScan")\  
    .getOrCreate()

from google.colab import drive  
drive.mount('/content/drive')

df = spark.read.parquet(  
    "/content/drive/MyDrive/BD_Project/mta_hourly_features.parquet"  
)
```

## Features

### Ridership Trend Analysis
- Daily and 7-day rolling average plots
- Monthly ridership summaries
- Hour-of-day patterns segmented by weekday/weekend
- Top 20 stations by total ridership
- Hour vs. weekday heatmaps

### Anomaly Detection
- 24-period rolling average per station
- Flags ridership below 30% of moving average as anomalies
- Time series visualizations for selected stations

### Predictive Modeling
- Feature set includes: time fields, geography, fare type, payment method, etc.
- Models trained using Spark MLlib:
  - Linear Regression
  - Random Forest
  - Gradient Boosted Trees (GBT)
- Evaluation metric: RMSE

### Station Clustering and PCA
- KMeans clustering on 24-hour ridership profiles
- PCA reduction for 2D visualization
- Identified four station cluster types:
  - Downtown Core
  - Residential Hubs
  - Sparse Stations
  - Transfer Points

---

## Results Summary

| Model                  | RMSE    |
|------------------------|---------|
| Linear Regression      | 117.45  |
| Random Forest          | 114.48  |
| Gradient Boosted Trees | 104.12  |

---

## Technologies Used

- Python 3.11  
- Google Colab  
- PySpark 3.3.2 (Hadoop 3)  
- Spark MLlib  
- Matplotlib, Seaborn  
- Parquet format for data storage  

---

## Authors

- **Rohan Subramaniam** – rs1234@nyu.edu  
- **Nishanth Ganapathy Palaniswamy** – ng3124@nyu.edu  
- **Akash R** – ar8974@nyu.edu
  
---

## Resources

- NYC MTA Open Data: [https://www.mta.info/open-data]
- MTA Subway hourly ridership dataset: [https://data.ny.gov/Transportation/MTA-Subway-Hourly-Ridership-2020-2024/wujg-7c2s/about_data]
