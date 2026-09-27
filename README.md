# Seller Lifecycle & Retention Analytics

## Overview

This project analyzes seller activation, lifecycle, retention, segmentation,
and churn behavior in an online marketplace.

The analysis focuses on the seller side of a two-sided marketplace and asks:

> How can a marketplace convert one-time sellers into consistent,
> high-value sellers — and detect churn before it happens?

The project combines lifecycle analysis, RFM segmentation, K-means clustering,
habit formation analysis, churn trajectory analysis, and marketplace-level
buyer-seller dynamics.

The analysis also includes methodological checks for right-censoring,
confidence intervals, denominator issues, and other potential sources of bias.

---

## Dataset

**Source:** Kaggle — Brazilian E-Commerce Public Dataset by Olist

- Sellers analyzed: **2,945**
- Analysis window: **20 months**
- Corrected lifecycle analysis window: **18 months**
- Seller-month records after lifecycle correction: **25,080**

The pilot period (2016-09 to 2016-12) was excluded following a
domain-informed analysis.

---

## Business Question

The project investigates four main questions:

1. Which seller segments generate the most marketplace value?
2. What early seller behaviors are associated with retention?
3. What patterns appear before sellers churn?
4. How can the marketplace prioritize activation and retention efforts?

---

## Methodology

The analysis follows a retention lifecycle framework based on the
**Amplitude Product Analytics Playbook**.

### 1. Critical Event

The first delivered order was selected as the critical event because it
provided approximately 96% dataset coverage.

### 2. Usage Interval

The data-derived 80th-percentile gap between seller activities was
**42 days**.

This was mapped to the Playbook's Monthly interval and operationalized
using a 30-day threshold.

### 3. Seller Lifecycle

Each seller-month was classified into:

- New
- Current
- Resurrected
- At Risk
- Churned

Right-censoring was addressed by excluding recent records that did not have
enough follow-up time to confirm churn.

### 4. Seller Segmentation

RFM-style features were combined with Product Diversity:

- Recency
- Frequency
- Monetary value
- Product diversity

K-means clustering was used to identify **5 seller segments**.

The cluster count was selected using Elbow and Silhouette analysis rather
than being chosen arbitrarily.

---

## Seller Lifecycle Distribution

After correcting for right-censoring:

| Lifecycle | Share |
|---|---:|
| Current | 36.4% |
| Churned | 33.9% |
| At Risk | 12.5% |
| New | 10.4% |
| Resurrected | 6.8% |

Current sellers represent the largest lifecycle category after excluding
records that were too recent to confirm churn.

---

## Seller Segmentation

Five seller segments were identified:

| Segment | % Sellers | % GMV |
|---|---:|---:|
| Top Sellers | 14.9% | 69.6% |
| Steady Sellers | 22.3% | 15.8% |
| At-Risk Sellers | 18.4% | 11.4% |
| Low-Volume Active | 20.0% | 1.8% |
| Dormant / One-Time | 24.4% | 1.4% |

The Top Seller segment represents only **14.9% of sellers** but generates
**69.6% of marketplace GMV**.

This indicates a strong concentration of marketplace value among a relatively
small seller segment.

---

## Habit Formation & Early Retention

Seller activity during the first 30 days was analyzed as an early retention
signal.

After correcting for right-censoring:

| First 30-Day Sales | Active Next Period |
|---|---:|
| 1 sale | 34.5% |
| 2 sales | 54.3% |
| 3–5 sales | 71.5% |
| 6+ sales | 94.6% |

The relationship is near-linear: sellers who make more sales during their
first 30 days are more likely to remain active in the following period.

This is a **correlational finding**, not evidence of causation.

---

## Churn Trajectory

Among sellers with at least three active months, gradual declines were
observed before churn.

During the final three active months:

| Metric | Decline |
|---|---:|
| Orders | -35.6% |
| GMV | -41.2% |
| Product diversity | -24.7% |

This suggests that churn is often preceded by a gradual decline in seller
activity rather than occurring suddenly.

However, **535 sellers (18%) never made a second sale**, representing a
different churn pattern that is not captured by the gradual-decline analysis.

---

## Buyer-Seller Marketplace Dynamics

Buyer and seller activity was analyzed at the monthly marketplace level.

A bootstrap analysis with 1,000 resamples found:

### Same-Month Relationship

- Correlation: **r = 0.987**
- 95% CI: **[0.944, 0.996]**

The same-month relationship is statistically supported.

### One-Month-Lagged Relationship

- Correlation: **r = 0.031**
- 95% CI: **[-0.792, 0.568]**
- Monthly pairs: **18**

