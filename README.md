# Market Share Analysis by Region

![Kaggle Badge](https://img.shields.io/badge/Kaggle-20BEFF?logo=kaggle&logoColor=fff&style=for-the-badge) ![Jupyter Badge](https://img.shields.io/badge/Jupyter-F37626?logo=jupyter&logoColor=fff&style=for-the-badge) ![pandas Badge](https://img.shields.io/badge/pandas-150458?logo=pandas&logoColor=fff&style=for-the-badge) ![Microsoft Excel Badge](https://img.shields.io/badge/Microsoft%20Excel-217346?logo=microsoftexcel&logoColor=fff&style=for-the-badge)

For inquiries or feedback, please contact: 

[![INSTAGRAM](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/piyush.mujmule) ![Kaggle Badge](https://img.shields.io/badge/Kaggle-yellow?logo=kaggle&logoColor=fff&style=for-the-badge)

## Overview

This project involves analyzing the market share of electric vehicles (EVs) across different regions. The analysis includes data preparation, market share calculation, regional comparison, and visualization to identify trends and insights.

## Dataset

The dataset used in this analysis is `Global EV Data 2024.csv`, which contains information on electric vehicle market data including regions, categories, parameters, and powertrains.

## Steps

### 1. Data Preparation

#### Load the Dataset

```python
import pandas as pd

# Load the dataset
file_path = 'Global EV Data 2024.csv'
ev_data = pd.read_csv(file_path)

# Display the first few rows of the dataset
print(ev_data.head())
```

#### Clean the Data

```python
# Check for missing values
print(ev_data.isnull().sum())

# Drop or fill missing values
ev_data = ev_data.dropna()
```

#### Ensure Data Consistency

```python
# Check for inconsistent data
print(ev_data['region'].unique())
print(ev_data['category'].unique())
print(ev_data['parameter'].unique())
print(ev_data['powertrain'].unique())

# Correct any inconsistent data if needed
# ev_data['region'] = ev_data['region'].str.strip()
```

#### Filter Relevant Data

```python
# Filter data for years 2015 onwards
ev_data = ev_data[ev_data['year'] >= 2015]
```

### 2. Market Share Calculation

#### Calculate Total Market Size

```python
# Group data by region and year to calculate total market size
total_market_size = ev_data.groupby(['region', 'year'])['value'].sum().reset_index()
total_market_size.rename(columns={'value': 'total_market_size'}, inplace=True)

# Merge total market size back into the original dataset
ev_data = pd.merge(ev_data, total_market_size, on=['region', 'year'])
```

#### Calculate Market Share

```python
# Calculate the market share for each entry
ev_data['market_share'] = (ev_data['value'] / ev_data['total_market_size']) * 100
```

### 3. Regional Comparison

#### Compare Market Shares Across Regions

```python
# Pivot data to compare market shares across regions
market_share_pivot = ev_data.pivot_table(index='year', columns='region', values='market_share')
```

#### Analyze Trends and Outliers

```python
# Describe the data to find trends and outliers
market_share_summary = market_share_pivot.describe()
print(market_share_summary)
```

### 4. Visualization

#### Create Bar Charts

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Example: Bar chart for a specific year
sns.barplot(x='region', y='market_share', data=ev_data[ev_data['year'] == 2023])
plt.title('Market Share by Region in 2023')
plt.xticks(rotation=90)
plt.show()
```

#### Create Heatmaps

```python
# Heatmap of market share over time for different regions
sns.heatmap(market_share_pivot, cmap="YlGnBu")
plt.title('Heatmap of Market Share by Region Over Time')
plt.show()
```

#### Create Geospatial Maps (Optional)

```python
import geopandas as gpd

# Load a world map
world = gpd.read_file(gpd.datasets.get_path('naturalearth_lowres'))

# Merge with the EV market share data (ensure the region names match)
# merged_data = world.set_index('name').join(ev_data.set_index('region'))

# Plot the data
# merged_data.plot(column='market_share', cmap='OrRd', legend=True)
# plt.title('Global Market Share of EVs by Region')
# plt.show()
```

### 5. Strategic Insights

#### Summarize Findings

```python
# Summarize key insights from the data
top_regions = ev_data.groupby('region')['market_share'].mean().sort_values(ascending=False)
print("Top regions by average market share:")
print(top_regions.head())
```

### 6. Reporting

#### Create a Report or Dashboard

Compile all findings, visualizations, and insights into a comprehensive report or interactive dashboard using tools like Jupyter Notebook, Tableau, or Power BI.

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/HacktiveMindset/Market-Share-Analysis-by-Region
   ```

2. Install the required Python packages:
   ```bash
   pip install pandas matplotlib seaborn geopandas
   ```

## Usage

1. Place the dataset `Global EV Data 2024.csv` in the appropriate directory.
2. Run the analysis scripts as described in the steps above.

## Contributing

If you'd like to contribute to this project, please fork the repository and submit a pull request with your changes.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

