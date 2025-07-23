# Python Environment

SnapThink includes a powerful Python environment with pre-installed data science libraries, making it easy to analyze data and create visualizations.

## What's Included

### 📦 **Pre-installed Libraries**
- **pandas** - Data manipulation and analysis
- **numpy** - Numerical computing
- **matplotlib** - Data visualization
- **requests** - HTTP requests
- **jupyter** - Notebook functionality
- **pybullet** - Physics simulation

### 🔧 **Environment Management**
- **Micromamba** - Fast package manager
- **Isolated environments** - Each notebook has its own Python environment
- **Easy package installation** - Add libraries as needed

## Getting Started with Python

### 🚀 **Your First Python Code**

Simply ask the AI to run Python code:
```
User: Calculate 2 + 2 using Python
AI: I'll calculate that for you:

```python
result = 2 + 2
print(f"2 + 2 = {result}")
```

Output: 2 + 2 = 4
```

### 📊 **Basic Data Analysis**

Try these examples:
```
User: Create a simple list and show its statistics
AI: I'll create a list and calculate basic statistics:

```python
import numpy as np

# Create sample data
data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Calculate statistics
mean = np.mean(data)
median = np.median(data)
std = np.std(data)

print(f"Mean: {mean}")
print(f"Median: {median}")
print(f"Standard Deviation: {std:.2f}")
```

Output:
Mean: 5.5
Median: 5.5
Standard Deviation: 2.87
```

## Working with Data

### 📈 **Creating Visualizations**

SnapThink automatically displays matplotlib plots:
```python
import matplotlib.pyplot as plt
import numpy as np

# Generate sample data
x = np.linspace(0, 10, 100)
y = np.sin(x)

# Create plot
plt.figure(figsize=(10, 6))
plt.plot(x, y, 'b-', linewidth=2)
plt.title('Sine Wave')
plt.xlabel('X values')
plt.ylabel('Y values')
plt.grid(True)
plt.show()
```

The plot will automatically appear in your chat!

### 📊 **Loading CSV Data**

When you upload a CSV file, SnapThink provides the exact file path:
```python
import pandas as pd

# Load your uploaded CSV (SnapThink provides the path)
df = pd.read_csv("/path/to/your/notebook/docs/data.csv")

# Display first 5 rows
print(df.head())

# Show basic info
print(f"\nDataset shape: {df.shape}")
print(f"Columns: {list(df.columns)}")
```

### 🔍 **Data Exploration**

Ask for specific analyses:
```
User: Analyze my sales data and show trends by month
AI: I'll analyze your sales data:

```python
import pandas as pd
import matplotlib.pyplot as plt

# Load the data
df = pd.read_csv("your_sales_data.csv")

# Convert date column
df['date'] = pd.to_datetime(df['date'])

# Group by month and sum sales
monthly_sales = df.groupby(df['date'].dt.to_period('M'))['sales'].sum()

# Create visualization
plt.figure(figsize=(12, 6))
monthly_sales.plot(kind='line', marker='o')
plt.title('Monthly Sales Trends')
plt.xlabel('Month')
plt.ylabel('Sales ($)')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# Show summary statistics
print(f"Total sales: ${monthly_sales.sum():,.2f}")
print(f"Average monthly sales: ${monthly_sales.mean():,.2f}")
```
```

## Advanced Features

### 🔄 **Persistent Environment**

Python variables and imports persist throughout your conversation:
```python
# First message - load data
import pandas as pd
df = pd.read_csv("data.csv")
print(f"Loaded {len(df)} rows")

# Later in conversation - use same data
# The 'df' variable is still available!
print(df.describe())
```

### 📦 **Installing New Packages**

Need additional libraries? Just ask:
```
User: Install the seaborn library for better visualizations
AI: I'll install seaborn for you:

```python
# Installing seaborn...
# (SnapThink handles the micromamba installation)

import seaborn as sns
print("Seaborn installed and imported successfully!")
```
```

### 🎨 **Custom Functions**

Create and reuse functions:
```python
def analyze_column(df, column_name):
    """Analyze a specific column in the dataframe"""
    print(f"Analysis for column: {column_name}")
    print(f"Data type: {df[column_name].dtype}")
    print(f"Unique values: {df[column_name].nunique()}")
    print(f"Missing values: {df[column_name].isnull().sum()}")
    
    if df[column_name].dtype in ['int64', 'float64']:
        print(f"Mean: {df[column_name].mean():.2f}")
        print(f"Median: {df[column_name].median():.2f}")

# Use the function
analyze_column(df, 'age')
```

## Data Science Workflows

### 📋 **Exploratory Data Analysis (EDA)**

Follow this typical workflow:
```
1. "Load my dataset and show basic information"
2. "Display the first few rows and column types"
3. "Check for missing values and outliers"
4. "Create histograms for numerical columns"
5. "Show correlation matrix for key variables"
6. "Identify interesting patterns in the data"
```

### 📊 **Statistical Analysis**