The confidence interval crosses zero, so the available data is insufficient
to determine whether buyer activity predicts seller activity one month ahead.

The conclusion is therefore **inconclusive**, rather than "no relationship."

---

## Engagement & Stickiness

An additional analysis applied the Amplitude Mastering Engagement framework
to the same seller/order data.

### Power User Curve

The seller activity distribution was polarized:

- **37.2%** of sellers were active in 80–100% of months.
- **19.2%** were active in only 0–20% of months.

Sellers active in 80–100% of months were Top Sellers at **34.5%**, compared
with **0.8%** among sellers active in only 0–20% of months.

This is an association rather than a predictive model because both measures
use the same underlying seller data and the Top Seller definition.

### Engagement Trigger Proxy

Fast Responders were defined as sellers with no more than 30 days between
sales.

Among measurable sellers:

- Fast Responders: **87%**
- Median GMV: **$1,508**
- Slow Responders median GMV: **$342**
- Median GMV difference: **4.4×**

Fast Responders also showed approximately half the churn rate of Slow
Responders.

Median GMV is used as the more reliable comparison because average GMV is
affected by outliers.

---

## Business Recommendations

### 1. New Seller Activation

Encourage new sellers to reach **3–6 sales during their first 30 days**.

The corrected analysis shows substantially higher next-period retention among
sellers with higher early activity.

### 2. At-Risk Intervention

Monitor sellers whose GMV and order activity decline by approximately
**35–40%** before churn.

This can provide an early intervention signal.

### 3. First-to-Second-Sale Conversion

Approximately **18% of sellers never make a second sale**.

This group represents a distinct activation problem and should be addressed
separately from gradual-decline churn.

### 4. Segment-Specific Retention

Top Sellers generate 69.6% of GMV and may justify higher-touch retention
strategies.

Dormant / One-Time sellers can be approached with lower-cost, automated
activation programs.

---

## Methodological Corrections

A major part of this project was reviewing and correcting potential
methodological issues.

### Right-Censoring

Sellers without sufficient follow-up time were excluded when calculating
retention and confirmed churn.

This changed the habit-formation results from the original estimates to:

**34.5% → 94.6%** across the four activity groups.

### Lifecycle Censoring

The most recent two months were excluded when confirming seller churn.

After correction:

- Current: **36.44%**
- Churned: **33.91%**

Current therefore became the largest lifecycle category.

### Confidence Intervals

Bootstrap confidence intervals were added to the buyer-seller correlation
analysis.

This changed the interpretation of the lagged relationship from "no
relationship" to **inconclusive due to the wide confidence interval**.

### Denominator Check

The engagement analysis distinguishes between:

- all sellers
- measurable sellers
- sellers with sufficient follow-up

This prevents percentages from being interpreted against the wrong
population.

---

## Limitations

- The pilot period was excluded, leaving a 20-month raw window and an
  18-month lifecycle analysis window.
- Buyer-seller analysis is performed at marketplace aggregation level because
  96.58% of buyers purchase from only one seller.
- Habit Formation and Churn Trajectory are correlational analyses, not
  predictive models.
- K-means segments describe associations in the observed data and should
  not be interpreted as causal groups.
- The Power User Curve and Value Concentration analyses use the same
  underlying dataset.
- Geographic retention differences were relatively modest and should not be
  interpreted as evidence that geography causes retention.
- Findings are specific to the Olist dataset from Brazil and should be
  validated against local marketplace data before being applied elsewhere.

---

## Key Takeaways

1. A small seller segment generates a large share of marketplace GMV.
2. Early seller activity is strongly associated with subsequent retention.
3. Churn can appear as a gradual decline in GMV, orders, and product
   diversity.
4. One-time sellers represent a distinct activation problem.
5. Same-month buyer and seller activity move together, while the one-month
   predictive relationship remains inconclusive.
6. Right-censoring and confidence intervals materially affect the
   interpretation of retention analysis.
7. K-means segmentation provides a practical framework for differentiated
   seller strategies.

---

## Technical Stack

- Python
- Pandas
- NumPy
- Scikit-learn
- K-means clustering
- RFM analysis
- Statistical analysis
- Bootstrap confidence intervals
- Cohort retention analysis
- Lifecycle analysis
- Excel
- Data visualization

---

## Project Structure

```text
seller-lifecycle-retention/
│
├── README.md
│
├── analysis/
│   └── seller_lifecycle_analytics.xlsx
│
├── dashboard/
│   └── seller_lifecycle_dashboard.pdf
│
└── report/
    └── seller_lifecycle_executive_summary.pdf
