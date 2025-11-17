Automated Report Generator

This project automates data cleaning and generates a daily operational summary. It demonstrates skills in workflow automation, structured programming, and data handling, relevant to environments requiring reliable monitoring logic.

**Features**
• Loads a CSV dataset
• Cleans numerical values
• Computes metrics such as mean and min/max
• Saves a formatted report as a text file

**Files**
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



**automated_report_generator.py**
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
