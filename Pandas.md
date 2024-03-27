# Pandas

To create the Data Frame

```python
data = {
    'name': ['Angela', 'Amin', 'Daniel'],
    'age': [18, 24, 27]
}

df = pd.DataFrame(data)
```

## Querying

Get the general metrics

```python
df.describe()
```

Also for comparing rows of data frame, 

```python
filtered_df = df['Age'] > 20
# Then to get the data frame of it 
df[filtered_df]
```

### Filtering

```python
# Filter rows
df.loc[df['Type 1'] == 'Fire']

# Consider the not annotation
df.loc[~df['Name'].str.contains('Mega')]
df.loc[df['Type 1'].str.contains('fire|grass', flags=re.I, regex=True)]

# Consider the and annotation
new_df = df.loc[(df['Type 1'] == 'Fire') & (df['Type 2'] != 'Fire')]
new_df = new_df.reset_index()

# Remove the duplicates
df.drop_duplicates().reset_index(drop=True)
```

### Sorting

```python
df.sort_values(['Name', 'HP'], ascending=[1, 0])
```


## Iterating

```python
for idx, row in df.iterrows():
	# Do something
	pass
```

### Aggregate statistics

```python
# Calculate the mean based on their type
df.groupby('Type 1').mean()
df.groupby(['Type 1']).mean()
df.groupby('name').aggregate({
	'age': 'mean'
})
new_df = df.groupby('name').aggregate({
	'age': 'max'
})
```

## Indexing

To get the index of a Data Frame

```python
df.index
```

If you want to have the range after filter, you can

```python
new_df = df.groupby('name').aggregate({
	'age': 'max'
})
new_df.reset_index()
```

You can change and set the indicies.

```python
df_class_act = df_class_act.set_index('stdid')
```

## Shape

```python
# To get the size of the data
df.shape

# To get the length
df.shape[0]
```

## Drop

```python
# Remove any rows that have NaN value
df = df.dropna()
```

## Series

```python
import numpy as np
a = pd.Series([1, 2, 3, 4, 'jadi', np.nan)

dates = pd.date_range('20210101', periods=6)
```

## Data Frame

```py
df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=['a', 'b', 'c', 'd'])
```

## Load data from csv

```python
import pandas as pd

# Load csv data
df = pd.read_csv('pokemon_data.csv')
# You can also load excel file
excel_data = pd.read_excel('pokemon_data.xlsx')

# Print all data
print(df)
```

You can print head or tail lines

```python
df.head(30)
df.tail(30)
```

### Dealing with big data

```python
for df in pd.read_csv('pokemon_data.csv', chunksize=5):
	# Do the task
	pass
```

## Reading data

```python
df.columns

# Read specific columns
df[['Name', 'Type 1']]

# Read specific lines
df.iloc[2:30]

# Read a field
df.iloc[4, 2]
```

## Mutate columns

You can rename the titles

```python
cdf.rename(columns = {
	'TA1/Total':'TA1',
	'TA2/Total':'TA2',
	'TA3/Total':'TA3',
	'TA4/Total':'TA4',
}, inplace = True)
```

Mutate the values

```python
# Add a column
df['Total'] = df['HP'] + df['Defense'] + df['Attack']
df['Total'] = df.iloc[:, 4:10].sum(axis=1)

# Delete a column
df = df.drop(columns=['Total'])
```

## Mutate data

```python
df.loc[df['Type 1'] == 'Fire', 'Type 1'] = 'Flamer'
```

## Convert

To convert the data to python list

```python
df.head(10).values.tolist()
```

## Saving

```python
df.to_csv('output.csv')
df.to_excel('output.xlsx')
```

# Resources

- [Tutorial - Github Repo](https://github.com/KeithGalli/pandas)
- Udemy - Data Engineering for Beginners with Python and SQL 2023-9