# Capstone Report — Content Trend Prediction for Refresh Prioritization

- **Author:** Siddhartha Dwivedi
- **Lane:** Content Trend Prediction
- **Repo:** https://github.com/Sidd13789/flyrank-ml-internship
- **Date:** 24 July 2026

---

## 1. Problem framing

The goal of this project is to help FlyRank content teams identify which pages should be refreshed first. The unit of analysis is an individual content page. The model predicts the future content trend direction using historical SEO, engagement, and freshness metrics. Editors can use these predictions to prioritize content updates instead of manually reviewing thousands of pages. A wrong prediction may cause unnecessary updates or missed opportunities, so the model is intended to support human decision-making rather than replace it. Machine learning is useful because it can detect patterns across large datasets that are difficult to identify manually.

---

## 2. Data safety

This project uses anonymized production search data provided during the FlyRank Machine Learning Internship. Features include search performance, traffic, engagement, and content freshness metrics.

The following columns were excluded from the model:
- `trend_direction` (used only as the target label)
- `trend_pct` (label-derived information)
- `content_id`
- `client_id`

These columns were excluded to prevent data leakage and avoid learning client-specific information. No client-identifying information is included anywhere in the `work/` directory.

---

## 3. Baseline

A simple majority-class baseline was used for comparison. The baseline predicts the most common trend class for every content page. This provides a transparent benchmark against which the Random Forest model can be evaluated. The Random Forest model achieved substantially better predictive performance than the baseline on the same evaluation split.

---

## 4. Model / analysis

A Random Forest Classifier was selected because it performs well on structured tabular data and can model non-linear relationships between features.

Input features included:
- Search volume
- Competition
- CPC
- Word count
- Character count
- Impressions
- Clicks
- Sessions
- Engagement metrics
- Freshness metrics
- Content age
- CTR
- Average position
- Other numerical SEO features

The following columns were intentionally excluded:
- `trend_direction`
- `trend_pct`
- `client_id`
- `content_id`

Target:
Predict the future content trend direction (up, down, stable, new, or flat).

---

## 5. Evaluation

The dataset was split into 80% training data and 20% testing data using a random seed of 42.

Evaluation metric:
- Classification Accuracy

The Random Forest model achieved high predictive performance on the evaluation split and outperformed the majority-class baseline. A limitation of this evaluation is that future work should include grouped or time-aware validation to further reduce the possibility of feature leakage. Most prediction errors occurred between similar trend categories such as *stable* and *flat*.

---

## 6. Interpretation

The analysis suggests that engagement metrics, traffic statistics, and freshness-related features are among the strongest indicators of future content performance. Historical search behaviour appears to provide useful signals for predicting trend direction. The results support using machine learning as a decision-support system, although additional validation is recommended before production deployment.

---

## 7. Recommendation

The model should be used to rank content pages that are likely to decline in performance so editors can prioritize updates more efficiently. Pages predicted to remain stable require less immediate attention, allowing editorial resources to focus on higher-impact content. Confidence in the results is high for experimentation, but production deployment should include additional leakage-resistant validation and continuous monitoring.

---

## 8. Reproducibility

Environment:
- Python 3.x
- Pandas
- NumPy
- Scikit-learn
- Matplotlib

Random Seed:
- 42

Steps to reproduce:

```bash
git clone https://github.com/Sidd13789/flyrank-ml-internship.git
cd flyrank-ml-internship
pip install -r requirements.txt
jupyter notebook
```

Open `work/notebooks/capstone.ipynb` and run all cells from top to bottom to reproduce the complete workflow and results.

---

### Claims Checklist

- ✔ Observed results only
- ✔ Decision-support model
- ✔ No causal claims
- ✔ No client-identifying information
- ✔ Label leakage considered
- ✔ Results reproducible
