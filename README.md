# Mall Customer Segmentation using Clustering

Groups 200 shopping-mall customers into segments that behave alike, so marketing can target each one differently. This is an unsupervised learning project comparing **K-Means** and **Hierarchical (Ward) clustering**.

## Problem

The mall has each customer's age, annual income and a spending score (1–100) that it assigns from purchase behaviour. There are no labels saying which customers are "VIP" or "budget", so the goal is to *discover* natural groups and turn them into actionable segments.

## Dataset

`Mall_Customers.csv`: 200 customers, no missing values or duplicates.

| Column | Description |
|---|---|
| CustomerID | Unique ID (used as the index, not as a feature) |
| Gender | Male / Female (56% female) |
| Age | 18–70 years |
| Annual Income (k$) | 15–137 thousand dollars |
| Spending Score (1-100) | Higher means the customer spends more |

## Approach

1. **Exploratory data analysis:** distributions, correlations, gender comparison and a skewness check.
2. **Clustering on four feature sets:** Income + Spending, Age + Spending, Age + Income, and all three (standardised).
3. **Choosing the number of clusters** with four methods together: the elbow method, silhouette score, Davies–Bouldin index and dendrograms.
4. **Robustness check:** both algorithms are compared with the Adjusted Rand Index (ARI). When two different methods find the same groups, the groups are real.
5. **Profiling:** the average customer in each cluster, translated into named segments with marketing actions.

## Key results

| Feature set | k | Silhouette | K-Means vs Hierarchical (ARI) |
|---|---|---|---|
| Income + Spending | 5 | **0.554** | **0.94** |
| Age + Spending | 4 | 0.500 | 0.79 |
| Age + Income | 4 | 0.434 | 0.57 (weak structure) |
| All three (scaled) | 6 | 0.428 | 0.89 |

- Income and spending have almost **zero correlation**, yet they form **five very clear clusters**. Correlation alone can't reveal cluster structure.
- Every high spender is under about 40 years old.
- Gender barely differs between segments, so it isn't used as a clustering feature.

## Customer segments (final model: K-Means, k = 6)

| Segment | Avg age | Avg income (k$) | Avg score | Size | Suggested action |
|---|---|---|---|---|---|
| Affluent big spenders | 33 | 87 | 82 | 39 | VIP / loyalty programme, premium brands |
| Affluent but cautious | 42 | 89 | 17 | 33 | Biggest growth opportunity: personalised, quality-led offers |
| Young, low income, high spend | 25 | 25 | 78 | 23 | Trend-led affordable products, social media |
| Low income, low spend | 46 | 26 | 19 | 21 | Value deals and essentials |
| Older average customers | 56 | 54 | 49 | 45 | Comfort, convenience, service-led promotions |
| Younger average customers | 27 | 57 | 48 | 39 | Bundles and dining/entertainment offers to move them up |

**Recommended model: K-Means.** It matches or beats hierarchical clustering on silhouette, agrees strongly with it, and can assign new customers to a segment with `predict()`.

## Tech stack

Python · pandas · NumPy · scikit-learn · SciPy · Matplotlib · seaborn · Plotly

## How to run

    pip install pandas numpy scikit-learn scipy matplotlib seaborn plotly jupyter
    jupyter notebook Mall_Customer_Segmentation.ipynb

Run all cells from top to bottom. The automated profiling section is optional and needs `ydata-profiling`, which supports Python ≤ 3.13; it is skipped automatically if the package isn't installed.

## Limitations and next steps

- Small sample (200 customers) with only three features; real segmentation would add purchase behaviour such as RFM (Recency, Frequency, Monetary).
- Clusters are validated with statistics only. The next step is to test segment-specific campaigns with A/B tests.
- Possible extensions: Gaussian Mixture Models, DBSCAN, K-Prototypes (to include Gender), and saving the model to segment new customers automatically.

## Repository structure

    ├── Mall_Customer_Segmentation.ipynb   # Full analysis
    ├── Mall_Customers.csv                 # Dataset
    └── README.md
