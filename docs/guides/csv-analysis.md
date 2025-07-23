# CSV Data Analysis Guide

Learn how to analyze CSV datasets effectively with SnapThink's integrated AI and Python capabilities.

## Quick Start with CSV Files

### 1. **Upload Your CSV**
1. Click **"📄 Upload Document"**
2. Select your `.csv` file
3. Wait for automatic processing
4. SnapThink will analyze the structure and provide file paths

### 2. **Automatic Analysis**
When you upload a CSV, SnapThink automatically:
- Detects columns and data types
- Counts rows and shows preview
- Provides Python file path for coding
- Suggests initial analysis questions

### 3. **Start Analyzing**
```
User: Upload sales_data.csv
AI: ✅ Processed CSV file: sales_data.csv
- 1,500 rows × 6 columns
- Columns: Date, Product, Category, Quantity, Price, Region
- File path: /path/to/notebook/docs/sales_data.csv

Ready for analysis! Try asking:
• "Show me the first few rows"
• "What are the summary statistics?"
• "Create a sales trend visualization"
```

## Common Analysis Patterns

### 📊 **Basic Data Exploration**

#### View Your Data
```
User: Show me the first 10 rows of my data
AI: I'll display the first 10 rows:

```python
import pandas as pd
df = pd.read_csv("/path/to/your/data.csv")
print(df.head(10))
```

| Date       | Product  | Category | Quantity | Price | Region |
|------------|----------|----------|----------|-------|--------|
| 2024-01-01 | Widget A | Electronics | 5 | 29.99 | North |
| 2024-01-01 | Widget B | Home | 3 | 45.00 | South |
...
```

#### Get Summary Statistics
```
User: Show me summary statistics for all numerical columns
AI: Here are the summary statistics:

```python
print(df.describe())
print(f"\nDataset shape: {df.shape}")
print(f"Data types:\n{df.dtypes}")
```

Output shows mean, median, standard deviation, etc.
```

### 📈 **Time Series Analysis**

#### Sales Trends Over Time
```
User: Create a monthly sales trend chart
AI: I'll create a monthly sales trend visualization:

```python
import matplotlib.pyplot as plt
import pandas as pd

# Convert date column and calculate monthly sales
df['Date'] = pd.to_datetime(df['Date'])
monthly_sales = df.groupby(df['Date'].dt.to_period('M'))['Quantity'].sum()

# Create trend chart
plt.figure(figsize=(12, 6))
monthly_sales.plot(kind='line', marker='o', linewidth=2)
plt.title('Monthly Sales Trends', fontsize=16)
plt.xlabel('Month')
plt.ylabel('Total Quantity Sold')
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

[Chart displays automatically]
```

#### Seasonal Patterns
```
User: Analyze seasonal patterns in my sales data
AI: I'll analyze seasonal patterns:

```python
# Extract month and year
df['Month'] = df['Date'].dt.month
df['Year'] = df['Date'].dt.year

# Calculate average sales by month
seasonal_pattern = df.groupby('Month')['Quantity'].mean()

# Create seasonal chart
plt.figure(figsize=(10, 6))
seasonal_pattern.plot(kind='bar', color='skyblue')
plt.title('Average Sales by Month (Seasonal Pattern)')
plt.xlabel('Month')
plt.ylabel('Average Quantity')
plt.xticks(range(12), ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
                       'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
plt.show()

print("Peak months:", seasonal_pattern.idxmax())
print("Low months:", seasonal_pattern.idxmin())
```
```

### 🏷️ **Category Analysis**

#### Sales by Category
```
User: Compare sales performance across different product categories
AI: I'll analyze sales by category:

```python
# Group by category and calculate totals
category_sales = df.groupby('Category').agg({
    'Quantity': 'sum',
    'Price': 'mean',
    'Product': 'count'
}).round(2)

# Rename columns for clarity
category_sales.columns = ['Total_Quantity', 'Avg_Price', 'Product_Count']
category_sales['Revenue'] = (category_sales['Total_Quantity'] * 
                            category_sales['Avg_Price']).round(2)

print(category_sales)

# Create visualization
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(15, 6))

# Quantity by category
category_sales['Total_Quantity'].plot(kind='bar', ax=ax1, color='lightcoral')
ax1.set_title('Total Quantity Sold by Category')
ax1.set_ylabel('Quantity')

# Revenue by category
category_sales['Revenue'].plot(kind='bar', ax=ax2, color='lightgreen')
ax2.set_title('Revenue by Category')
ax2.set_ylabel('Revenue ($)')

plt.tight_layout()
plt.show()
```
```

### 🗺️ **Geographic Analysis**

#### Regional Performance
```
User: Show me sales performance by region
AI: I'll analyze regional sales performance:

