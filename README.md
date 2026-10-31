# Business Analytics on the Olist E-Commerce Dataset

## Overview

This project performs an end-to-end business analytics study of the
**Brazilian Olist e-commerce dataset** using Python. The Jupyter
Notebook (`BA(German_Dataset).ipynb`) combines data cleaning,
exploratory analysis, customer analytics, sales forecasting, product
performance analysis, payment behaviour, delivery performance, and
review analysis.

The objective is to turn transactional e-commerce data into business
insights that can support decisions around revenue growth, customer
retention, product strategy, payment options, and delivery experience.

> **Dataset note:** Although the notebook filename contains
> `German_Dataset`, the CSV filenames and fields used in the notebook
> correspond to the Olist Brazilian e-commerce dataset. Update the
> notebook filename or this description if a different dataset was
> intended.

## Analyses Included

### 1. Data loading and preparation

The notebook loads the following source tables:

-   `olist_orders_dataset.csv`
-   `olist_order_items_dataset.csv`
-   `olist_products_dataset.csv`
-   `olist_customers_dataset.csv`
-   `olist_order_payments_dataset.csv`
-   `olist_order_reviews_dataset.csv`
-   `olist_geolocation_dataset.csv`

Preparation steps include: - Inspecting dataset dimensions, sample
records, data types, and missing values. - Aggregating geolocation
records to one row per ZIP-code prefix using mean latitude and
longitude, and the first city/state values. - Removing exact duplicate
rows. - Handling selected missing values (for example, removing records
with missing payment values and filling missing payment types or product
categories). - Converting order date fields to datetime. - Restricting
the order data to delivered orders with a recorded customer delivery
date. - Joining orders with customers, payments, order items, and
products to create a consolidated analysis table.

### 2. Sales analysis

-   Monthly revenue trends.
-   Revenue by product category, including the top 10 categories.
-   Average Order Value (AOV).
-   Revenue by customer state.
-   Cumulative revenue growth over time.

### 3. Customer analysis

-   Monthly trend of new versus repeat customers.
-   Monthly cohort retention analysis, represented as a retention
    heatmap.
-   RFM (Recency, Frequency, Monetary) customer segmentation into:
    -   Champions
    -   Loyal Customers
    -   Big Spenders
    -   At Risk
    -   Others

### 4. Sales forecasting

Monthly revenue is modelled using two approaches: - **Prophet**, with
yearly seasonality enabled. - **ARIMA**, using an initial `(1, 1, 1)`
model specification.

The notebook forecasts the next six months and compares Prophet and
ARIMA using: - Root Mean Squared Error (RMSE) - Mean Absolute Percentage
Error (MAPE)

### 5. Customer Lifetime Value (CLV)

-   Overall AOV, purchase frequency, and average customer lifespan.
-   Estimated customer-level CLV.
-   CLV comparison across RFM customer segments.

The notebook uses the relationship:

`CLV = AOV × Purchase Frequency × Customer Lifespan`

### 6. Product analysis

-   Top 10 products by revenue.
-   Top 10 product categories by revenue.
-   Pareto analysis to examine the cumulative contribution of products
    to revenue and the 80% revenue threshold.
-   High-AOV products (niche/premium product analysis).
-   Comparison of high-revenue bestsellers and high-AOV products.

### 7. Payment method analysis

-   Order count and revenue by payment type.
-   Order and revenue shares by payment type.
-   Distribution of payment instalments.
-   Average payment value by instalment count.

### 8. Delivery performance

-   Actual delivery time and estimated delivery time.
-   Delivery delay relative to the estimated delivery date.
-   Proportion of late versus on-time/early deliveries.
-   Average delivery delay by product category.
-   Comparison of review scores for late and on-time/early deliveries.

### 9. Review analysis

-   Review-score distribution and average review score.
-   Review-score comparison between one-time and repeat customers.
-   Word clouds for positive reviews (score 4 or higher) and negative
    reviews (score 2 or lower), based on available review comments.

## Tech Stack

-   Python
-   Jupyter Notebook
-   pandas
-   NumPy
-   Matplotlib
-   Seaborn
-   Prophet
-   statsmodels
-   WordCloud

The notebook also uses `mean_squared_error` from scikit-learn for
forecast evaluation.

## Repository Structure

Place the notebook and the dataset CSV files in your project directory.
A suggested structure is:

