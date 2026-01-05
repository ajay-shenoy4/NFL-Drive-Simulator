# NFL Drive Simulator & 4th Down Decision Engine

![NFL Analytics](https://img.shields.io/badge/Data-nflfastR-blue)
![Model](https://img.shields.io/badge/Model-XGBoost-orange)
![R](https://img.shields.io/badge/Maintained%20in-R-red)

## Project Overview
This project leverages 10 years of NFL play-by-play data (2015–2025) to build a probabilistic simulator for offensive drives. Using a combination of **XGBoost** for yardage prediction and **Logistic Regression** for play-type tendency and situational success, the engine can simulate individual plays, full drives, and provide optimal 4th-down decision logic based on **Expected Points (EP)**.

The core goal is to determine the "mathematically optimal" decision in high-leverage situations while accounting for the stochastic nature (noise) of professional football.

---

## Key Features
* **Yardage Prediction:** An XGBoost regressor trained on situational data to predict the outcome of any given run or pass.
* **Tendency Modeling:** A GLM (General Linear Model) that predicts whether a team is likely to pass or run based on the current "game state."
* **4th Down Decision Bot:** A mathematical engine that compares the Expected Points (EP) of:
    * **Going for it:** Weighted by conversion probability and expected field position gain.
    * **Field Goal:** Modeled on kicker accuracy relative to distance.
    * **Punting:** Modeled on net punt averages, return yards, and touchback probabilities.
* **Monte Carlo Drive Simulator:** Simulate 1,000+ drives from any spot on the field to see the distribution of touchdowns, turnovers, and punts.

---

## Technical Workflow

### 1. Data Integration & ETL
The project utilizes `nflfastR` to ingest over a decade of play-by-play data. The data is cleaned to focus on "true" offensive plays:
* **Exclusions:** QB spikes, kneels, and non-regular season action.
* **Feature Engineering:** Calculation of `yards_manual` by comparing current and subsequent yard lines to ensure data integrity.

### 2. Modeling Architecture
* **Primary Yardage Model:** XGBoost using `reg:squarederror`.
    * **Features:** Down, Distance, Yardline, Quarter, Time Remaining, Score Differential, Play Type, and Goal-to-Go status.
* **Situational Models:** Logistic regressions for Field Goal success probability and 4th-down conversion rates.

### 3. Simulation Logic
The simulation includes "stochastic noise" (modeled after the model's RMSE) to ensure play outcomes reflect real-world variance. 
* **Turnover Simulation:** Incorporates historical league averages for Fumble and Interception rates.
* **Sack Logic:** Probabilistic sack triggers that result in yardage loss and stalled drives.
* **Clock Management:** 40-second runoff for runs and 8-second runoff for passes.

---

## Performance & Evaluation
The model's accuracy is tracked using **RMSE (Root Mean Squared Error)** and **MAE (Mean Absolute Error)**. 

### Feature Importance (Top 5)
1. **yardline_100** (Field position)
2. **ydstogo** (Distance to marker)
3. **down**
4. **play_type** (Run vs Pass)
5. **half_seconds_remaining**

---

## Visualizations
The script generates several analytical plots to compare the model against historical reality:
1. **Expected Points by Field Position:** A line graph showing the "break-even" points for Punting vs. Field Goals vs. Going for it.
2. **Simulation vs. Reality:** A comparison bar chart matching simulated drive outcomes against historical NFL frequencies (2015-2025).
3. **Drive Heatmaps:** Histograms showing the distribution of plays per drive by outcome.

---

## Usage

### Prerequisites
Ensure you have the following R libraries installed:
```r
install.packages(c("nflfastR", "xgboost", "caret", "tidyverse", "readr"))
