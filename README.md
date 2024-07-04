# Complete Pandas Tutorial
A comprehensive tutorial on the Python Pandas library, updated to be consistent with best practices and features available in 2024.

<img src='./images/thumbnail.jpg' width=50%>

The tutorial can be watched [here](https://youtu.be/2uvysYbKdjM?si=8UnGt0bwLwo-eEQL)

The code that is walked through in the tutorial is in [tutorial.ipynb](./tutorial.ipynb)

# Getting Started with Pandas Locally

To get started with Pandas locally, you can follow these steps to set up your environment and clone the recommended repository.

## Setting Up Your Local Environment

### Step 1: Install Python

First, ensure you have Python installed on your system. You can download Python from the [official website](https://www.python.org/).

### Step 2: Fork the Repository

Fork the repository to your own GitHub account by visiting [complete-pandas-tutorial](https://github.com/KeithGalli/complete-pandas-tutorial) and clicking the "Fork" button in the top-right corner.

### Step 3: Clone the Forked Repository

Clone your forked repository to your local machine. Open a terminal or command prompt and run:

```sh
git clone https://github.com/yourusername/complete-pandas-tutorial.git
cd complete-pandas-tutorial
```

Replace `yourusername` with your actual GitHub username.

### Step 4: Create a Virtual Environment (optional)

Creating a virtual environment is a good practice to manage dependencies for your projects. Run the following command:

```sh
python -m venv myenv
```

Activate the virtual environment:

- On Windows:
  ```sh
  myenv\Scripts\activate
  ```
- On macOS/Linux:
  ```sh
  source myenv/bin/activate
  ```

To deactivate the virtual environment, run:

- On Windows:
  ```sh
  myenv\Scripts\deactivate.bat
  ```
- On macOS/Linux:
  ```sh
  deactivate
  ```

### Step 5: Install Required Libraries

With the virtual environment activated, install the necessary libraries from the `requirements.txt` file:

```sh
pip install -r requirements.txt
```

### Step 6: Open Your Code Editor

You can use your favorite code editor like Visual Studio Code or PyCharm. Open the cloned repository folder in your code editor.

### Step 7: Create a Jupyter Notebook

Create a new Jupyter Notebook file in your code editor:

- In Visual Studio Code, click on the "New File" icon or press `Ctrl+N`, then save the file with a `.ipynb` extension.
- In PyCharm, right-click on the project folder, select "New", and then "Jupyter Notebook".
- **Else**, if these options don't work or you are using an editor that doesn't support Jupyter Notebooks, run the following command in your terminal:
  ```sh
  jupyter notebook
  ```
  This will open Jupyter Notebook in your web browser.

## Using Google Colab

If you prefer not to set up things locally, you can use Google Colab, which allows you to run Python code in your browser without any setup.

Go to [Google Colab](https://colab.research.google.com/) and start a new notebook. You can upload your dataset and start coding with Pandas immediately.

# Python Pandas Cheat Sheet

This cheat sheet is a companion to the "Complete Python Pandas Data Science Tutorial."

## Creating DataFrames

Creating DataFrames is the foundation of using Pandas. Here’s how to create a simple DataFrame and display its content.

```python
import pandas as pd
import numpy as np

# Create a simple DataFrame
df = pd.DataFrame(
    [[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]],
    columns=["A", "B", "C"],
    index=["x", "y", "z", "zz"]
)

# Display the first few rows
df.head()

# Display the last two rows
df.tail(2)
```

## Loading Data
Loading data into DataFrames from various file formats is crucial for real-world data analysis.

```python
# Load data from CSV
coffee = pd.read_csv('./warmup-data/coffee.csv')

# Load data from Parquet
results = pd.read_parquet('./data/results.parquet')

# Load data from Excel
olympics_data = pd.read_excel('./data/olympics-data.xlsx', sheet_name="results")
```

## Accessing Data
Accessing different parts of the DataFrame allows for flexible data manipulation and inspection.

```python
# Access columns
df.columns

# Access index
df.index.tolist()

# General info about the DataFrame
df.info()

# Statistical summary
df.describe()

# Number of unique values in each column
df.nunique()

# Access unique values in a column
df['A'].unique()

# Shape and size of DataFrame
df.shape
df.size
```
## Filtering Data
Filtering data is essential for extracting relevant subsets based on conditions.

```python
# Filter rows based on conditions
bios.loc[bios["height_cm"] > 215]

# Multiple conditions
bios[(bios['height_cm'] > 215) & (bios['born_country']=='USA')]

# Filter by string conditions
bios[bios['name'].str.contains("keith", case=False)]

# Regex filters
bios[bios['name'].str.contains(r'^[AEIOUaeiou]', na=False)]
```
## Adding/Removing Columns
Adding and removing columns is important for maintaining and analyzing relevant data.

```python
# Add a new column
coffee['price'] = 4.99

# Conditional column
coffee['new_price'] = np.where(coffee['Coffee Type']=='Espresso', 3.99, 5.99)

# Remove a column
coffee.drop(columns=['price'], inplace=True)

# Rename columns
coffee.rename(columns={'new_price': 'price'}, inplace=True)

# Create new columns from existing ones
coffee['revenue'] = coffee['Units Sold'] * coffee['price']
```
## Merging and Concatenating Data
Merging and concatenating DataFrames is useful for combining different datasets for comprehensive analysis.

```python
# Merge DataFrames
nocs = pd.read_csv('./data/noc_regions.csv')
bios_new = pd.merge(bios, nocs, left_on='born_country', right_on='NOC', how='left')

# Concatenate DataFrames
usa = bios[bios['born_country']=='USA'].copy()
gbr = bios[bios['born_country']=='GBR'].copy()
new_df = pd.concat([usa, gbr])
```
## Handling Null Values
Handling null values is essential to ensure the integrity of data analysis.

```python
# Fill NaNs with a specific value
coffee['Units Sold'].fillna(0, inplace=True)

# Interpolate missing values
coffee['Units Sold'].interpolate(inplace=True)

# Drop rows with NaNs
coffee.dropna(subset=['Units Sold'], inplace=True)
```
## Aggregating Data
Aggregation functions like value counts and group by help in summarizing data efficiently.

```python
# Value counts
bios['born_city'].value_counts()

# Group by and aggregation
coffee.groupby(['Coffee Type'])['Units Sold'].sum()
coffee.groupby(['Coffee Type'])['Units Sold'].mean()

# Pivot table
pivot = coffee.pivot(columns='Coffee Type', index='Day', values='revenue')
```
## Advanced Functionality
Advanced functionalities such as rolling calculations, rankings, and shifts can provide deeper insights.

```python
# Cumulative sum
coffee['cumsum'] = coffee['Units Sold'].cumsum()

# Rolling window
latte = coffee[coffee['Coffee Type']=="Latte"].copy()
latte['3day'] = latte['Units Sold'].rolling(3).sum()

# Rank
bios['height_rank'] = bios['height_cm'].rank(ascending=False)

# Shift
coffee['yesterday_revenue'] = coffee['revenue'].shift(1)
```
## New Functionality
The PyArrow backend offers optimized performance for certain operations, particularly string operations.

```python
# PyArrow backend
results_arrow = pd.read_csv('./data/results.csv', engine='pyarrow', dtype_backend='pyarrow')
results_arrow.info()
```
