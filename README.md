# Datawarehouse_hospital

# 🏥 Hospital Data Warehouse: Sepsis & Clinical Deterioration Monitoring
**Author:** Ola Abdallah, MBBS
## 📌 Project Overview
This project demonstrates the end-to-end construction of a modern clinical data warehouse using **SQL Server**. It transforms raw, disjointed hospital data (patient demographics, hourly vitals, and lab results) into a structured system designed to monitor patient stability, calculate sepsis risk, and detect rapid hemodynamic decline.
By applying the **Medallion Architecture (Bronze, Silver, Gold)**, this repository bridges the gap between raw healthcare data extraction and actionable clinical intelligence.
## 🏗️ Data Architecture (The Medallion Approach)
### 1. Bronze Layer (Raw Data Ingestion)
 * Ingested raw CSV files containing synthetic hospital records.
 * Retained data in its original, unoptimized state (VARCHAR formats, dirty strings) to serve as a historical baseline.
### 2. Silver Layer (Data Cleansing & Transformation)
 * **Data Type Casting:** Safely converted chaotic strings into strict numeric and float data types using TRY_CAST.
 * **String Scrubbing:** Removed hidden trailing spaces and non-standard characters from clinical metrics.
 * **Integrity Checks:** Handled missing values and replaced error codes (e.g., -1 flags) with standard NULL logic to prevent downstream analytical skew.
### 3. Gold Layer (Clinical Intelligence Views)
 * **master_patient_monitoring:** A unified fact/dimension view that joins patient demographics with their most recent hourly vitals and labs. Populates high-precision sepsis_risk_score metrics and active tachycardia flags.
 * **patient_early_warning_alerts:** An automated scoring system modeled after clinical Early Warning Scores (like NEWS2). It uses CASE WHEN logic to assign risk points across vital signs (tachycardia, bradycardia, hypoxia) and integrates the sepsis_risk_score. It classifies patients into actionable triage tiers ('GREEN: Stable', 'YELLOW: Monitor', 'RED ALERT: High Risk') for immediate nursing review.
 * **vitals_trend_analysis:** An advanced time-series view designed to act as an Early Warning System. It calculates the rate of change in systolic blood pressure over rolling 6-hour windows.
## 💻 Key SQL Engineering Skills Demonstrated
 * **Window Functions:** Utilized LAG() with PARTITION BY and ORDER BY to compare a patient's current vital signs against their own historical baseline.
 * **Advanced Error Handling:** Implemented COALESCE to neutralize baseline calculations during the first 5 hours of admission (preventing "ghost alerts") and NULLIF to eliminate fatal division-by-zero errors during critical drops.
 * **Common Table Expressions (CTEs):** Used multi-step CTEs to isolate logic, making the code modular, readable, and highly performant.
 * **Clinical Threshold Filtering:** Built dynamic mathematical filters to isolate patients experiencing a >20% drop in systolic blood pressure.
## 🚀 Business & Clinical Impact
In a real-world setting, the output of this data warehouse would feed directly into a nursing station dashboard or an automated alerting system. Instead of clinicians manually calculating trends across fragmented charts, the database automatically highlights patients who are compensating or entering shock, enabling faster, data-driven medical interventions.
