# Power Plant Efficiency Optimization using Machine Learning

![Python](https://img.shields.io/badge/Python-3.7%2B-blue.svg)
![Pandas](https://img.shields.io/badge/Pandas-1.x-blue.svg)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.x-orange.svg)
![XGBoost](https://img.shields.io/badge/XGBoost-1.x-green.svg)
![Optuna](https://img.shields.io/badge/Optuna-3.x-purple.svg)

## 1. Overview

This project demonstrates an end-to-end machine learning workflow to optimize the operational efficiency of a coal-fired power plant. The goal was to create a "digital twin" of the plant's operations to identify novel settings that could **increase electrical power output (MW)** while simultaneously **decreasing coal consumption**.

The project successfully identified a new set of operational parameters predicted to deliver a **~9.5% increase in power output** while **reducing coal flow by ~6.0%**, representing a significant improvement in fuel efficiency and operational profitability.

---

## 2. The Business Problem

In thermal power plants, maximizing power generation while minimizing fuel consumption is the primary driver of profitability and sustainability. While it's known that more coal generally produces more power, operating at maximum fuel input is often inefficient and unsustainable due to equipment stress.

The challenge was to move beyond this simple relationship and find a more harmonious balance between over 100 different operational inputs (damper positions, air flow, temperatures, etc.) to achieve peak efficiency at a sustainable fuel rate. The typical operation yielded ~635 MW from a coal flow of ~346 units. The goal was to significantly improve this ratio.

---

## 3. Methodology

The project was executed in a four-stage pipeline:

#### Stage 1: Data Preparation & Feature Engineering
- An initial dataset of 104 operational parameters was processed.
- To create more meaningful inputs for the model, 23 new features were engineered by:
    - **Aggregating** related sensor data (e.g., calculating the `mean`, `std`, and `range` of all coal burner damper positions).
    - **Calculating** critical process values like temperature differentials (`delta-T`) and the air-to-fuel ratio.
- This resulted in a robust set of 47 predictive features.

#### Stage 2: Model Training & Selection
- Two models were evaluated for their ability to predict the Generator MW output: Random Forest and XGBoost.
- The XGBoost model demonstrated superior performance, capturing the plant's complex, non-linear dynamics with higher accuracy.

#### Stage 3: Hyperparameter Tuning
- To maximize the predictive power of the XGBoost model, the **Optuna** framework was used to perform automated hyperparameter tuning.
- Optuna intelligently searched through hundreds of combinations of model settings (`n_estimators`, `max_depth`, `learning_rate`, etc.) to find the optimal configuration, resulting in a highly accurate predictive model.

#### Stage 4: Multi-Objective Optimization
- With a reliable "digital twin" of the plant, Optuna was used again to solve the core business problem.
- An objective function was defined to find the optimal combination of the **104 original, controllable input parameters**.
- The goal was to find settings that **maximized the predicted MW** while keeping the **predicted Coal Flow below a target of -5%** from the baseline average.

---

## 4. Key Results

The project achieved its objectives, delivering a fine-tuned model and a set of actionable, optimized operational parameters.

#### A. Model Performance

The tuned XGBoost model was significantly more accurate than the baseline Random Forest.

| Model             | MAE    | RMSE   | R-squared |
| :---------------- | :----- | :----- | :-------- |
| Random Forest     | 0.4910 | 0.6882 | 0.8212    |
| **Tuned XGBoost** | **0.4187** | **0.5645** | **0.8797** |

#### B. Operational Optimization Outcome

The optimization process identified a novel set of operating parameters that are predicted to significantly improve plant efficiency.

| Parameter        | Typical Operation (Baseline) | **Optimized Operation (Predicted)** | Improvement |
| :--------------- | :--------------------------- | :-------------------------------- | :---------- |
| **Generator MW** | ~635 MW                      | **695.47 MW** | **+9.5%** |
| **Coal Flow** | ~346.5                       | **325.60** | **-6.0%** |

---

## 5. How to Use This Repository

This repository contains the code and saved artifacts to reproduce the project results.

#### Prerequisites
You will need the following libraries installed:
```bash
pip install pandas numpy scikit-learn xgboost optuna joblib
```

## 6. Conclusion

This project serves as a powerful case study on the practical application of machine learning for industrial process optimization. The key insight is that significant efficiency gains are often found not by maximizing individual inputs, but by discovering a more harmonious and efficient balance between all controllable variables. The final proposed settings provide a clear, data-driven path to generating more power with less fuel, directly improving both financial and environmental outcomes.
