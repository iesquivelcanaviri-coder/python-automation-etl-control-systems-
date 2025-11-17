PROJECT 2 — etl_temperature_pipeline

ETL Workflow for Temperature Logs
This project demonstrates an ETL pipeline that extracts, cleans, transforms, and loads temperature sensor data. It includes noise removal and anomaly detection logic relevant to industrial HVAC and cooling systems.

Features
• Extracts raw CSV data
• Converts values to numeric types
• Removes out-of-range sensor noise
• Flags anomalies for maintenance analysis
• Outputs a cleaned dataset

Files
etl_pipeline_temperature_logs.py
raw_temperature_logs.csv

Example Usage
python etl_pipeline_temperature_logs.py
Example Dataset (raw_temperature_logs.csv)
temperature
22
25
-15
75
61
18
20

etl_pipeline_temperature_logs.py
import pandas as pd

def extract(file_path):
    return pd.read_csv(file_path)

def transform(df):
    df = df.copy()
    df['temperature'] = df['temperature'].astype(float)
    df = df[(df['temperature'] > -40) & (df['temperature'] < 80)]
    df['anomaly'] = df['temperature'].apply(lambda x: 1 if x > 60 or x < 0 else 0)
    return df

def load(df, output_file="processed_temperature_logs.csv"):
    df.to_csv(output_file, index=False)
    print("Processed file saved:", output_file)

if __name__ == "__main__":
    raw = extract("raw_temperature_logs.csv")
    cleaned = transform(raw)
    load(cleaned)
