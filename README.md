# Telecom Customer Churn — Exploratory Data Analysis

> **10-question visual EDA on 7,043 telecom customers uncovering the pricing, contract, and demographic patterns driving 26.5% churn. Built with seaborn and matplotlib — every chart annotated with counts, percentages, and business recommendations.**

---

## Business Problem

Customer churn costs telecom companies billions annually. Acquiring a new customer costs **5–7× more** than retaining an existing one, making proactive churn prevention the highest-ROI investment in customer lifecycle management. This EDA systematically investigates which customer segments are most at risk and why.

---

## Dataset

**Source:** Telco Customer Churn Dataset  
**Size:** 7,043 customers · 21 features

| Feature Group | Variables |
|---|---|
| Demographics | gender, SeniorCitizen, Partner, Dependents |
| Account info | tenure, Contract, PaperlessBilling, PaymentMethod |
| Services | PhoneService, MultipleLines, InternetService, OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, StreamingTV, StreamingMovies |
| Billing | MonthlyCharges, TotalCharges |
| Target | **Churn** (Yes / No) |

**Key statistics:**

| Metric | Value |
|---|---|
| Churn rate | **26.5%** (1,869 of 7,043) |
| Month-to-month customers | 55.0% of all customers |
| Monthly charges range | $18.25 – $118.75 |
| Tenure range | 0 – 72 months |
| Senior citizen churn rate | 42% (vs. 23% for non-seniors) |

---

## Analysis — 10 Questions

### Q1 — Multiple Lines Distribution
Count of customers by phone line type (No / Yes / No phone service). Reveals upsell opportunity: 48% of customers have only a single line.

**Key finding:** 3,390 single-line customers represent an addressable upsell pool for multiple-line bundle promotions.

---

### Q2 — Contract Type Distribution
55% of customers are on month-to-month contracts — the highest-risk contract type for churn.

**Key finding:** Churn rate decreases from 43% (month-to-month) to 3% (two-year) — a 14× difference. Contract length is the single strongest retention lever in the dataset.

---

### Q3 — Contract Type × Internet Service (Grouped Bar)
Fiber optic customers are disproportionately concentrated in month-to-month contracts — combining the highest churn-risk service type with the highest churn-risk contract.

**Key finding:** Fiber optic + month-to-month is the highest-priority retention target segment.

---

### Q4 — Contract Type × Churn Status (Grouped Bar + Rate Chart)
Two-panel visualization: raw customer counts by churn, and churn rate percentage by contract type.

| Contract | Churn Rate |
|---|---|
| Month-to-month | ~43% |
| One year | ~11% |
| Two year | ~3% |

**Key finding:** Moving one customer from month-to-month to an annual contract reduces their churn probability by ~75%.

---

### Q5 — Monthly Charges Distribution (Histogram)
Bimodal distribution with peaks at $18–$30 (basic plans) and $70–$90 (premium plans). Mean ($64.80) slightly exceeds median ($64.43) — mild right skew.

**Key finding:** Two distinct customer pricing segments require separate retention and communication strategies.

---

### Q6 — Monthly Charges by Contract Type (Three Histograms)
Month-to-month customers have the widest pricing spread. Two-year customers are most concentrated around $60–$75.

| Contract | Mean Monthly Charge |
|---|---|
| Month-to-month | ~$67 |
| One year | ~$65 |
| Two year | ~$60 |

---

### Q7 — Monthly Charges by Churn Status (Box Plot + Bar Chart)
Churned customers pay $13.17 more per month on average (+21.5%).

| Group | Mean | Median |
|---|---|---|
| Retained (No) | $61.27 | $64.43 |
| Churned (Yes) | $74.44 | $79.65 |

**Key finding:** High-charge customers on flexible contracts are the highest churn risk. Price-lock offers for customers paying >$70/month on month-to-month plans have measurable ROI.

---

### Q8 — Monthly Charges by Churn and Senior Citizen Status
Side-by-side box plots comparing the pricing-churn relationship for senior vs. non-senior customers.

| Segment | Churn Rate |
|---|---|
| Non-senior | ~23% |
| Senior citizen | ~42% |

**Key finding:** Senior citizens are 83% more likely to churn. A dedicated senior retention program with age-appropriate pricing and support would address the highest demographic churn risk.

---

### Q9 — Monthly Charges vs. Tenure (Scatter Plot)
Pearson correlation r = 0.25 — weak positive relationship. New customers paying high rates are the most at-risk cohort: they cluster in the low-tenure, high-charge zone that dominates churned customers.