Perform various statistical tests:
```python
from scipy import stats

# T-test example
group1 = df[df['category'] == 'A']['value']
group2 = df[df['category'] == 'B']['value']

t_stat, p_value = stats.ttest_ind(group1, group2)
print(f"T-statistic: {t_stat:.3f}")
print(f"P-value: {p_value:.3f}")
```

### 🎯 **Machine Learning**

Simple ML workflows:
```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score

# Prepare data
X = df[['feature1', 'feature2']]
y = df['target']

# Split data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Train model
model = LinearRegression()
model.fit(X_train, y_train)

# Evaluate
predictions = model.predict(X_test)
r2 = r2_score(y_test, predictions)
print(f"R² Score: {r2:.3f}")
```

## Best Practices

### 💡 **Code Organization**

#### Break Complex Tasks into Steps
❌ **Too complex**: "Load data, clean it, analyze everything, and create all possible visualizations"
✅ **Step by step**:
1. "Load my dataset and show basic info"
2. "Clean the data by handling missing values"
3. "Create a correlation matrix"
4. "Generate specific visualizations for key insights"

#### Use Descriptive Variable Names
```python
# Good
customer_ages = df['age']
monthly_revenue = df.groupby('month')['revenue'].sum()

# Avoid
x = df['age']
data = df.groupby('month')['revenue'].sum()
```

### 📈 **Visualization Tips**

#### Request Specific Plot Types
- `"Create a scatter plot of X vs Y"`
- `"Show a histogram of customer ages"`
- `"Make a box plot to compare groups"`
- `"Generate a heatmap of correlations"`

#### Customize Your Plots
```python
plt.figure(figsize=(12, 8))  # Larger size
plt.style.use('seaborn')     # Better style
plt.title('My Analysis', fontsize=16)
plt.tight_layout()           # Better spacing
```

### 🔍 **Debugging and Troubleshooting**

#### Check Your Data
```python
# Always start with these
print(df.info())           # Data types and missing values
print(df.describe())       # Statistical summary
print(df.head())          # First few rows
```

#### Handle Common Issues
```python
# Missing values
df = df.dropna()  # Remove missing values
# or
df = df.fillna(df.mean())  # Fill with mean

# Data types
df['date'] = pd.to_datetime(df['date'])  # Convert to datetime
df['category'] = df['category'].astype('category')  # Convert to category
```

## Environment Management

### 🔧 **Package Installation**

SnapThink uses micromamba for package management:
```
User: I need scikit-learn for machine learning
AI: I'll install scikit-learn for you:

# Installing via micromamba...
# Package installed successfully!

import sklearn
print(f"Scikit-learn version: {sklearn.__version__}")
```

### 📦 **Available Packages**

Common packages you can request:
- **Data Science**: scikit-learn, scipy, statsmodels
- **Visualization**: seaborn, plotly, bokeh
- **Web Scraping**: beautifulsoup4, scrapy
- **Time Series**: prophet, statsmodels
- **Deep Learning**: tensorflow, pytorch
- **Geographic**: geopandas, folium

### 🔄 **Environment Reset**

If needed, you can reset the Python environment:
```
User: Reset my Python environment
AI: I'll create a fresh Python environment for this notebook.
# [Creates new isolated environment with base packages]
```

## Example Workflows

### 📊 **Sales Data Analysis**
```
1. Upload: sales_data.csv
2. "Load my sales data and show the structure"
3. "Calculate total sales by product category"
4. "Create a monthly sales trend chart"
5. "Identify the top 10 customers by revenue"
6. "Find seasonal patterns in the data"
```

### 🧪 **Scientific Data Analysis**
```
1. Upload: experiment_results.csv
2. "Load the experimental data and check for outliers"
3. "Calculate descriptive statistics for each condition"
4. "Perform ANOVA to compare groups"
5. "Create box plots showing group differences"
6. "Generate a summary report of findings"
```

### 📈 **Financial Analysis**
```
1. Upload: stock_prices.csv
2. "Calculate daily returns and volatility"
3. "Create candlestick charts for price movements"
4. "Compute moving averages and technical indicators"
5. "Analyze correlation with market indices"
6. "Build a simple trading strategy backtest"
```

## Troubleshooting

### Common Python Issues

#### "Module not found"
```
User: Install the missing package
AI: I'll install [package_name] for you using micromamba...
```

#### "Code execution failed"
- Check for syntax errors in the code
- Ensure data files are properly loaded
- Verify variable names and data types

#### "Plot not displaying"
- Make sure `plt.show()` is called
- Check if matplotlib backend is properly configured
- Try `%matplotlib inline` for Jupyter-style display

#### "Memory issues"
- Work with smaller data samples first
- Use `df.head()` to preview large datasets
- Process data in chunks for very large files

## Next Steps

- **[Document Analysis](document-analysis.md)** - Combine Python with document processing
- **[CSV Analysis Guide](../guides/csv-analysis.md)** - Detailed data analysis examples
- **[Python Data Science Guide](../guides/python-datascience.md)** - Advanced techniques

**Ready to start coding?** Ask the AI to write some Python code for your specific use case!