```python
# Regional analysis
regional_analysis = df.groupby('Region').agg({
    'Quantity': ['sum', 'mean'],
    'Price': 'mean'
}).round(2)

# Flatten column names
regional_analysis.columns = ['Total_Sales', 'Avg_Sales_Per_Order', 'Avg_Price']

print("Regional Performance Summary:")
print(regional_analysis)

# Create regional comparison chart
plt.figure(figsize=(12, 8))

# Subplot for total sales
plt.subplot(2, 2, 1)
regional_analysis['Total_Sales'].plot(kind='bar', color='steelblue')
plt.title('Total Sales by Region')
plt.ylabel('Total Quantity')

# Subplot for average order size
plt.subplot(2, 2, 2)
regional_analysis['Avg_Sales_Per_Order'].plot(kind='bar', color='orange')
plt.title('Average Order Size by Region')
plt.ylabel('Average Quantity')

plt.tight_layout()
plt.show()
```
```

## Advanced Analysis Techniques

### 🔍 **Data Quality Assessment**

#### Missing Values and Outliers
```
User: Check my data quality and identify any issues
AI: I'll assess your data quality:

```python
import numpy as np

print("=== DATA QUALITY REPORT ===\n")

# Missing values
print("1. Missing Values:")
missing_vals = df.isnull().sum()
print(missing_vals[missing_vals > 0])

# Duplicate rows
print(f"\n2. Duplicate Rows: {df.duplicated().sum()}")

# Outliers in numerical columns
print("\n3. Outlier Analysis:")
numerical_cols = df.select_dtypes(include=[np.number]).columns

for col in numerical_cols:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1
    outliers = df[(df[col] < Q1 - 1.5*IQR) | (df[col] > Q3 + 1.5*IQR)]
    print(f"{col}: {len(outliers)} outliers ({len(outliers)/len(df)*100:.1f}%)")

# Data type issues
print(f"\n4. Data Types:")
print(df.dtypes)
```
```

### 📊 **Statistical Analysis**

#### Correlation Analysis
```
User: Find correlations between numerical variables
AI: I'll analyze correlations in your data:

```python
import seaborn as sns

# Calculate correlation matrix
numerical_df = df.select_dtypes(include=[np.number])
correlation_matrix = numerical_df.corr()

# Create heatmap
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, 
            annot=True, 
            cmap='coolwarm', 
            center=0,
            square=True,
            fmt='.2f')
plt.title('Correlation Matrix of Numerical Variables')
plt.tight_layout()
plt.show()

# Identify strong correlations
print("Strong correlations (>0.7 or <-0.7):")
for i in range(len(correlation_matrix.columns)):
    for j in range(i+1, len(correlation_matrix.columns)):
        corr_val = correlation_matrix.iloc[i, j]
        if abs(corr_val) > 0.7:
            print(f"{correlation_matrix.columns[i]} - {correlation_matrix.columns[j]}: {corr_val:.3f}")
```
```

#### Distribution Analysis
```
User: Show me the distribution of key variables
AI: I'll analyze the distributions:

```python
# Create distribution plots
numerical_cols = df.select_dtypes(include=[np.number]).columns
n_cols = len(numerical_cols)

fig, axes = plt.subplots(2, 2, figsize=(15, 10))
axes = axes.ravel()

for i, col in enumerate(numerical_cols[:4]):  # Plot first 4 numerical columns
    axes[i].hist(df[col], bins=30, alpha=0.7, color='skyblue', edgecolor='black')
    axes[i].set_title(f'Distribution of {col}')
    axes[i].set_xlabel(col)
    axes[i].set_ylabel('Frequency')
    
    # Add mean line
    mean_val = df[col].mean()
    axes[i].axvline(mean_val, color='red', linestyle='--', label=f'Mean: {mean_val:.2f}')
    axes[i].legend()

plt.tight_layout()
plt.show()
```
```

### 💡 **Business Intelligence**

#### Customer Segmentation
```
User: Segment customers based on their purchasing behavior
AI: I'll perform customer segmentation analysis:

