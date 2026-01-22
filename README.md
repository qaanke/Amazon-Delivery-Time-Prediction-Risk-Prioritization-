# 📦 Amazon Delivery Prediction & Risk Prioritization System

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python)
![LightGBM](https://img.shields.io/badge/Model-LightGBM-green?style=for-the-badge&logo=google)
![Folium](https://img.shields.io/badge/Viz-Geospatial_Analytics-orange?style=for-the-badge&logo=leaflet)
![Status](https://img.shields.io/badge/Status-Production_Ready-success?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge)

> **An End-to-End Decision Support System (DSS) designed to address operational uncertainty in last-mile logistics.**

---

## 1. 🌍 Project Summary

**The Problem:** In logistics, average delivery times are misleading. Standard models predict *when* a package arrives but fail to identify *which* deliveries are at risk of failure due to dynamic factors like traffic jams or severe weather.

**The Solution:** A holistic system that:
1.  **Predicts** exact delivery duration using Gradient Boosting (LightGBM).
2.  **Quantifies Risk** via a custom `Priority Score` algorithm.
3.  **Visualizes** bottlenecks on an interactive map for proactive intervention.

**The Output:** A ranked list of high-risk orders (`smart_routing.csv`) and an interactive geospatial dashboard (`priority_map.html`) for operations managers.

---

## 2. 💾 Dataset & Inputs

* **Source:** [Amazon Delivery Dataset](https://www.kaggle.com/datasets/sujalsuthar/amazon-delivery-dataset) via Kaggle (Sujal Suthar).
* **Target Variable:** `Time_taken(min)` (Regression Task).
* **Key Inputs:**
    * **Agent Info:** Age, Rating.
    * **Context:** Weather Conditions, Road Traffic Density.
    * **Logistics:** Vehicle Type, Order Date/Time.
    * **Geospatial:** Pickup & Delivery Coordinates (Lat/Lon).

> *Note: Raw data is located in `data/raw/`. If missing, please download via the Kaggle link above.*

---

## 3. ⚙️ Pipeline Architecture

The project follows a linear, modular workflow designed for reproducibility: 

[Image of machine learning pipeline diagram]


1.  **Cleaning & EDA:** Handling missing values, formatting dates, and analyzing distributions (`01_eda...`).
2.  **Feature Engineering:** Creating geospatial and temporal features.
3.  **Modeling:** Training LightGBM with a time-based split strategy (`02_modelling...`).
4.  **Risk Scoring:** Calculating `Priority Score` based on model variance and external risks (`03_risk...`).
5.  **Smart Routing & Viz:** Generating the final decision support map (`04_visualization...`).

---

## 4. 📐 Feature Engineering Highlights

Raw data was transformed into actionable signals:

* **📍 Haversine Distance:** Calculated the geodesic distance (km) between warehouse and delivery points using coordinates. 
* **🕒 Temporal Buckets:** Created `Order_Period` (Morning, Afternoon, Evening, Late Night) to capture traffic patterns and human fatigue factors.
* **🚦 Categorical Handling:** Optimized for LightGBM's native categorical support (avoiding sparse One-Hot matrices for High Cardinality data).
* **📉 Outlier Management:** Capped extreme delivery times based on IQR logic to prevent model skewing.

---

## 5. 🚀 Modeling & Results

### Methodology
* **Split Strategy:** Used **Time-Based Splitting** (sorting by date) instead of random `train_test_split`. This prevents data leakage by ensuring the model trains on "past" data to predict "future" orders.
* **Model Selection:** **LightGBM** was chosen over Random Forest and XGBoost due to its superior speed (10x faster inference) and native handling of categorical logistics variables.

### Performance Metrics
* **RMSE (Root Mean Squared Error):** Optimized to minimize minute-level deviations.
* **R² Score:** High variance explanation capabilities.
* **Feature Importance:** The model identified **Traffic Density**, **Vehicle Type**, and **Weather** as the top drivers of delay, validating the business logic.

---

## 6. ⚠️ Risk & Priority Score Definition

A raw prediction (e.g., "45 mins") is not actionable. I engineered a heuristic logic to translate "Time" into "Urgency". 

[Image of risk matrix heatmap]


### A. Risk Labels (Prototype / Offline)
Labels are derived from the Absolute Error between prediction and actual time:
* **Low Risk:** Deviation < 15 mins
* **Medium Risk:** 15 mins < Deviation < 30 mins
* **High Risk:** Deviation > 30 mins

### B. Priority Score Formula (Production)
A weighted formula to rank orders dynamically:

$$\text{Priority Score} = (\text{Norm. Prediction} \times \alpha) + (\text{Traffic Penalty} \times \beta) + (\text{Weather Penalty} \times \gamma)$$

* *Parameters ($\alpha, \beta, \gamma$) are tunable based on the company's SLA requirements.*
* **Logic:** A motorcycle in a "Sandstorm" gets a higher priority score than a van in "Sunny" weather, even if the distance is shorter.

---

## 7. 📂 Key Outputs

The system generates the following artifacts:

* **`models/lightgbm_delivery.pkl`**: The trained, serialized model ready for inference.
* **`models/features_used.csv`**: Schema file ensuring new data matches training columns.
* **`data/processed/smart_routing.csv`**: The final operational file containing predictions and risk scores.
* **`visualizations/priority_map.html`**: **The MVP Product.** An interactive map showing high-risk clusters. 
* **`visualizations/priority_table.csv`**: A ranked list of orders requiring immediate attention.

---

## 8. 🗺️ How to Use the Map

1.  Clone the repository and run the notebooks.
2.  Navigate to the `visualizations/` folder.
3.  Double-click **`priority_map.html`**.
4.  The dashboard will open in your default web browser (Chrome/Edge/Safari).
    * **🔴 Red Markers:** High Priority / High Risk Orders.
    * **🟢 Heatmap:** General order density.
    * **Click Markers:** View details like Order ID, Predicted Time, and Traffic Status.

---

## 9. 🔮 Limitations & Next Steps

To evolve this from a generic portfolio project to an enterprise solution:

* **Probabilistic Forecasting:** Move from point prediction to `P(delay > 30 min)` using Quantile Regression.
* **Weight Calibration:** Use AHP (Analytic Hierarchy Process) or optimization to scientifically determine $\alpha, \beta, \gamma$ weights.
* **VRP Integration:** Feed the risk scores into a Vehicle Routing Problem solver (e.g., Google OR-Tools) to auto-generate optimized routes.
* **Deployment:** Containerize the pipeline using Docker and create a CLI for batch processing.

---

## 👤 Author

**Halil Kaan Kaçar**
* **Role:** Industrial Engineering Student & Logistics Optimization Enthusiast
* **Focus:** Supply Chain Analytics, Machine Learning, Operations Research
* **Connect:** [LinkedIn](https://www.linkedin.com/in/halil-kaan-kacar-59b366258/)

---
*Copyright (c) 2026 Halil Kaan Kaçar. Distributed under the MIT License.*
