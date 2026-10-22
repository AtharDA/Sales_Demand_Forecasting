# Sales & Demand Forecasting

## Project Overview

This project explores historical weekly sales data and builds baseline forecasting models to compare their performance. It uses the Store Sales - Time Series Forecasting dataset from the Corporación Favorita retail forecasting competition on Kaggle.

The project follows a step-by-step workflow: data understanding and exploratory data analysis (EDA), feature engineering, model building, and model evaluation.

## Objectives

- Understand historical sales trends and patterns.
- Prepare weekly sales data for forecasting.
- Build a Naive Baseline and a Linear Regression model.
- Evaluate and compare model predictions using regression metrics.
- Identify limitations and potential improvements.

## Dataset

The project uses the following source files from the Kaggle dataset:

- `train.csv` — daily sales observations, including date, store, product family, sales, and promotion information.
- `stores.csv` — store-related information.

The raw training dataset contains approximately 3 million rows. For this project, sales are aggregated by week to create a smaller time series for modeling.

**Date coverage:** January 2013 to August 2017. The final incomplete week is excluded during feature engineering.

Dataset source: [Store Sales - Time Series Forecasting (Kaggle)](https://www.kaggle.com/competitions/store-sales-time-series-forecasting)

Please download the dataset from Kaggle and place the required raw files in `data/raw/`. The dataset is not included in this repository.

## Project Structure

```text
Sales_Demand_Forecasting/
├── data/
│   ├── raw/
│   │   ├── train.csv
│   │   └── stores.csv
│   └── processed/
│       ├── weekly_sales.csv
│       └── weekly_sales_features.csv
├── notebooks/
│   ├── 1_Data_Understanding_and_EDA.ipynb
│   ├── 2_Feature_Engineering.ipynb
│   ├── 3_Model_Building.ipynb
│   └── 4_Model_Evaluation_and_Insights.ipynb
├── outputs/
│   └── model_predictions.csv
├── models/
├── README.md
└── requirements.txt
```

Generated files may appear only after the notebooks have been run. The `models/` directory is reserved for saved model artifacts if added later.

## Workflow

### 1. Data Understanding and EDA

- Load and inspect the raw sales and store datasets.
- Check for missing values and duplicate rows.
- Explore daily and weekly sales trends.
- Review yearly sales patterns and sales by product family.
- Aggregate daily sales into weekly sales.
- Save the weekly sales data to `data/processed/weekly_sales.csv`.

### 2. Feature Engineering

- Load the weekly sales data.
- Remove the incomplete final week.
- Create calendar features such as `year`, `month`, and `week_of_year`.
- Create a continuous `time_index` for modeling the long-term trend.
- Save the prepared dataset to `data/processed/weekly_sales_features.csv`.

### 3. Model Building

The data is sorted by date and split chronologically:

- **Training set:** 192 weekly observations (first 80%).
- **Testing set:** 49 weekly observations (final 20%).

Two models are built:

- **Naive Baseline:** uses the last observed sales value from the training period as the prediction for each test week.
- **Linear Regression:** uses `time_index` as the input feature to model the overall sales trend.

Predictions are saved to `outputs/model_predictions.csv`.

### 4. Model Evaluation

The models are evaluated using:

- **MAE (Mean Absolute Error):** average absolute prediction error.
- **RMSE (Root Mean Squared Error):** gives greater weight to larger errors.
- **R²:** compares model performance with a test-set mean prediction benchmark.

## Results

Evaluation results from the current notebook run:

| Model | MAE | RMSE | R² |
|---|---:|---:|---:|
| Naive Baseline | 385,162.65 | 523,645.65 | -0.11 |
| Linear Regression | 551,384.32 | 637,005.79 | -0.64 |

The Naive Baseline performed better than Linear Regression on all three reported metrics. Both models produced negative R² scores, indicating that neither model outperformed the benchmark of predicting the mean of the actual test-period sales.

The actual-versus-predicted chart also shows that weekly sales fluctuate substantially. The Naive Baseline produces a constant prediction, while Linear Regression captures a gradual upward trend but misses many of the fluctuations.

These results are a baseline comparison, not evidence of a highly accurate forecasting system.

## Limitations and Future Improvements

- Linear Regression currently uses only a continuous time index, so it does not capture seasonal patterns or short-term fluctuations.
- The Naive Baseline repeats the last training-period sales value for all test weeks.
- Future work could add seasonal and calendar features, compare additional forecasting approaches, and validate results across different time periods.
- Further feature engineering and model evaluation would be needed before using forecasts for operational decisions.

## How to Run

1. Download the dataset from the Kaggle competition page linked above.
2. Place `train.csv` and `stores.csv` in `data/raw/`.
3. Install the required Python packages:

   ```bash
   pip install -r requirements.txt
   ```

4. Open Jupyter Notebook from the project directory:

   ```bash
   jupyter notebook
   ```

5. Run the notebooks in order, from Notebook 1 through Notebook 4. Each notebook depends on files generated by the preceding notebook.

## Tools and Technologies

- Python
- pandas
- NumPy
- Matplotlib
- scikit-learn
- Jupyter Notebook

## Dataset

This project uses the Store Sales - Time Series Forecasting
dataset from Kaggle.

Download the dataset here:
https://www.kaggle.com/competitions/store-sales-time-series-forecasting/data

After downloading, place `train.csv` and `stores.csv` inside `data/raw/`.

## Author

**Mohammed Athar Ansari**
