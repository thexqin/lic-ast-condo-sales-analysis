# ✨ LIC & Astoria Condo Sales Analysis ✨

Welcome to the `lic-ast-condo-sales-analysis` repository\! 🏙️ This project offers an in-depth look into residential condominium sales in two vibrant Queens neighborhoods: **Long Island City (LIC)** and **Astoria**. By analyzing publicly available real estate transaction data, we uncover key insights into market trends, property values, and the crucial metric of price per square foot.

Whether you're a real estate professional, a data science enthusiast, or simply curious about the NYC property market, this repository provides a ready-to-use analytical pipeline.

## 🌟 Project Overview

This Jupyter notebook-based project delivers a comprehensive analytical pipeline to:

  * **Acquire and Clean Data:** Ingest raw real estate transaction data from the NYC Department of Finance (DOF), performing robust cleaning and standardization.
  * **Enrich Data:** Integrate external data, specifically using the Socrata Open Data API to fetch crucial **Gross Square Footage (sqft)** information for individual units, which is essential for accurate price per square foot calculations.
  * **Analyze Market Trends:** Calculate and analyze price per square foot metrics for condo sales in LIC and Astoria, offering insights into neighborhood-specific market dynamics.
  * **Visualize Findings:** Generate insightful visualizations to compare property values and distributions across both neighborhoods, including a detailed focus on the prominent **Skyline Tower**.

## 🚀 Features

  * **Data Loading & Initial Exploration:** Easily load raw sales data and get a quick overview of its structure.
  * **Robust Data Cleaning:** Automate the cleaning process by dropping irrelevant columns and converting data types for better analysis.
  * **Neighborhood-Specific Processing:** Filter and process data for LIC and Astoria to analyze their unique market dynamics, focusing on specific property types and sale conditions.
  * **Address Standardization:** Clean and standardize address formats for consistency, which is crucial for successful API lookups.
  * **External Data Integration:** Leverage the Socrata Open Data API to fetch **Gross Square Footage (sqft)** for individual units, enhancing the accuracy of price calculations.
  * **Price Per Square Foot Calculation:** Compute a standardized $$/sqft$ metric to enable fair comparisons of property values.
  * **Outlier Handling:** Implement basic filtering to remove extreme outliers and focus on a more reasonable range of price per square foot values, ensuring more meaningful statistical descriptions and visualizations.
  * **Data Export:** Export processed and enriched dataframes to CSV for further analysis or reporting.

## 📊 Data Sources

