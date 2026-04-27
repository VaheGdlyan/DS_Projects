# Freemium Revenue Intelligence System
### End-to-end behavioral analysis, statistical experimentation, and revenue prediction for a mobile subscription product

---

## Business Problem

A mobile application running a freemium model faces a compounding monetization challenge: the product is generating engagement but not revenue. The top of the funnel is healthy — nearly 39,000 unique devices — yet the paywall converts at **0.8%**, and only **1.69% of users ever generate any revenue**.

The core question is not *what is the conversion rate* — it is *why it is this low, whether product changes can move it, and which users are worth targeting*.

This system answers all three.

---

## System Overview

The architecture follows a deliberate three-stage progression. Each stage is independently valuable; together, they form a closed-loop decision engine.

```
[ Stage 1: Behavioral Analysis ]  ──▶  [ Stage 2: Experimentation ]  ──▶  [ Stage 3: Prediction ]
   Where users drop and why            Does the fix actually work?          Who will pay, and how much?
```

**Why this order matters:**  
Funnel analysis identifies the *where* and *why* of monetization failure. Experimentation provides statistically rigorous proof that a proposed fix works before a full rollout. Predictive modeling operationalizes both — allowing the system to score every incoming user and prioritize intervention before revenue is lost.

---

## Key Insights & Evidence

### Stage 1 — Behavioral Analysis (Funnel)

**Insight 1: The paywall is the terminal failure point.**  
Of 11,451 unique devices that reach the subscription offer screen, only 92 complete a purchase — a **0.8% stage-to-stage conversion rate**. Concurrently, 20,000+ users successfully export content using the free tier. The free product is robust enough to satisfy the average user, collapsing the perceived value of premium.

**Insight 2: Registration is a side branch, not a gate.**  
Only 10.5% of users (4,099 / 38,730) open the registration screen. Of those, 44% abandon before completing. Meanwhile, 24,681 devices access the creator flow without ever registering. The application's "Guest Mode" architecture removes the primary incentive to authenticate, disconnecting the product from its own monetization funnel.

**Insight 3: Platform and geography stratify conversion quality.**  
Apple users convert at the bottom of the funnel at a higher absolute volume (59 vs. 33 subscriptions) despite lower top-of-funnel volume. Germany — the smallest market by raw event volume — outperforms Brazil, Vietnam, and Egypt in late-stage conversion relative to size, reflecting macroeconomic purchasing power differentials that require localized pricing (PPP) to address.

> 📊 **Segmented Funnel Analysis**
> ![Android vs Apple Funnel](C:\Users\Home\Desktop\DS_Projects\images\output_android_ios.png) 
---

### Stage 2 — Experimental Validation (A/B Test)

**Scale:** ~887,000 users, 19-day experiment window, SRM-validated (Chi-Square p = 0.2219).

**Primary Metric — Conversion Rate:**  
The redesigned paywall (Variant B) produced a **flat result** on conversion volume (p = 0.1669). H₀ was not rejected. Variant B does not increase the number of buyers.

**Secondary Metric — Revenue Per Visitor:**  
Variant B produced a statistically significant increase in revenue quality. Average Revenue Per Converter shifted from **$46.83 → $48.57** (Mann-Whitney U, p = 0.0128). The Gini coefficient moved from 0.307 to 0.291, confirming the lift is distributed across the mid-tier — not driven by outlier "whale" activity.

**Bottom line:** Variant B is an upsell engine, not a volume engine. It targets the same number of users but extracts more value per transaction.

**Revenue impact at scale:** RPV delta of **+$0.0179 per visitor** translates to approximately **$17,900 per 1M visitors** (~$2.1M annually at 10M monthly visitors). The 95% CI is entirely above zero; direction is not in question.

**Decision:** Ship Variant B via staged rollout — 20% → 50% → 100% traffic. Flag the Oct 10–11 daily RPV anomaly for engineering review before the 50% gate opens.

> 📊 **Conversion Rates with 95% Confidence Intervals**
> ![Variant A vs Variant B Conversion Rates](C:\Users\Home\Desktop\DS_Projects\images\output_variantA_variantB.png) 

---

### Stage 3 — Predictive Modeling (Revenue)

**Problem:** Extreme zero-inflation (98.31% non-payers, 1.69% payers) across 390,000+ training users. Standard classification fails — predicting zero for all users achieves 98%+ accuracy while being operationally worthless.

**Architecture — Two-Stage Hurdle Model:**

