Python Engineering Projects

This repository contains three Python projects demonstrating automation, ETL workflows, and control-system logic simulation. They were designed to reflect a structured engineering mindset and to support applications for roles involving control systems, automation, HVAC, and data-driven engineering workflows.

Repository Contents

Automation Script
Automated report generator that cleans data, performs calculations, and produces daily monitoring summaries.

ETL Temperature Pipeline
End-to-end ETL pipeline for temperature sensor logs, including anomaly detection. Reflects typical preprocessing required for cooling-system monitoring.

Cooling System Controller Simulation
A Python simulation modelling simple decision logic used in cooling and HVAC control systems.

Each project includes:
• Python scripts
• Sample datasets
• Documentation

PROJECT 1 — automation_script/README.md
Automated Report Generator

This project automates data cleaning and generates a daily operational summary. It demonstrates skills in workflow automation, structured programming, and data handling, relevant to environments requiring reliable monitoring logic.

Features

• Loads a CSV dataset
• Cleans numerical values
• Computes metrics such as mean and min/max
• Saves a formatted report as a text file

Files

automated_report_generator.py

input_data.csv

Example Usage
python automated_report_generator.py

Example Dataset (input_data.csv)
value
10
20
15
30
25

automated_report_generator.py
import pandas as pd
from datetime import datetime

def load_data(file_path):
    try:
        return pd.read_csv(file_path)
    except FileNotFoundError:
        print("Input file not found.")
        return None

def clean_data(df):
    df = df.copy()
    df.dropna(subset=['value'], inplace=True)
    df['value'] = df['value'].astype(float)
    return df

def generate_report(df):
    return {
        "timestamp": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
        "total_records": len(df),
        "mean_value": df['value'].mean(),
        "max_value": df['value'].max(),
        "min_value": df['value'].min()
    }

def save_report(report):
    with open("daily_report.txt", "w") as f:
        for k, v in report.items():
            f.write(f"{k}: {v}\n")
    print("Report generated: daily_report.txt")

if __name__ == "__main__":
    df = load_data("input_data.csv")
    if df is not None:
        df = clean_data(df)
        summary = generate_report(df)
        save_report(summary)



PROJECT 2 — etl_temperature_pipeline/README.md
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

📁 PROJECT 3 — cooling_system_simulation/README.md
Cooling System Controller Simulation

A Python simulation of simple control logic used in HVAC and cooling systems. It reflects an engineering understanding of system states, thresholds, and decision rules relevant to industrial control systems.

Features

• Monitors temperature readings
• Switches states based on thresholds
• Simulates controller response

Files

cooling_system_controller_sim.py

Example Usage
python cooling_system_controller_sim.py


cooling_system_controller_sim.py
class CoolingSystemController:
    def __init__(self, min_temp=16.0, max_temp=22.0):
        self.min_temp = min_temp
        self.max_temp = max_temp
        self.state = "IDLE"

    def evaluate_temperature(self, temperature):
        if temperature > self.max_temp:
            self.state = "COOLING_ON"
        elif temperature < self.min_temp:
            self.state = "COOLING_OFF"
        else:
            self.state = "STABLE"
        return self.state

    def simulate(self, readings):
        results = []
        for temp in readings:
            status = self.evaluate_temperature(temp)
            results.append((temp, status))
        return results

if __name__ == "__main__":
    readings = [18.2, 23.5, 21.0, 15.1, 19.5]
    controller = CoolingSystemController()
    for temp, status in controller.simulate(readings):
        print(f"Temperature {temp} -> System State: {status}")
