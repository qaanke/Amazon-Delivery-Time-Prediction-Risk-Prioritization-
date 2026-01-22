# 📦 Amazon Delivery Prediction & Risk Prioritization System

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python)
![LightGBM](https://img.shields.io/badge/Model-LightGBM-green?style=for-the-badge&logo=google)
![Folium](https://img.shields.io/badge/Viz-Folium_Maps-orange?style=for-the-badge&logo=leaflet)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge)

> **An End-to-End Decision Support System (DSS) designed to address operational uncertainty in last-mile logistics.**

---

## 🌍 Executive Summary

This project tackles a fundamental challenge in supply chain management: **Last-Mile Delivery Uncertainty.** Instead of simply predicting *when* a package will arrive, this system functions as a proactive **Risk Management Tool**.

By leveraging historical delivery data, I engineered a system that:
1.  **Predicts** delivery durations using Gradient Boosting (LightGBM).
2.  **Calculates** a custom `Priority Score` based on environmental factors (Traffic, Weather).
3.  **Visualizes** high-risk orders on an interactive geospatial dashboard to enable dynamic rerouting.

---

## 🚀 Key Technical Highlights

### 1. 🧠 Prevention of Data Leakage (Time-Based Splitting)
Unlike standard academic projects that use random shuffling, this project respects the temporal nature of logistics.
* **Method:** Data is split chronologically based on `_timestamp`.
* **Impact:** Ensures the model is trained only on "past" data to predict "future" outcomes, simulating a real-world production environment.

### 2. ⚠️ Custom Risk Scoring Algorithm
A raw prediction (e.g., "45 mins") is insufficient for decision-making. I developed a heuristic **Weighted Risk Formula** to quantify urgency:

$$\text{Priority Score} = (\alpha \times \text{Prediction}) + (\beta \times \text{Traffic Penalty}) + (\gamma \times \text{Weather Penalty})$$

* **Logic:** A motorcycle courier in a "Sandstorm" is assigned a higher risk multiplier than a van in "Sunny" weather, even if the distance is the same.

### 3. 🗺️ Geospatial Intelligence
The final output is not a static report but an **Interactive HTML Map** (via Folium).
* **Cluster Analysis:** Identifies geographic bottlenecks where delays occur frequently.
* **Visual Cues:** High-risk orders appear as large, red markers; low-risk orders are small, green markers.

---

## 📂 Project Architecture

The repository follows a modular software engineering structure for reproducibility:

```bash
├── data/
│   ├── raw/                 # Original dataset
│   └── processed/           # Cleaned data, features, and risk scores
├── models/
│   ├── lightgbm_delivery.pkl # Serialized trained model
│   └── features_used.csv     # Feature schema
├── notebooks/
│   ├── 01_eda_and_feature_engineering.ipynb  # Data cleaning & bottleneck analysis
│   ├── 02_modelling.ipynb                    # LightGBM training (Time-based split)
│   ├── 03_risk_analysis_and_routing.ipynb    # Priority Algorithm & Decision Logic
│   └── 04_visualization.ipynb                # Dashboard generation
├── visualizations/
│   ├── priority_map.html    # INTERACTIVE DASHBOARD (Open this file in browser)
│   └── priority_table.csv   # Ranked list of high-risk orders
├── requirements.txt         # Project dependencies
├── LICENSE                  # MIT License
└── README.md                # Project Documentation
