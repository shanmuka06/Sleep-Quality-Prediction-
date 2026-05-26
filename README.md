# 😴 Sleep Quality Prediction

A machine learning project that analyzes lifestyle and health factors to predict sleep quality using the **Sleep Health and Lifestyle Dataset**.

---

## 📌 Project Overview

Sleep quality is influenced by a wide range of physical and lifestyle factors. This project performs end-to-end analysis — from data cleaning and exploratory analysis to training and tuning multiple regression models — to predict an individual's sleep quality score (1–10).

The best model achieved a **~96.78% R² accuracy** using a tuned Random Forest Regressor.

---

## 📂 Dataset

**Source:** Sleep Health and Lifestyle Dataset  
**Size:** 374 rows × 13 columns

| Feature | Description |
|---|---|
| Gender | Male / Female |
| Age | Age in years |
| Occupation | Job/profession |
| Sleep Duration | Hours of sleep per day |
| Quality of Sleep | Subjective rating (1–10) — **target variable** |
| Physical Activity Level | Minutes of activity per day |
| Stress Level | Subjective rating (1–10) |
| BMI Category | Underweight / Normal / Overweight / Obese |
| Blood Pressure | Systolic/Diastolic (split into two features) |
| Heart Rate | Resting BPM |
| Daily Steps | Steps per day |
| Sleep Disorder | None / Insomnia / Sleep Apnea |

---

## 🔧 Project Pipeline

### 1. Data Cleaning
- Removed redundant `Person ID` column
- Parsed `Blood Pressure` into separate `Systolic BP` and `Diastolic BP` columns
- Standardized BMI category labels
- Filled 219 missing `Sleep Disorder` values with `"No Disease"`
- Removed duplicate records

### 2. Feature Engineering
- **BMI Score** — numeric mapping of BMI categories
- **Health Risk Score** — composite score: `(BMI Score × 2) + Stress Level + (Heart Rate / 10)`
- **Stress Tolerance Index (STI)** — ratio of Heart Rate to Stress Level

### 3. Exploratory Data Analysis

**Univariate Analysis**
- Count plots for all categorical columns
- Histograms with KDE for all continuous columns

**Multivariate Analysis** — key questions explored:
- Does sleep quality vary by gender and BMI?
- Which occupations have the highest stress levels?
- How does physical activity relate to sleep quality?
- How do sleep disorders differ between genders?
- How do sleep duration and quality vary across age groups?
- What is the relationship between daily steps and heart rate?
- How does Health Risk Score vary with sleep quality?

### 4. Machine Learning

**Models Trained:**
| Model | Notes |
|---|---|
| Linear Regression | Baseline |
| Ridge Regression | L2 regularization |
| SVR | Support Vector Regressor |
| Random Forest Regressor | Best performer |

**Hyperparameter Tuning:**
- `RandomizedSearchCV` with 5-fold cross-validation applied to Ridge and Random Forest

**Best Model — Random Forest Regressor:**
```
n_estimators=780, max_depth=43, max_features='sqrt',
min_samples_split=2, min_samples_leaf=2, bootstrap=False
```

**Results:**
| Metric | Score |
|---|---|
| R² | ~0.9678 |
| MAE | Low |
| MSE | Low |

### 5. Feature Importance

The tuned Random Forest was used to extract and visualize feature importances, identifying which health and lifestyle factors most strongly predict sleep quality.

---

## 🛠️ Tech Stack

- **Python 3**
- `pandas`, `numpy` — data manipulation
- `matplotlib`, `seaborn` — visualization
- `scikit-learn` — modeling, evaluation, hyperparameter tuning

---


---

## 📊 Key Findings

- **Sleep duration** and **stress level** are strongly correlated with sleep quality
- People with **higher physical activity** tend to report better sleep quality
- **Overweight/Obese** individuals generally show lower sleep quality scores
- **Nurses and Sales Representatives** report the highest average stress levels
- The Random Forest model with tuned hyperparameters achieves ~96.78% R² on the test set

---

## 📁 Project Structure

```
sleep-quality-prediction/
│
├── Sleep_Quality_Prediction.ipynb   # Main notebook
├── Sleep_health_and_lifestyle_dataset.csv  # Dataset (add manually)
└── README.md
```

---

## 👤 Author

**B Shanmukanath Reddy**  
📧 [sanmukanathreddy11@gmail.com](mailto:sanmukanathreddy11@gmail.com)  
Feel free to connect or raise an issue for any questions or suggestions.

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
