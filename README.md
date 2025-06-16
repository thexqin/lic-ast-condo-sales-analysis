# ✨ lic-ast-condo-sales-analysis ✨

Welcome to the `lic-ast-condo-sales-analysis` repository\! 🏙️ This project delves into residential condo sales data for two dynamic neighborhoods in Queens, New York City: **Long Island City (LIC)** and **Astoria**. Using publicly available real estate transaction data, we aim to uncover insights into market trends, property values, and price per square foot.

Whether you're a real estate enthusiast, a data science learner, or just curious about NYC property, this repository offers a ready-to-use analytical pipeline.

## 🚀 Features

  * **Data Loading & Initial Exploration:** Effortlessly load raw sales data from CSV and get a quick overview.
  * **Robust Data Cleaning:** Automate the cleaning process by dropping irrelevant columns and converting data types for better analysis. 🧹
  * **Neighborhood-Specific Processing:** Focus on specific areas (LIC and Astoria) to analyze their unique market dynamics, filtering by relevant property types and sale conditions.
  * **External Data Integration:** Leverage the Socrata Open Data API to fetch crucial **Gross Square Footage (SQFT)** information for individual units, enhancing the accuracy of price calculations. 📊
  * **Price Per Square Foot Calculation:** Compute a standardized metric ($$/SQFT$) to compare property values across different units and neighborhoods.
  * **Data Export:** Export processed and enriched dataframes to CSV for further analysis or visualization. 💾

## 📊 Data Sources

This analysis relies on data from two primary sources:

  * **NYC Department of Finance (DOF) Rolling Sales Data:** The initial sales transaction data (`2020-2025.csv`) is sourced from the NYC Department of Finance, providing details on property sales, addresses, and prices.
  * **NYC Department of Buildings (DOB) - Socrata API:** We utilize the [NYC DOB - Building Permits Issuance](https://www.google.com/search?q=https://data.cityofnewyork.us/Housing-Development/DOB-Building-Permits-Issued/8y4t-faws) dataset via the Socrata API to programmatically retrieve gross square footage for specific apartment units.

## 🛠️ How to Use

To get started with this analysis, follow these steps:

### 1\. Clone the Repository

```bash
git clone https://github.com/thexqin/lic-ast-condo-sales-analysis.git
cd lic-ast-condo-sales-analysis
```

### 2\. Install Dependencies

Ensure you have Python 3.x installed. Then, install the necessary libraries using `pip`:

```bash
pip install pandas requests sodapy matplotlib
```

### 3\. Obtain a Socrata APP\_TOKEN

To use the Socrata API for fetching square footage data, you'll need an **APP TOKEN**.

  * Go to the [NYC OpenData website](https://data.cityofnewyork.us/).
  * Sign up for a free account.
  * Generate an App Token (usually found in your profile settings or when accessing API documentation).

Once you have your `APP_TOKEN`, you'll need to set it in the notebook. **For security, avoid hardcoding it directly.** While the provided notebook code uses `APP_TOKEN` directly, in a production setting, you would load this from an environment variable or a secure configuration file.

### 4\. Place Your Data

Make sure your `2020-2025.csv` file (containing the NYC DOF sales data) is in the root directory of the cloned repository.

### 5\. Run the Jupyter Notebook

Open and run the Jupyter notebook (`lic_ast_condo_sales_analysis.ipynb`) in your preferred environment (e.g., Jupyter Lab, VS Code with Python extension).

```bash
jupyter notebook
```

Execute each cell sequentially. The notebook will:

  * Load and clean the sales data.
  * Process data specifically for Long Island City and Astoria.
  * Call the Socrata API to enrich the data with square footage.
  * Calculate and display price per square foot.
  * Export the refined data to `2019-2025-lic.csv` and `2019-2025-ast.csv`.

## 📈 Insights

The analysis generates valuable metrics like the average price per square foot for condos in LIC and Astoria, allowing for direct comparison and trend observation. Visualizations (like box plots for $$/SQFT$) help in understanding the distribution and identifying outliers in property values.

## 🤝 Contributing

Contributions are welcome\! If you have suggestions for improvements, new features, or find any bugs, please feel free to open an issue or submit a pull request.

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for details.

Happy analyzing\! 🏠✨
