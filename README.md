# Sleep Quality Prediction Using Machine Learning

## 📊 Overview

This project aims to predict the **Quality of Sleep** (rated 1–10) using real-world health and lifestyle data. The workflow follows the standard data science pipeline: data cleaning, exploratory data analysis (EDA), feature engineering, model building, hyperparameter tuning, and evaluation. Four regression models were trained and compared, and the best-performing model was selected based on test performance metrics.

---

## 📦 Dataset Details

* **File:** `Sleep_health_and_lifestyle_dataset.csv`
* **Size:** 374 rows × 13 columns
* **Target Variable:** `Quality of Sleep` (scale 1–10)
* **Features include:**

  * **Demographics:** Gender, Age, Occupation
  * **Sleep Metrics:** Sleep Duration, Quality of Sleep
  * **Lifestyle:** Physical Activity Level, Daily Steps, Stress Level
  * **Health:** BMI Category, Blood Pressure (split into Systolic BP & Diastolic BP), Heart Rate
  * **Medical:** Sleep Disorder (None / Insomnia / Sleep Apnea)

---

## 🚀 Getting Started

To explore and run the notebook:

1. **Clone the repository:**

   ```bash
   git clone https://github.com/Bruhadev45/Sleep_Quality_Prediction.git
   cd Sleep_Quality_Prediction
   ```

2. **Install the required packages:**
   Ensure you have Python 3.10+ installed. Then install the necessary libraries:

   ```bash
   pip install pandas numpy scikit-learn matplotlib seaborn jupyter
   ```

3. **Launch the Jupyter Notebook:**

   ```bash
   jupyter notebook "Sleep_Quality_Prediction.ipynb"
   ```

4. Make sure `Sleep_health_and_lifestyle_dataset.csv` is placed in the same working directory as the notebook.

5. Run all cells from top to bottom (`Kernel → Restart & Run All`) to execute the full pipeline.

---

## 🛠️ Workflow

### 1. Data Cleaning & Preprocessing

* Loaded the dataset and inspected shape, data types, and summary statistics.
* Identified and handled **219 missing values** in the `Sleep Disorder` column — filled with `"No Disease"` rather than dropping rows (dropping would have removed ~59% of data).
* Split the `Blood Pressure` column (`"120/80"` format) into two separate numeric columns: `Systolic BP` and `Diastolic BP`.
* Standardized inconsistent labels in `BMI Category` (`"Normal Weight"` → `"Normal"`).
* Dropped the `Person ID` column as it carries no predictive value.
* Ensured correct data types for all columns.

---

### 2. Exploratory Data Analysis (EDA)

* Performed **univariate analysis** — count plots for categorical features, KDE histograms for continuous features.
* Investigated 10 structured research questions through targeted visualizations:

  | # | Research Question | Chart Used |
  |---|-------------------|------------|
  | 1 | Does sleep quality vary by gender and BMI? | Heatmap |
  | 2 | Which occupations have the highest stress levels? | Bar plot |
  | 3 | Does physical activity affect sleep quality? | Scatter plot |
  | 4 | How does sleep disorder prevalence differ by gender? | Grouped count plot |
  | 5 | Does daily step count vary across BMI categories? | Box plot |
  | 6 | Does having a sleep disorder affect physical activity? | Violin plot |
  | 7 | How do sleep duration and quality relate to stress? | 3-variable scatter |
  | 8 | How do BP, heart rate, and steps vary with sleep quality? | GroupBy table |
  | 9 | How do sleep metrics vary across age groups? | GroupBy table |
  | 10 | What is the relationship between daily steps and heart rate? | Regression plot |

* Generated a **full correlation heatmap** to identify linear relationships between all numeric features.
* Plotted data distribution histograms for outlier detection and shape analysis.

---

### 3. Feature Engineering

* Created three new domain-informed features to enrich the model's signal:

  | Feature | Formula | Purpose |
  |---------|---------|---------|
  | `BMI Score` | Normal=1, Overweight=2, Obese=3 | Ordinal encoding of BMI for calculations |
  | `Health Risk Score` | `(BMI Score × 2) + Stress Level + (Heart Rate / 10)` | Composite health burden metric |
  | `STI` (Stress Tolerance Index) | `Heart Rate / Stress Level` | Cardiovascular response to stress |

* Applied **Label Encoding** to all categorical columns before model training:
  * `Gender`, `Occupation`, `BMI Category`, `Sleep Disorder`
* Removed duplicate records to prevent data leakage across the train/test split.

---

### 4. Machine Learning Models

#### Model Evaluation & Selection

* **Train/Test Split:** 80% training · 20% testing · `random_state=42`
* **Feature Matrix (X):** 15 features — 12 original + 3 engineered
* Four models were trained and evaluated on the test set:

  | Model | Performance |
  |-------|------------|
  | Linear Regression | Baseline — moderate performance |
  | **Ridge Regression** | ✅ Strong — selected for tuning |
  | SVR | Lower performance — not selected |
  | **Random Forest Regressor** | ✅ Best — selected for tuning |

* **SVR showed much lower performance** compared to the other models on test data.
* Based on results, **Random Forest** and **Ridge Regression** were selected for hyperparameter tuning using `RandomizedSearchCV` with 5-fold cross-validation.

---

#### Hyperparameter Tuning

* **Ridge:** Tuned `alpha` over a range of 0.1 – 2.0
* **Random Forest:** Tuned `n_estimators`, `max_depth`, `max_features`, `min_samples_split`, `min_samples_leaf`, and `bootstrap`
* Best parameters found for Random Forest:

  ```
  n_estimators=780, max_depth=43, max_features='sqrt',
  min_samples_split=2, min_samples_leaf=2, bootstrap=False
  ```

---

#### Training Results for Selected Models

| Model | R² | MAE | MSE |
|-------|----|-----|-----|
| **Ridge Regression** | 0.9280 | 0.2394 | 0.1085 |
| **Random Forest Regressor** | **0.9678** | **0.0600** | **0.0484** |

* **R²** — Coefficient of determination (higher is better; 1.0 = perfect fit)
* **MAE** — Mean Absolute Error (lower is better; average prediction error)
* **MSE** — Mean Squared Error (lower is better; penalizes large errors)

---

## 🏆 Key Findings

* **Random Forest Regressor** achieved the best overall performance with **R² = 0.9678**, lowest MAE, and lowest MSE — making it the final chosen model.
* **Ridge Regression** also performed strongly and is a viable lightweight alternative.
* **SVR** underperformed on this dataset and was not selected for final evaluation.
* Key factors driving sleep quality predictions:
  * `Stress Level` — strongest negative predictor
  * `Sleep Duration` — strong positive predictor
  * `Health Risk Score` — composite health burden
  * `Systolic BP` / `Diastolic BP` — cardiovascular health signals
  * `Physical Activity Level` — active individuals sleep better
* Feature importance analysis confirmed that **engineered features** (`Health Risk Score`, `STI`) contributed meaningfully alongside the original dataset features.

---

## 🗂️ Files

| File | Description |
|------|-------------|
| `Sleep_health_and_lifestyle_dataset.csv` | Raw dataset — 374 rows × 13 columns |
| `Sleep_Quality_Prediction.ipynb` | Complete notebook — cleaning, EDA, modelling, evaluation |
| `README.md` | Project documentation |

---

