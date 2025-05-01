# 🏢 Building Energy Efficiency Analysis Using Linear Regression

**Predicting and Understanding Energy Use Intensity (EUI) in Singapore's Commercial Buildings**
> with data provided by: [Data.gov.sg | Building and Construction Authority (BCA)](https://data.gov.sg/collections/22/datasets/d_e86d8a219d0936dbb321ade068a381da/view)

    Data Science Midterm Coursework

`Data Processing · EDA · Data Visualization · Feature Engineering · Linear Regression · Sustainability Insights`

[![Python](https://img.shields.io/badge/Made%20with-Python-blue?logo=python&logoColor=white)](https://www.python.org/)  
[![License: SGODL](https://img.shields.io/badge/Data%20License-Singapore%20Open%20Data%20License-green)](https://beta.data.gov.sg/)

---

## 📌 Overview

This project aims to support sustainable building practices by analyzing the **Energy Use Intensity (EUI)** of commercial buildings in Singapore. Using a cleaned and preprocessed dataset of office and retail buildings, a **linear regression model** is developed to investigate key factors influencing energy consumption.

> **Goal:** Offer practical insights to building managers, policymakers, and stakeholders to optimize energy usage and reduce carbon footprint.

---

## 💡 Objectives

- **EUI Modeling**: Develop a regression model to understand and predict Energy Use Intensity.
- **Feature Impact**: Quantify how features like chiller age, AC efficiency, solar panel usage, etc., influence energy use.
- **Sustainability Insights**: Offer actionable recommendations for improving building energy performance.

---

## 🧾 Dataset
> This dataset contains the building energy performance data collected through BCA’s Building Energy Submission System (BESS), under the legislation on Annual Mandatory Submission of Building Information and Energy Consumption Data for Section 22FJ ‘Powers to Obtain Information’ of Building Control Act.
>
> Note: The data are declared by building owners and might be updated from time to time.
- **Source**: [Listing of Building Energy Performance Data 2020](https://data.gov.sg/collections/22/datasets/d_e86d8a219d0936dbb321ade068a381da/view)
- **Size**: 564 entries × 23 features  
- **Features**:
  - Building size and function (Office, Retail, Hotel, etc.)
  - Green Mark certification details
  - HVAC system types and efficiency
  - Percentage of LED usage and solar panel installations
  - Energy Use Intensity (2017–2020)

> **Sample Dataframe**
> - 20 columns, exluding target: 2018-20 EUI 
<small>
  
  |index|buildingname|buildingaddress|buildingtype|mainbuildingfunction|buildingsize|yearobtainedtopcsc|greenmarkrating|greenmarkyearofaward|greenmarkversion|grossfloorarea|percentageofairconditionedfloorarea|averagemonthlybuildingoccupancyrate|numberofhotelrooms|typeofairconditioningsystem|ageofchiller|centralisedairconditioningplantefficiency|yearoflastchillerplantaudithealthcheck|percentageusageofled|installationofsolarpv|2017|
  |---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
  |0|UNITED SQUARE|101 THOMSON ROAD, SINGAPORE 307591|Commercial Building|Mixed Development|Large|1984|NaN|NaN|NaN|65,947|93%|90%|NaN|Water Cooled Chilled Water Plant|12\.0|0\.68|2019\.0|2%|N|281\.32|
  |1|HPL HOUSE|50 CUSCADEN ROAD, SINGAPORE 249724|Commercial Building|Office|Small|1980|NaN|NaN|NaN|7,372|97%|90%|NaN|Others|2\.0|NaN|NaN|80%|N|277\.93|

</small>

> Complete Dataframe
> - 23 columns
  <img width="1200" alt="df:1" src="https://github.com/user-attachments/assets/6682afbd-83fe-4a3b-876d-b639f023e961" />
  <img width="1200" alt="df:2" src="https://github.com/user-attachments/assets/dd067a6b-ef0f-4be7-8d5a-2c4355b72982" />


---

## 🔧 Data Preparation

| Step | Description |
|------|-------------|
| 🧹 Cleaning | Removed irrelevant columns, handled missing values |
| 🔍 Outlier Removal | AC efficiency outliers identified via boxplots |
| 📐 Imputation | Median-based imputation for skewed features |
| 🔄 Encoding | One-hot encoded categorical features |
| 📏 Normalization | Scaled numerical features using `StandardScaler` |

> Example: **Outlier Removal**
>
> Before:  
> <p align="center">
>  <img src="https://github.com/user-attachments/assets/b0efa161-941a-4d95-a2e6-4177e3b25187" alt="boxplot-before" width="800"/>
> </p>
> After: 
> <p align="center">
>  <img src="https://github.com/user-attachments/assets/ea2e6a4d-da4e-418e-95a9-0f81aa7ea815" alt="boxplot-after" width="800"/>
> </p>
*Accuracy in range distribution improves model reliability*

---

## 📈 Exploratory Data Analysis (EDA)

### 🔠 Feature Distributions
- Right-skewed distribution in AC efficiency and chiller age
- High LED usage and recent chiller audits positively associated with lower EUI
> Example: Numerical data - **Statistical analysis**
> <p align="center">
>  <img src="https://github.com/user-attachments/assets/5546cdb9-b8a1-4bfa-b423-ad79208e8ecd" alt="heatmap" width="800"/>
> </p>
*Visualizing distributions*

### 🧩 Correlations
| Feature | Correlation with EUI Decrease (2018–2020) |
|---------|-------------------------------------------|
| Solar PV Installed | **+0.045** |
| Central AC Efficiency | **+0.031** |
| Chiller Age | **-0.015** |
| LED Usage | **-0.024** |
| District Cooling | **-0.058** |

> Heatmap
> <p align="center">
>  <img src="https://github.com/user-attachments/assets/32ef51f9-934f-482a-878e-7297501ea460" alt="heatmap" width="800"/>
> </p>
*Visualizing correlation among key variables*


---

## 🔍 Target Group Analysis

Focus was narrowed to **Office and Retail buildings** with:
- High occupancy rates
- High percentage of air-conditioned floor area

> Target Scatter Plot 
>  <p align="center">
>    <img src="https://github.com/user-attachments/assets/e56f368e-e934-41c8-8cd5-59721f28c2f5" alt="target-scatter" width="800"/>
>  </p>
- From **2018-2020**: due to pronounced decline observed
> Median EUI across Years 
> <p align="center">
>  <img src="https://github.com/user-attachments/assets/897b1850-d361-405e-81ba-19899fcc63fc" alt="eui-med-time" width="800"/>
> </p>

---

## 🧠 Model: Linear Regression

| Step | Details |
|------|---------|
| 🎯 Target | EUI difference between 2018 and 2020 |
| 🧾 Features | AC system type, chiller age, LED usage, AC efficiency, audit year, solar PV |
| 🔬 Approach | Scikit-learn Linear Regression |
| 📉 Evaluation | R-squared and residual error analysis |

> *to be included: results summary, graph of predictions vs. actuals*

---

## ✅ Key Takeaways

- **Water-Cooled AC systems** and **solar panels** are moderately linked to reduced EUI.
- **Recent audits** and **high LED usage** suggest minor improvements in efficiency.
- Despite data limitations, meaningful trends were identified for targeted sustainability improvements.

---

## 📁 Repository Structure

```
├── data/
│   └── ListingofBuildingEnergyPerformanceData2020.csv
├── notebooks/
│   └── EUI_Data_Science.ipynb
├── README.md
└── requirements.txt
```

---

## 📚 References
- Singapore's open data portal | BCA
> Building and Construction Authority, 2024, "Listing of Building Energy Performance Data 2020", data.gov.sg. Accessed: May 1, 2025. [Online]. Available: https://data.gov.sg/datasets/d_e86d8a219d0936dbb321ade068a381da/view 
- [ENERGY STAR: Understanding EUI](https://www.energystar.gov/buildings/benchmark/understand_metrics/what_eui)
- [URA Postal District Mapping](https://www.ura.gov.sg/Corporate/-/media/Corporate/Property/PMI-Online/List_Of_Postal_Districts.pdf)

---

## ✨ Future Work

- Integrate `data.gov.sg` API for automated, reproducible data access
- Visualize feature–target relationships to assess linearity and improve model interpretability
- Improve model performance using ensemble or tree-based methods (e.g., Random Forest)
- Expand dataset across more years and regions
- Analyze causality using time-series forecasting or intervention analysis