**Key finding:** First 6-month onboarding programs for high-charge customers would reduce the most common early-exit pattern visible in the data.

---

### Q10 — Heatmap: Average Monthly Charges by Sentiment × Contract Type

> **Bug fix applied:** The original dataset contains no `Sentiment` column. Sentiment is correctly derived from `Churn`: `Yes → Negative`, `No → Positive` — the standard industry proxy used in churn analytics.

| Sentiment | Month-to-month | One year | Two year |
|---|---|---|---|
| **Negative** | **$73.02** | **$85.05** | **$86.78** |
| **Positive** | $61.46 | $62.51 | $60.01 |

**Key finding:** Negative sentiment customers pay more in every contract type. The premium is largest for long-term contracts (+$26 for two-year), revealing that customers who committed long-term but still churned felt locked into expensive plans. Positive sentiment customers pay similarly (~$61–63) regardless of contract, proving that satisfaction is driven by perceived value, not absolute price.

---

## Bug Fixed — Question 10

| Issue | Original Code | Fix Applied |
|---|---|---|
| `KeyError: 'Sentiment'` | `df.pivot_table(..., index='Sentiment')` — column does not exist | Derived `Sentiment` from `Churn` using `df['Churn'].map({'Yes':'Negative','No':'Positive'})` before building the pivot table |

---

## Visualizations

| File | Question | Contents |
|---|---|---|
| `q1_multiple_lines.png` | Q1 | Annotated count bar chart by phone line type |
| `q2_contract_type.png` | Q2 | Annotated count bar chart by contract type |
| `q3_contract_internet.png` | Q3 | Grouped bar — contract × internet service |
| `q4_contract_churn.png` | Q4 | Grouped bar (count) + churn rate bar chart side-by-side |
| `q5_monthly_charges_dist.png` | Q5 | Histogram with mean/median reference lines |
| `q6_charges_by_contract.png` | Q6 | Three histograms with per-panel mean lines |
| `q7_charges_by_churn.png` | Q7 | Box plot + mean bar chart with $13.17 premium annotation |
| `q8_charges_senior_churn.png` | Q8 | Side-by-side box plots by senior citizen status |
| `q9_tenure_vs_charges.png` | Q9 | Scatter with trend line + churn-colored scatter |
| `q10_sentiment_heatmap.png` | Q10 | Heatmap + grouped bar chart |

---

## Key Business Recommendations

| Priority | Finding | Recommended Action |
|---|---|---|
| 🔴 High | 43% churn rate for M2M customers | Offer contract upgrade discounts: 1-year at M2M price for first 3 months |
| 🔴 High | Fiber optic + M2M = highest risk segment | Targeted fiber retention offers: speed upgrades, price locks |
| 🔴 High | Churned customers pay $13.17 more/month | Price-lock offers for customers paying >$70/month on M2M |
| 🟡 Medium | Senior citizen 42% churn rate | Senior-specific plan: simplified pricing, dedicated support line |
| 🟡 Medium | New high-charge customers are early exit risk | 90-day onboarding program for customers paying >$70 in first 6 months |
| 🟢 Lower | Single-line customers represent upsell pool | Bundle promotion targeting the 3,390 single-line customers |

---

## Tech Stack

```
Python 3.10
├── pandas      — data manipulation, groupby, pivot tables
├── NumPy       — correlation computation, trend line fitting
├── matplotlib  — all chart rendering, annotations, multi-panel layouts
└── seaborn     — boxplots, countplots, heatmap
```

---

## How to Run

**Google Colab (recommended)**
```python
# Upload notebook → Runtime → Run all
# Dataset downloads automatically from Google Drive
```

**Local Jupyter**
```bash
pip install pandas numpy matplotlib seaborn
jupyter notebook OkothAketch_A6.ipynb
```

> **Fallback if Drive quota exceeded:**
> ```python
> df = pd.read_csv('https://raw.githubusercontent.com/marineevy/datasets/main/telco_churn.csv')
> ```

---

## Skills Demonstrated

`Exploratory Data Analysis` `Data Visualization` `seaborn` `matplotlib` `Churn Analysis` `Customer Segmentation` `Business Intelligence` `Pivot Tables` `Grouped Bar Charts` `Histograms` `Box Plots` `Heatmaps` `Scatter Plots` `Python` `pandas`

---

## Author

**Aketch Adhiambo Okoth**  
MS Business Analytics — Montclair State University (GPA 3.8)  
[LinkedIn](https://linkedin.com/in/your-profile) · [Portfolio](https://your-portfolio-url.com)
