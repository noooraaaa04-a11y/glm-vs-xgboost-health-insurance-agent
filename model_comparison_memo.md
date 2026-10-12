# Model Comparison Memo: Poisson GLM vs. XGBoost Challenger (Claim Frequency)

**To:** Pricing Actuary  
**From:** ABC Health Model Comparison Assistant  
**Date:** Current Evaluation  
**Subject:** Out-of-Time Performance Comparison of Poisson GLM and XGBoost Frequency Models  

---

### Purpose
This memorandum provides a comparative evaluation of the production candidate Poisson GLM and the XGBoost challenger for health insurance claim frequency, assessed on unseen out-of-time data (policies starting in months 10-12). Note that all data and analyses discussed herein are based on synthetic data generated for the fictional insurer ABC Health.

### Data and Method
The evaluation uses an unseen test quarter comprising 12,463 policies and 482 claims. We compare the out-of-time performance of both models across Poisson deviance, Gini coefficient (risk ranking), actual-over-expected (A/E) calibration, and top-to-bottom relative claim frequency ratios. Furthermore, a bootstrap comparison (200 runs) was conducted to test whether performance differences between the models are statistically robust or attributable to sample variation.

### Results Table

| Metric | Poisson GLM | XGBoost Challenger |
| :--- | :--- | :--- |
| **Poisson Deviance (lower is better)** | 0.385 | 0.3947 |
| **Gini Coefficient (higher = better risk ranking)** | 0.2696 | 0.1784 |
| **Actual-over-Expected Ratio** | 1.0102 | 1.0142 |
| **Top-to-Bottom Ratio (riskiest 20% vs safest 20%)** | 3.32 | 2.79 |
| **Test Policies / Claims** | 12,463 / 482 | 12,463 / 482 |

#### Calibration by Age Band (Actual / Expected)
*Note: Each age band holds only about 70-140 claims, so age-band deviations may be random noise.*

* **18–35 (3,892 policies):** GLM A/E: 0.823 | XGBoost A/E: 0.804
* **36–45 (2,119 policies):** GLM A/E: 0.977 | XGBoost A/E: 0.966
* **46–55 (2,088 policies):** GLM A/E: 0.949 | XGBoost A/E: 1.026
* **56–65 (2,208 policies):** GLM A/E: 1.108 | XGBoost A/E: 1.138
* **66–75 (2,156 policies):** GLM A/E: 1.155 | XGBoost A/E: 1.119

---

### Interpretation
* **Risk Ranking & Deviance:** The Poisson GLM achieves a higher Gini coefficient (0.2696 vs 0.1784) and a lower Poisson deviance (0.385 vs 0.3947), indicating superior risk differentiation and predictive accuracy on this test set.
* **Bootstrap Significance:** A bootstrap re-sampling test (200 runs) of the test policies showed a mean Gini difference (GLM minus XGBoost) of 0.0911 with a 95% interval of [0.0544, 0.1338], and a deviance improvement mean of 0.0096 with a 95% interval of [0.0054, 0.0147]. Because neither interval includes zero (with 100% of runs favoring the GLM), the evidence suggests the performance difference is real and unlikely to be due to luck alone.
* **Discrimination:** The GLM's top-to-bottom ratio of 3.32 demonstrates a wider spread between the riskiest and safest policy groups compared to XGBoost's 2.79.
* **Calibration:** Both models show similar overall actual-over-expected totals (1.0102 for GLM and 1.0142 for XGBoost). Across individual age bands, both models tend to underpredict claims in older age groups (e.g., 56–75) and overpredict in younger bands (e.g., 18–35); however, given the small claim counts per band (~70–140 claims), these variances may largely reflect random noise.

---

### Limitations
* The data is synthetic and was generated from a GLM-style formula, which may naturally favour the GLM.
* Results are based on a single train/test split.
* The bootstrap re-samples only the test policies; it does not retrain the models or test other periods.
* Scope is strictly limited to claim frequency (no severity or premium calculations are included).

---

### Draft Recommendation
The evidence suggests retaining the Poisson GLM as the production pricing model, given its stronger risk-ranking performance and superior deviance on the unseen test quarter. The XGBoost challenger should be retained for diagnostic and feature-interaction exploration, but should not replace the GLM for production deployment at this stage.

Draft for review - not signed