```python
# Assuming we have customer data
customer_metrics = df.groupby('Customer_ID').agg({
    'Quantity': 'sum',           # Total purchases
    'Price': 'mean',             # Average order value
    'Date': ['count', 'max']     # Frequency and recency
}).round(2)

# Flatten columns
customer_metrics.columns = ['Total_Purchases', 'Avg_Order_Value', 'Order_Frequency', 'Last_Purchase']

# Create customer segments
customer_metrics['Segment'] = 'Regular'
customer_metrics.loc[customer_metrics['Total_Purchases'] > customer_metrics['Total_Purchases'].quantile(0.8), 'Segment'] = 'High Value'
customer_metrics.loc[customer_metrics['Order_Frequency'] < customer_metrics['Order_Frequency'].quantile(0.2), 'Segment'] = 'Low Frequency'

# Segment analysis
segment_summary = customer_metrics.groupby('Segment').agg({
    'Total_Purchases': 'mean',
    'Avg_Order_Value': 'mean',
    'Order_Frequency': 'mean'
}).round(2)

print("Customer Segment Analysis:")
print(segment_summary)

# Visualization
plt.figure(figsize=(12, 8))
scatter = plt.scatter(customer_metrics['Total_Purchases'], 
                     customer_metrics['Avg_Order_Value'],
                     c=customer_metrics['Segment'].astype('category').cat.codes,
                     alpha=0.6, s=50)
plt.xlabel('Total Purchases')
plt.ylabel('Average Order Value')
plt.title('Customer Segmentation')
plt.colorbar(scatter)
plt.show()
```
```

## Practical Examples

### 📈 **Sales Performance Dashboard**

Create a comprehensive sales analysis:
```
User: Create a complete sales performance dashboard
AI: I'll create a comprehensive sales dashboard:

```python
# Create a 2x2 dashboard
fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=(16, 12))

# 1. Monthly sales trend
monthly_sales = df.groupby(df['Date'].dt.to_period('M'))['Quantity'].sum()
monthly_sales.plot(ax=ax1, kind='line', marker='o', color='blue')
ax1.set_title('Monthly Sales Trend')
ax1.set_ylabel('Units Sold')

# 2. Category performance
category_sales = df.groupby('Category')['Quantity'].sum().sort_values(ascending=False)
category_sales.plot(ax=ax2, kind='bar', color='green')
ax2.set_title('Sales by Category')
ax2.set_ylabel('Units Sold')

# 3. Regional distribution
regional_sales = df.groupby('Region')['Quantity'].sum()
ax3.pie(regional_sales.values, labels=regional_sales.index, autopct='%1.1f%%')
ax3.set_title('Sales Distribution by Region')

# 4. Top products
top_products = df.groupby('Product')['Quantity'].sum().sort_values(ascending=False).head(10)
top_products.plot(ax=ax4, kind='barh', color='orange')
ax4.set_title('Top 10 Products by Sales')
ax4.set_xlabel('Units Sold')

plt.tight_layout()
plt.show()

# Summary statistics
print("=== SALES DASHBOARD SUMMARY ===")
print(f"Total Units Sold: {df['Quantity'].sum():,}")
print(f"Average Order Size: {df['Quantity'].mean():.1f}")
print(f"Total Revenue: ${(df['Quantity'] * df['Price']).sum():,.2f}")
print(f"Number of Products: {df['Product'].nunique()}")
print(f"Active Regions: {df['Region'].nunique()}")
```
```

### 🎯 **Market Analysis**

Analyze market trends and opportunities:
```
User: Analyze market opportunities and trends in my data
AI: I'll perform a market opportunity analysis:

```python
# Market growth analysis
df['Revenue'] = df['Quantity'] * df['Price']
df['Month'] = df['Date'].dt.to_period('M')

# Growth rate calculation
monthly_revenue = df.groupby('Month')['Revenue'].sum()
growth_rates = monthly_revenue.pct_change() * 100

print("=== MARKET ANALYSIS ===")
print(f"Average monthly growth rate: {growth_rates.mean():.1f}%")
print(f"Best performing month: {monthly_revenue.idxmax()} (${monthly_revenue.max():,.2f})")

# Category growth analysis
category_monthly = df.groupby(['Month', 'Category'])['Revenue'].sum().unstack(fill_value=0)
category_growth = category_monthly.pct_change().mean() * 100

print(f"\nCategory Growth Rates:")
for category, growth in category_growth.sort_values(ascending=False).items():
    print(f"{category}: {growth:.1f}%")

# Visualization
plt.figure(figsize=(15, 10))

# Revenue trend
plt.subplot(2, 2, 1)
monthly_revenue.plot(kind='line', marker='o', color='blue')
plt.title('Monthly Revenue Trend')
plt.ylabel('Revenue ($)')

# Growth rate
plt.subplot(2, 2, 2)
growth_rates.plot(kind='bar', color='green')
plt.title('Month-over-Month Growth Rate')
plt.ylabel('Growth Rate (%)')

# Category trends
plt.subplot(2, 1, 2)
for category in category_monthly.columns:
    category_monthly[category].plot(label=category, marker='o')
plt.title('Revenue Trends by Category')
plt.ylabel('Revenue ($)')
plt.legend()

plt.tight_layout()
plt.show()
```
```

## Best Practices

### 📋 **Data Preparation**

#### Clean Your Data First
```python
# Standard cleaning steps
df = df.dropna()  # Remove missing values
df = df.drop_duplicates()  # Remove duplicates
df['Date'] = pd.to_datetime(df['Date'])  # Convert dates
df['Category'] = df['Category'].str.title()  # Standardize text
```

#### Validate Your Data
```python
# Check for logical errors
assert df['Quantity'].min() >= 0, "Negative quantities found"
assert df['Price'].min() > 0, "Zero or negative prices found"
print("Data validation passed!")
```

### 🎯 **Analysis Tips**

#### Start with Questions
- What are the overall trends?
- Which categories/regions perform best?
- Are there seasonal patterns?
- What drives high performance?

#### Use Multiple Visualizations
- **Line charts** for trends over time
- **Bar charts** for category comparisons
- **Scatter plots** for relationships
- **Heatmaps** for correlations
- **Pie charts** for proportions

#### Tell a Story with Data
1. **Descriptive**: What happened?
2. **Diagnostic**: Why did it happen?
3. **Predictive**: What will happen?
4. **Prescriptive**: What should we do?

## Common CSV Analysis Scenarios

### 📊 **E-commerce Analytics**
- Sales trends and seasonality
- Product performance analysis
- Customer segmentation
- Geographic sales patterns
- Inventory optimization

### 📈 **Financial Analysis**
- Revenue and profit trends
- Expense categorization
- Budget vs. actual analysis
- Cash flow patterns
- ROI calculations

### 🏥 **Survey Data Analysis**
- Response distribution analysis
- Cross-tabulation of responses
- Satisfaction scores
- Demographic breakdowns
- Statistical significance testing

### 🎓 **Academic Research**
- Experimental data analysis
- Statistical hypothesis testing
- Correlation and regression analysis
- Data visualization for papers
- Results summarization

## Troubleshooting

### Common Issues

#### "File not found" Error
- Ensure the CSV file was successfully uploaded
- Check the file path provided by SnapThink
- Try re-uploading the file

#### Encoding Problems
```python
# Try different encodings
df = pd.read_csv("file.csv", encoding='utf-8')
# or
df = pd.read_csv("file.csv", encoding='latin-1')
```

#### Date Parsing Issues
```python
# Specify date format
df['Date'] = pd.to_datetime(df['Date'], format='%Y-%m-%d')
# or let pandas infer
df['Date'] = pd.to_datetime(df['Date'], infer_datetime_format=True)
```

#### Memory Issues with Large Files
```python
# Read in chunks
chunk_list = []
for chunk in pd.read_csv("large_file.csv", chunksize=10000):
    # Process each chunk
    chunk_list.append(chunk)
df = pd.concat(chunk_list, ignore_index=True)
```

## Next Steps

- **[Python Environment](../features/python-environment.md)** - Learn more about Python capabilities
- **[Document Analysis](../features/document-analysis.md)** - Combine CSV with other documents
- **[Python Data Science Guide](python-datascience.md)** - Advanced statistical techniques

**Ready to analyze your CSV data?** Upload a file and start asking questions about your data!