| Stage | Task | Model | Output |
|---|---|---|---|
| Stage 1 | Will this user pay? | LightGBM Classifier (`scale_pos_weight`) | P(Payer) |
| Stage 2 | If they pay, how much? | Gradient Boosted Regressor | E(Revenue \| Payer) |
| Final | Expected revenue | Platt Scaling + Threshold Optimization | Calibrated revenue score |

**Key results:**

| Metric | Pre-Calibration | Post-Calibration |
|---|---|---|
| MAE | $8.79 | **$1.83** |
| Revenue Capture (Top 10%) | 92.9% | 92.9% |
| Model State | Over-confident | Production-ready |

Platt Scaling eliminated systematic probability inflation from `scale_pos_weight` without sacrificing ranking performance. The 79.2% MAE reduction was achieved purely through calibration — the underlying ranking model was already elite.

**Top predictive signals:**
- `storage_gb` — hardware tier as a purchasing power proxy
- `first_paid_tap_hour` — speed to first monetization touchpoint ("Golden Window")
- `engagement_score` — session depth distinguishing active users from passive ones

> 📊 **Reliability Curves: Uncalibrated vs. Platt-Scaled Probabilities**
> ![Reliability Curves](C:\Users\Home\Desktop\DS_Projects\images\output_normal+platt-scaled.png)

---

## Results & Business Impact

| Decision | Evidence | Mechanism |
|---|---|---|
| Ship Variant B (staged rollout) | RPV +$0.0179/visitor, p < 0.05 | ARPC lift, Gini democratized |
| Localized pricing for BR/VN/EG | Geographic CR variance p = 0.000167 | PPP differential confirmed |
| Target "Golden Window" users | Activation rate: 70.9% payers vs. 42.8% non-payers | First 72 hours are decisive |
| Score users at D3 for early intervention | MAE $1.83, 92.9% revenue capture | Hurdle model identifies payers before they pay |
| Paywall redesign is insufficient alone | Flat CR despite significant ARPC lift | Volume fix requires trigger-placement experiment |

The system does not produce insights — it produces decisions with confidence intervals attached.

---

## Project Structure

```
freemium-revenue-intelligence/
│
├── task_1_subscription_funnel.ipynb   # Funnel construction, segmentation, product hypotheses
├── task_2_testing_analysis.ipynb      # A/B test integrity, hypothesis testing, recommendation
├── task_3_notebook.ipynb              # Two-stage hurdle model, calibration, feature audit
│
├── data/
│   ├── app_open_data.csv
│   ├── event_data.csv
│   ├── ab_test_data.csv
│   ├── train.csv
│   └── test.csv
│
└── README.md
```

---

## Tech Stack

| Layer | Tools |
|---|---|
| Data processing | `pandas`, `numpy` |
| Statistical testing | `scipy.stats`, `statsmodels` |
| ML modeling | `lightgbm`, `scikit-learn` |
| Calibration | `sklearn.calibration` (Platt Scaling) |
| Visualization | `matplotlib`, `seaborn`, `plotly` |
| Experiment design | Two-Proportion Z-Test, Welch's T-Test, Mann-Whitney U, O'Brien-Fleming sequential bounds |

---

## Reproducibility

```bash
# 1. Clone the repository
git clone https://github.com/VaheGdlyan/DS_Projects.git
cd DS_Projects

# 2. Install dependencies
pip install pandas numpy scipy statsmodels lightgbm scikit-learn matplotlib seaborn plotly shap

# 3. Place raw data files into /data/

# 4. Run notebooks in order
jupyter notebook task_1_subscription_funnel.ipynb
jupyter notebook task_2_testing_analysis.ipynb
jupyter notebook task_3_notebook.ipynb
```

All random states are fixed (`SEED = 42`). Results are deterministic across environments.

---

## Future Improvements

| Priority | Improvement | Rationale |
|---|---|---|
| High | Trigger-placement A/B test (paywall at tool-use moment vs. volume trigger) | Task 1 showed feature-gated paywalls drive 26× higher CR than volume-trigger paywalls |
| High | PPP pricing experiment in Brazil / Vietnam | Geographic CR variance is statistically confirmed; unit price reduction may unlock volume |
| Medium | Real-time user scoring pipeline (D1 → D3 signal ingestion) | Model is trained on D1–D3 signals; deploying at D3 enables early lifecycle intervention |
| Medium | Multi-market model stratification | 100% of payers in current dataset are US-based; separate models per region would improve generalization |
| Low | Replace Platt Scaling with Isotonic Regression calibration | Isotonic is non-parametric and may outperform Platt on non-sigmoid miscalibration shapes |
