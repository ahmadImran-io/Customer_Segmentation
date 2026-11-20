# Customer Segmentation with K-Means

This project segments customers from the UCI Online Retail dataset using RFM and behavioural features. It uses K-Means clustering to group customers according to purchase value, frequency, recency, product diversity, and buying activity.

## Dataset

The notebook loads the dataset directly from:

- UCI Machine Learning Repository: [Online Retail](https://archive.ics.uci.edu/dataset/352/online+retail)
- Source file: `https://archive.ics.uci.edu/static/public/352/data.csv`

The dataset contains approximately 541,000 transaction records from a UK-based online gift retailer.

## Workflow

1. Load and inspect the transaction data.
2. Explore missing values, duplicates, quantities, prices, and country distribution.
3. Clean invalid transactions:
   - Remove rows with no customer ID.
   - Remove duplicate rows.
   - Remove cancelled invoices.
   - Remove non-positive quantity or unit-price values.
   - Remove records with invalid StockCode values.
4. Create one feature row per customer.
5. Cap outliers, apply a log transformation, and standardise features.
6. Compare values of K with inertia, silhouette, Davies-Bouldin, and Calinski-Harabasz metrics.
7. Train the final K-Means model and relabel clusters in descending monetary value order.
8. Profile segments using customer counts, revenue, feature averages, and medians.
9. Export the final customer segments to a CSV file.

## Customer Features

| Feature | Description |
|---|---|
| Recency | Days since the customer's latest purchase |
| Frequency | Number of unique invoices |
| Monetary | Total customer spending |
| AverageOrderValue | Monetary value divided by invoice count |
| TotalQuantity | Total quantity purchased |
| UniqueProducts | Number of unique StockCode values |
| AverageBasketSize | Total quantity divided by invoice count |
| ActiveMonths | Number of distinct months with purchases |
| CustomerTenure | Days between the first and last purchase |

## Usage

### Requirements

Python with the following packages:

```text
pandas
numpy
matplotlib
seaborn
scikit-learn
jupyter
```

Install the dependencies:

```powershell
python -m pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

### Run the notebook

```powershell
python -m jupyter notebook customer_segmentation.ipynb
```

Run all cells from top to bottom. The notebook downloads the dataset over the internet and does not require a local input file.

## Output

The final cell exports:

```text
customer_segments.csv
```

This file contains one row per customer and includes the customer ID, country, cluster label, and clustering features.

## Results

The notebook compares candidate cluster counts using:

- **Inertia** to identify the elbow point
- **Silhouette score** to measure cluster separation
- **Davies-Bouldin score** to measure within-cluster compactness
- **Calinski-Harabasz score** to compare cluster and within-cluster variance

Cluster 2 is excluded as the final choice because it generally reduces the analysis to a coarse split between active and inactive customers. The model evaluates `K >= 3` and selects the candidate with the highest silhouette score.

Customer data typically forms a continuum rather than distinct groups, so a silhouette score around `0.25-0.30` indicates moderate structure. The final segment profiles should be interpreted as actionable behavioural groups rather than perfectly separated customer populations.

## Cluster Visualization

The notebook generates the following visualizations:

- PCA plot showing customers in two dimensions
- Bar chart showing the number of customers in each cluster
- Segment feature heatmap comparing each cluster with the overall customer average
- Mean and median feature tables for each segment

Run every notebook cell to display these visualizations:

```powershell
python -m jupyter notebook customer_segmentation.ipynb
```

When using the notebook in Jupyter or VS Code, these charts appear directly below the corresponding code cells.

## Files

```text
Customer_Segmentation/
|-- customer_segmentation.ipynb
|-- README.md
|-- customer_segments.csv  (created after running the notebook)
```
