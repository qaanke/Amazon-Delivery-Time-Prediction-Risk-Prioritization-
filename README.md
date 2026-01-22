# 📦 Amazon Delivery Prediction & Risk Prioritization System

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python)
![LightGBM](https://img.shields.io/badge/Model-LightGBM-green?style=for-the-badge&logo=google)
![Folium](https://img.shields.io/badge/Viz-Geospatial_Analytics-orange?style=for-the-badge&logo=leaflet)
![Status](https://img.shields.io/badge/Status-Production_Ready-success?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge)

> **An End-to-End Decision Support System (DSS) designed to address operational uncertainty in last-mile logistics.**

---

## 🎯 Project Vision: Why this exists?

In Last-Mile Logistics, **averages are misleading.** Knowing that a delivery takes "30 minutes on average" is useless when a sandstorm hits or traffic jams occur.

This project is not just a time prediction model; it is a **Risk Management Tool**. It shifts the focus from *"When will it arrive?"* to *"Which orders are about to fail?"*.

**Core Value Proposition:**
1.  **Dynamic Prediction:** Uses Machine Learning to predict delivery time based on real-time conditions (Weather, Traffic, Vehicle Type).
2.  **Risk Quantification:** Converts predictions into a `Priority Score` to rank orders by urgency.
3.  **Visual Intelligence:** Plots high-risk bottlenecks on an interactive map for immediate operational intervention.

---

## 🛠️ Engineering Decisions & Critical Methodology

Unlike standard data science projects, every step here was taken with **production reality** in mind.

### 1. 🛑 Prevention of Data Leakage (Temporal Splitting)
* **The Problem:** In many tutorials, data is split randomly (`train_test_split`). In logistics, this is wrong because you cannot use "tomorrow's delivery" to predict "today's traffic."
* **My Solution:** I implemented a **Time-Based Split** strategy. The model is trained strictly on past data to predict future orders, simulating a real-world deployment environment.

### 2. ⚡ Model Selection: Why LightGBM?
After benchmarking against XGBoost and Random Forest, **LightGBM** was chosen for two critical reasons:
* **Categorical Handling:** Logistics data is heavy on categories (e.g., `Weather_conditions: Stormy`, `Road_traffic_density: Jam`). LightGBM handles these natively without the memory explosion of One-Hot Encoding.
* **Inference Speed:** It offers up to 10x faster training and prediction speeds, which is crucial for real-time dispatch systems.

### 3. 📐 Feature Engineering (Domain Knowledge)
Raw data is rarely enough. I engineered specific features to capture logistical reality:
* **Haversine Distance:** Calculated the geodesic distance between warehouse and destination coordinates.
* **Temporal Segments:** Created `Order_Period` buckets (Morning, Afternoon, Late Night) to capture human behavior and traffic patterns.

---

## ⚠️ The Risk Algorithm (The "Brain" of the System)

A raw prediction (e.g., "45 mins") is not actionable. I developed a heuristic **Weighted Risk Formula** to translate time into urgency.

$$\text{Priority Score} = (\text{Norm. Prediction} \times \alpha) + (\text{Traffic Penalty} \times \beta) + (\text{Weather Penalty} \times \gamma)$$

* **Logic:** A motorcycle courier in a "Sandstorm" is assigned a significantly higher risk multiplier than a van in "Sunny" weather, even if the distance is identical.
* **Result:** The system flags these "silent risks" that simple distance calculations miss.

---

## 📂 Project Architecture

The repository is structured to separate data processing, modeling, and visualization for scalability.

```bash
├── data/
│   ├── raw/                 # Original dataset
│   └── processed/           # Cleaned data, features, and risk scores
├── models/
│   ├── lightgbm_delivery.pkl # Serialized trained model
│   └── features_used.csv     # Feature schema for reproducibility
├── notebooks/
│   ├── 01_eda_and_feature_engineering.ipynb  # Data cleaning & bottleneck analysis
│   ├── 02_modelling.ipynb                    # LightGBM training (Time-based split)
│   ├── 03_risk_analysis_and_routing.ipynb    # Priority Algorithm & Decision Logic
│   └── 04_visualization.ipynb                # Dashboard generation
├── visualizations/
│   ├── priority_map.html    # INTERACTIVE DASHBOARD (Open in Browser)
│   └── priority_table.csv   # Ranked list of high-risk orders
├── requirements.txt         # Dependencies
├── LICENSE                  # MIT License
└── README.md                # Documentation


## 👏 Acknowledgements & Disclaimer

Dataset: Amazon Delivery Dataset by Sujal Suthar (Kaggle).

License: This project is open-sourced under the MIT License.

Note: This is a portfolio project demonstrating Operations Research and Data Science methodologies.