``` text
project/
├── README.md
├── BA(German_Dataset).ipynb
└── data/
    ├── olist_orders_dataset.csv
    ├── olist_order_items_dataset.csv
    ├── olist_products_dataset.csv
    ├── olist_customers_dataset.csv
    ├── olist_order_payments_dataset.csv
    ├── olist_order_reviews_dataset.csv
    └── olist_geolocation_dataset.csv
```

The structure above is a recommendation; the notebook currently reads
the CSV files from `/content/`, which is commonly used in Google Colab.
If running locally, change the paths in the data-loading cell to match
your folder structure.

## Getting Started

### 1. Clone or download the project

``` bash
git clone <repository-url>
cd <repository-folder>
```

If you are not using Git, download the notebook and dataset files into
the suggested structure.

### 2. Install dependencies

``` bash
pip install pandas numpy matplotlib seaborn prophet statsmodels scikit-learn wordcloud jupyter
```

Depending on your environment, Prophet may require additional system
dependencies. Refer to the Prophet installation documentation if its
installation fails.

### 3. Add the dataset files

Download or obtain the Olist CSV files listed in the **Data loading and
preparation** section. Put them in the `data/` folder, or place them in
the location expected by your notebook.

### 4. Update file paths

For a local project using the suggested structure, update the loading
cell to use paths such as:

``` python
from pathlib import Path
DATA_DIR = Path("data")

orders = pd.read_csv(DATA_DIR / "olist_orders_dataset.csv")
order_items = pd.read_csv(DATA_DIR / "olist_order_items_dataset.csv")
products = pd.read_csv(DATA_DIR / "olist_products_dataset.csv")
customers = pd.read_csv(DATA_DIR / "olist_customers_dataset.csv")
payments = pd.read_csv(DATA_DIR / "olist_order_payments_dataset.csv")
reviews = pd.read_csv(DATA_DIR / "olist_order_reviews_dataset.csv")
geolocation = pd.read_csv(DATA_DIR / "olist_geolocation_dataset.csv")
```

### 5. Run the notebook

``` bash
jupyter notebook
```

Open `BA(German_Dataset).ipynb` and run the cells from top to bottom.
Running cells in order is important because later analyses depend on
dataframes created during preprocessing.

## Important Implementation Notes

-   **Revenue aggregation:** The notebook joins order-level payments
    with order items. An order can contain multiple items and can have
    multiple payment records. This can multiply rows during joins and
    may duplicate payment values when revenue is summed from the merged
    table. Validate revenue calculations at the correct grain (order,
    payment, or item) before interpreting them as financial totals.

-   **Forecasting imports:** The notebook uses `np` and
    `mean_squared_error` in the forecasting evaluation cells. Ensure
    these are imported before those cells run:

    ``` python
    import numpy as np
    from sklearn.metrics import mean_squared_error
    ```

-   **Forecast evaluation:** The notebook's forecast comparison uses a
    six-month test period. Confirm that the train/test split, forecast
    frequency, and date alignment are consistent before comparing the
    metrics.

-   **CLV interpretation:** The notebook estimates lifespan from the
    time between a customer's first and last observed purchases, with a
    minimum of one month. For customers with only one observed purchase,
    this is a modelling assumption rather than a measured long-term
    relationship.

-   **Delivered-order filter:** The consolidated master dataset is
    restricted to delivered orders. Analyses based on this table
    therefore describe delivered orders, not every order placed on the
    platform.

-   **Review comments:** Word clouds depend on non-empty review comment
    text. Missing comments are retained during preprocessing and
    excluded when the text is combined.

## Outputs

The notebook displays tables, summary statistics, and visualizations
inline. These include sales trend charts, category/product rankings,
customer segmentation and cohort heatmaps, forecast plots, payment
charts, delivery comparisons, review distributions, and review-comment
word clouds.

The notebook does not define a separate export pipeline for saving all
charts or result tables to files.

## Scope and Limitations

-   Results depend on the supplied Olist CSV files and the preprocessing
    choices in the notebook.
-   The notebook is primarily exploratory and descriptive; observed
    associations (for example, delivery delay versus review score)
    should not automatically be interpreted as causal relationships.
-   Forecast performance depends on the available monthly history and
    the selected model specifications. The initial ARIMA order is not
    presented as an optimally tuned model.
-   Customer retention and CLV estimates are based on the observed
    transaction window and should be interpreted within that limitation.

## Author / Contributors

Add the project author(s), course information, institution, and date
here.
