# Part B: Business Case Analysis

## B1. Problem Formulation

### (a) Machine Learning Problem

The target variable is **items_sold**, which represents the number of products sold per store per month.

Candidate input features include:

* Store attributes: store_size, location_type, competition_density
* Promotion details: promotion_type
* Temporal features: month, is_weekend, is_festival
* Customer behavior indicators: footfall, past sales trends

This is a **supervised regression problem** because the goal is to predict a continuous numerical value (items sold). Regression is appropriate since we are estimating quantity rather than classifying categories.

---

### (b) Why Items Sold is Better than Revenue

Revenue can be influenced by pricing, discounts, and product mix, which may not reflect actual customer demand. For example, a high discount may increase items sold but reduce revenue per item.

Using **items sold (sales volume)** is more reliable because it directly measures customer response to promotions.

This illustrates an important principle in machine learning:
 *The target variable should align closely with the true business objective and not be distorted by external factors.*

---

### (c) Alternative Modelling Strategy

Instead of using a single global model, a better approach is a **segmented or hierarchical model**.

For example:

* Build separate models for **urban, semi-urban, and rural stores**, OR
* Include store-level features and interactions in a more advanced model

This is justified because customer behavior and promotion effectiveness vary significantly by location. A single model may fail to capture these differences.

---

## B2. Data and EDA Strategy

### (a) Data Joining and Aggregation

The four datasets (transactions, store attributes, promotion details, and calendar) would be joined using common keys such as:

* store_id
* transaction_date

The final dataset grain would be:
 **One row per store per month**

Aggregations performed:

* Total items_sold per store per month
* Average basket size
* Monthly footfall
* Promotion applied during that month

---

### (b) Exploratory Data Analysis (EDA)

1. **Promotion vs Sales Plot**

   * Compare items sold across promotion types
   * Helps identify which promotions are most effective

2. **Correlation Heatmap**

   * Shows relationships between variables
   * Helps in feature selection

3. **Time Series Plot (Sales over Time)**

   * Detect trends and seasonality
   * Useful for creating time-based features

4. **Boxplot by Location Type**

   * Compare sales distribution across urban/rural stores
   * Helps justify segmentation strategy

These analyses guide feature engineering and model selection.

---

### (c) Handling Imbalance (No Promotion Data)

Since 80% of data has no promotion:

* The model may become biased toward predicting low promotion impact

To fix this:

* Use **balanced sampling**
* Add a binary feature: “promotion applied or not”
* Possibly apply **weighting techniques**

---

## B3. Model Evaluation and Deployment

### (a) Train-Test Split and Metrics

Use a **time-based split**:

* Train on first ~2.5 years
* Test on last ~6 months

A random split is inappropriate because it mixes past and future data, causing **data leakage**.

Metrics:

* **RMSE**: Penalizes large errors (important for big sales mistakes)
* **MAE**: Easy to interpret average error
* **R² (optional)**: Measures how well the model explains variation

---

### (b) Explaining Model Recommendations

Feature importance can explain why different promotions are suggested.

For example:

* December → festivals → high demand → Loyalty Points works
* March → lower demand → Flat Discount boosts sales

We can:

* Check feature importance from the model
* Use SHAP or similar tools for explanation

This helps communicate insights clearly to the marketing team.

---

### (c) Deployment Process

1. **Save Model**

   * Use joblib or pickle to store trained pipeline

2. **Monthly Prediction**

   * Collect new data (store + calendar + promotions)
   * Apply same preprocessing
   * Generate predictions

3. **Automation**

   * Schedule monthly runs

4. **Monitoring**

   * Track prediction vs actual sales
   * Monitor RMSE over time

5. **Retraining**

   * Retrain model when performance drops
   * Or periodically (e.g., every 3–6 months)

---

# Conclusion

A well-designed ML system combining proper feature engineering, segmentation, and monitoring can significantly improve promotion effectiveness and maximize sales across different store types.
