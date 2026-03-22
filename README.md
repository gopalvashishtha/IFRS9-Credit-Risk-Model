# IFRS9-Credit-Risk-Model
End-to-end IFRS 9 ECL model including PD/LGD modeling, Staging logic, and Macro-economic scenario analysis.

## Overview
This project implements a comprehensive IFRS 9 Impairment Engine using Python. It covers the full end-to-end credit risk modeling workflow, from feature engineering using Weight of Evidence (WoE) to multi-scenario macro-adjusted loss forecasting.

Source: Lending Club (Kaggle) https://www.kaggle.com/datasets/wordsforthewise/lending-club

Data Volume: A sub-sample of 50,000 rows was used to ensure computational efficiency while maintaining statistical significance.

## Methodology & Modeling
The pipeline is built on the three pillars of Credit Risk: PD, LGD, and EAD.

1) **Probability of Default (PD):**  Developed a Logistic Regression model using Weight of Evidence (WoE) transformed features.
   -  Variables selected based on Information Value (IV), focusing on repayment capacity (DTI, Income) and
       credit quality (FICO, Grade).

   - Validation: Achieved a Test AUC of 0.69 and a Gini of 0.38, showing strong, stable discrimination without overfitting.

2)  **Loss Given Default (LGD)**
    - Implemented a Segmented LGD framework with a baseline of 70% (standard for unsecured retail).
    - Applied directional haircuts/add-ons based on home ownership (collateral proxy) and loan purpose (asset quality).

3) **Exposure at Default (EAD):**
   - Utilized the current outstanding loan amount as the primary EAD proxy.
  
 ## IFRS 9 Staging & Regulatory Proxies
  **Since the dataset is a static snapshot, I implemented the following SICR (Significant Increase in Credit Risk) proxies to simulate staging:**
  - Stage 1 (Performing): Current loans with low DTI and strong internal grades.
  - Stage 2 (SICR Proxy): Loans showing signs of stress—defined as DTI > 40%, poor internal grades (Grade E/F/G), or recent delinquencies.
  - Stage 3 (Default): Loans marked as "Charged Off," "Default," or "31-120 days late" (Rebuttable presumption of 90 DPD).

 ## Forward-Looking Macro Adjustment
 **To satisfy the "Forward-Looking" requirement of IFRS 9, I implemented a Probability-Weighted ECL across three economic scenarios:**
  - Upside (20%): Unemployment decrease (-1%).
  - Base (50%): Current economic conditions.
  - Downside (30%): Unemployment spike (+2%).

## Model Health & Validation
- Calibration: Achieved an E/A Ratio of 1.0095, indicating near-perfect calibration of predicted vs. actual defaults.
- Multicollinearity: Verified all VIF scores < 1.5, ensuring a stable and interpretable coefficient set.
- Monotonicity: Conducted trend analysis on DTI and Income to ensure the model aligns with economic intuition.