This analysis relies on data from two primary sources:

  * **NYC Department of Finance (DOF) Rolling Sales Data:** The initial sales transaction data (`2020-2025.csv`) is sourced from the NYC Department of Finance, providing details on property sales, addresses, and prices. You can find this data on the [NYC.gov website](https://www.nyc.gov/site/finance/property/property-rolling-sales-data.page).
  * **NYC Department of Finance (DOF) - Property Valuation and Assessment Data (Socrata API):** We use the [NYC OpenData portal's Property Valuation and Assessment Data](https://data.cityofnewyork.us/City-Government/Property-Valuation-and-Assessment-Data-Tax-Classes/8y4t-faws/about_data) via its Socrata API to programmatically retrieve gross square footage for specific apartment units.

## 📁 Project Structure

```
.
├── lic_ast_condo_sales_analysis.ipynb  # Main Jupyter Notebook for analysis
├── lic_chart.png                      # Visualization for LIC $/sqft (sample)
├── lic_plot.png                       # Another visualization for LIC $/sqft (sample)
├── ast_chart.png                      # Visualization for Astoria $/sqft (sample)
├── ast_plot.png                       # Another visualization for Astoria $/sqft (sample)
├── skyline_tower_boxplot_full.png     # Boxplot for Skyline Tower $/sqft (full data)
├── skyline_tower_boxplot_filtered.png # Boxplot for Skyline Tower $/sqft (filtered)
├── skyline_tower_sales_by_month_year.png # Bar chart for Skyline Tower sales by month/year
└── README.md                          # This README file
```

## 🛠️ How to Use

To run this analysis, follow these steps:

### 1\. Clone the Repository

```bash
git clone https://github.com/thexqin/lic-ast-condo-sales-analysis.git
cd lic-ast-condo-sales-analysis
```

### 2\. Install Dependencies

Using a virtual environment is highly recommended for managing dependencies.

```bash
# Create a virtual environment
python -m venv venv

# Activate the virtual environment
# On macOS/Linux:
source venv/bin/activate
# On Windows:
.\venv\Scripts\activate

# Install dependencies from requirements.txt
pip install -r requirements.txt
```

**`requirements.txt` content:**

```
pandas
requests
sodapy
matplotlib
seaborn
jupyter
```

### 3\. Obtain a Socrata APP\_TOKEN

To fetch square footage data from the Socrata API, you'll need an **APP TOKEN**.

  * Visit the [NYC OpenData website](https://data.cityofnewyork.us/).
  * Sign up for a free account.
  * Generate an App Token (typically found in your profile settings or API documentation).

**Important:** While the notebook code uses `APP_TOKEN` directly for demonstration, it's best practice to load this from an **environment variable** or a secure configuration file in a production setting to avoid hardcoding credentials in public repositories.

### 4\. Place Your Data

Ensure your `2020-2025.csv` file (containing the NYC DOF sales data) is located in the root directory of your cloned repository.

### 5\. Run the Jupyter Notebook

Open and execute the Jupyter notebook (`lic_ast_condo_sales_analysis.ipynb`). You can do this by running:

```bash
jupyter notebook
```

Execute each cell sequentially. The notebook will:

  * Load and clean the sales data.
  * Process data specifically for Long Island City and Astoria.
  * Call the Socrata API to enrich the data with square footage.
  * Calculate and display price per square foot.
  * Generate visualizations.
  * Export the refined data to `2018-2025-lic.csv` and `2018-2025-ast.csv` (these will be created in the root directory).

## ⚙️ Data Processing and Analysis Highlights

The notebook performs several key steps to transform raw data into actionable insights:

### Data Loading & Initial Inspection

```python
import pandas as pd

df = pd.read_csv('2020-2025.csv', parse_dates=['SALE DATE'])
df.info()
```

### Data Cleaning & Type Conversion

This step involves dropping unnecessary columns like `EASEMENT` and converting columns such as `ZIP CODE`, `YEAR BUILT`, and `GROSS SQUARE FEET` to appropriate integer types for better handling.

```python
df = df.drop(columns=['EASEMENT'])
df['ZIP CODE'] = df['ZIP CODE'].astype('Int64')
df['YEAR BUILT'] = df['YEAR BUILT'].astype('Int64')
df['GROSS SQUARE FEET'] = df['GROSS SQUARE FEET'].astype('Int64')
```

### Neighborhood-Specific Filtering & Standardization

The `process_neighborhood_data` function filters the main dataset for specific neighborhoods (Long Island City, Astoria) and property characteristics (e.g., `APARTMENT NUMBER` present, `BUILDING CLASS AT PRESENT` is 'R4', valid `SALE PRICE`). It then standardizes address components to ensure consistency for API lookups.

```python
def process_neighborhood_data(df, neighborhood_name, min_year_built=None):
    # ... (function definition as provided in previous output) ...
    return df_processed

dflic = process_neighborhood_data(df, 'LONG ISLAND CITY', 2018)
dfast = process_neighborhood_data(df, 'ASTORIA', 2018)

# Further address standardization
dflic['housenum'] = dflic['housenum'].str.strip().replace(r'^(\d{2})(\d{2})$', r'\1-\2', regex=True)
dflic['streetname'] = dflic['streetname'].str.strip().replace(r'ST$', 'STREET', regex=True)
# ... (similar cleaning for dfast and specific oddball addresses) ...
```

### Fetching Square Footage Data with Socrata API

A multithreaded function `get_sqft_multithreaded_sodapy` is used to efficiently query the Socrata API for `gross_sqft` based on `housenum_lo`, `street_name`, and `aptno`. This greatly enhances the accuracy of our price per square foot calculations.

```python
from sodapy import Socrata
from concurrent.futures import ThreadPoolExecutor

# ... (DATA_URL, DATA_SET, client, MAX_WORKERS definitions) ...

def check_aptno_sodapy(housenum_lo, street_name, aptno):
    # ... (function definition as provided in previous output) ...
    return sqft_value

def get_sqft_multithreaded_sodapy(df):
    # ... (function definition as provided in previous output) ...
    return sqft_values

dflic['sqft'] = get_sqft_multithreaded_sodapy(dflic)
dfast['sqft'] = get_sqft_multithreaded_sodapy(dfast)

dflic = dflic.dropna(subset=['sqft'])
dfast = dfast.dropna(subset=['sqft'])
```

### Price Per Square Foot Calculation & Neighborhood Analysis

After obtaining square footage, the $$/sqft$ metric is calculated. Descriptive statistics and box plots are then generated, with an initial filter applied to remove extreme outliers and focus on a more typical range of values for a "reasonable price analysis."

```python
# (Reload processed data if starting here)
dflic = pd.read_csv('2018-2025-lic.csv', parse_dates=['SALE DATE'])
dfast = pd.read_csv('2018-2025-ast.csv', parse_dates=['SALE DATE'])

dflic['$/sqft'] = round(dflic['SALE PRICE'] / dflic['sqft'])
dfast['$/sqft'] = round(dfast['SALE PRICE'] / dfast['sqft'])

print("LIC Data Description (filtered for reasonable $/sqft):")
print(dflic[(dflic['$/sqft']>500) & (dflic['$/sqft']<3000)].describe())
dflic[(dflic['$/sqft']>500) & (dflic['$/sqft']<3000)]['$/sqft'].plot.box(title='LIC 2018-2025 $ / sqft', vert=False)

print("\nAstoria Data Description (filtered for reasonable $/sqft):")
print(dfast[(dfast['$/sqft']>500) & (dfast['$/sqft']<2400)].describe())
dfast[(dfast['$/sqft']>500) & (dfast['$/sqft']<2400)]['$/sqft'].plot.box(title='Astoria 2018-2025 $ / sqft', vert=False)
```

## 📈 Insights & Visualizations

The analysis culminates in key metrics like the median price per square foot for condos in LIC and Astoria. Box plots help visualize these distributions, identify typical price ranges, and highlight outliers.

### Long Island City (LIC) 2018-2025 $/sqft

This plot shows the distribution of price per square foot for condo sales in Long Island City. The central line indicates the **median**, the box represents the **interquartile range (IQR)**, and the "whiskers" show data variability outside the IQR. Outliers are plotted as individual points.

### Astoria (AST) 2018-2025 $/sqft

This plot illustrates the distribution of price per square foot for condo sales in Astoria, using the same conventions as the LIC plot.

## 🏢 Deep Dive: Skyline Tower (23-15 44TH DRIVE, LIC)

A specific analysis of condo sales in **Skyline Tower** (23-15 44TH DRIVE) was conducted due to its significance in the LIC market. This involved:

  * Filtering the LIC dataset for the specific address.
  * Carefully cleaning the data to exclude **"bulk sales"** (transactions with unusually high `SALE PRICE` values that represent multiple units sold to a single entity, which would skew $$/sqft$ calculations for individual units). Outliers, like transactions above $2,610,739 or below $492,000, were identified and removed for this specific analysis.
  * Analyzing the descriptive statistics and distribution of $$/sqft$ for individual unit sales within Skyline Tower.
  * Visualizing monthly sales trends for this building.

### Skyline Tower Price Analysis (Filtered Data)

```python
# Reload and EDA for Skyline Tower
df2315 = pd.read_csv('23-15.csv', parse_dates=['SALE DATE'])

# Filter out bulk sales and unrealistic low prices for individual unit analysis
df2 = df2315[(df2315['SALE PRICE'] <= 2610739) &
             (df2315['SALE PRICE'] >= 492000)
             ].copy()
df2 = df2.drop(columns=['ZIP CODE', 'YEAR BUILT']).reset_index(drop=True)

print("\nSkyline Tower (23-15 44TH DRIVE) Price Analysis (Individual Units):")
print(df2.describe())
```

**Output:**

```
Skyline Tower (23-15 44TH DRIVE) Price Analysis (Individual Units):
       SALE PRICE                SALE DATE         sqft       $/sqft
count  7.440000e+02                      744   744.000000   744.000000
mean   1.273043e+06  2022-07-01 20:00:00.000  795.748656  1606.881720
min    4.920000e+05  2020-07-06 00:00:00.000  400.000000  1126.000000
25%    9.688585e+05  2021-08-02 18:00:00.000  635.000000  1427.750000
50%    1.201484e+06  2022-02-28 12:00:00.000  724.000000  1587.500000
75%    1.563686e+06  2023-05-24 18:00:00.000  972.000000  1754.000000
max    2.610739e+06  2025-05-30 00:00:00.000 1326.000000  4928.000000
std    3.980797e+05                      NaN  220.398092   268.356534
```

### Skyline Tower $/sqft Distribution

```python
df2['$/sqft'].plot.box(title='Skyline Tower (23-15 44TH DRIVE) $ / sqft', vert=False)
```

To get an even clearer picture, a box plot excluding very high outliers in $$/sqft$ (above $2500) is also provided:

```python
df2[df2['$/sqft'] < 2500]['$/sqft'].plot.box(title='Skyline Tower (23-15 44TH DRIVE) $ / sqft (Excluding High Outliers)', vert=False)
```

### Skyline Tower Sales Trend by Month and Year

```python
df2['month'] = df2['SALE DATE'].dt.month
df2['year'] = df2['SALE DATE'].dt.year
df2.groupby(['year', 'month']).size().plot.bar(figsize=(24, 6), rot=45, title='Skyline Tower Sales by Month')
```

## 💡 Key Findings

Based on the processed residential condo sales data from 2018 to 2025, with valid sale prices and integrated square footage:

  * **Long Island City (LIC):** Showed a median price per square foot ($$/sqft$) of approximately **$1462**. This is based on 1727 transactions within a reasonable $$/sqft$ range ($500 - $3000), indicating a robust market.
  * **Astoria:** Recorded a lower median price per square foot of about **$1162**. This was derived from 374 transactions within its defined $$/sqft$ range ($500 - $2400).

These findings suggest that, on average, LIC condominiums commanded a higher price per square foot than those in Astoria during the analyzed period. This could be due to factors like LIC's newer developments, closer proximity to Manhattan, and extensive amenities.

For **Skyline Tower (23-15 44TH DRIVE)**, after meticulously filtering out bulk sales and other extreme outliers, the median price per square foot for individual units stands at approximately **$1588** across 744 transactions. The sales trend visualization provides a granular view of this specific building's market performance over time.

## ➡️ Next Steps & Future Enhancements

  * **Time-Series Analysis:** Implement more advanced time-series analysis to identify seasonal patterns, long-term trends, and market shifts.
  * **Feature Engineering:** Explore the impact of additional property features, such as building age, number of bedrooms/bathrooms, and floor level, on sale prices.
  * **Granular Geographical Analysis:** Conduct more detailed geographical segmentation within each neighborhood, perhaps by sub-areas or proximity to key landmarks and transit hubs.
  * **Additional Data Integration:** Incorporate external datasets, such as demographic information, economic indicators, or public transit access data, to enrich insights.
  * **Interactive Visualizations:** Develop interactive plots using libraries like Plotly or Bokeh for more dynamic data exploration.
  * **Machine Learning Modeling:** Build predictive models to estimate property values based on a comprehensive set of features.

## 🤝 Contributing

Contributions are always welcome\! If you have suggestions for improvements, new features, or identify any bugs, please feel free to open an issue or submit a pull request. Please ensure your contributions align with the project's goals and maintain high code quality.

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for details.

Happy analyzing\! What other aspects of these neighborhoods are you curious about?
