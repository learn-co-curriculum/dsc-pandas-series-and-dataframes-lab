
# Understanding Pandas Series and DataFrames - Lab

## Introduction

In this lab, let's get some hands-on practice working with data cleanup using Pandas.

## Objectives
You will be able to:

- Manipulate columns in DataFrames (`df.rename()`, `df.drop()`) 
- Manipulate the index in DataFrames (`df.reindex()`, `df.drop()`, `df.rename()`) 
- Manipulate column datatypes 

## Let's get started!


```python
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
df = pd.read_csv('turnstile_180901.txt')
print(df.shape)
df.head()
```

## Rename all the columns to lower case


```python
new_cols = [col.lower() for col in df.columns]
#set equal to df.columns if you want the change to take place
new_cols
```

## Change the Index to be the Line Names


```python
df = df.set_index('LINENAME')
df.head()
```

## Remove the index


```python
df = df.reset_index() 
df.head()
```

## Create another column 'Num_Lines' that is a count of how many lines pass through a station. Then sort your DataFrame by this column in descending order
*Hint: According to the [data dictionary](http://web.mta.info/developers/resources/nyct/turnstile/ts_Field_Description.txt), LINENAME represents all train lines that can be boarded at a given station. Normally lines are represented by one character. For example, LINENAME 456NQR represents trains 4, 5, 6, N, Q, and R.*


```python
df['Num_Lines'] = df.LINENAME.map(lambda x: len(x))
```

## Write a function to clean a column name


```python
def clean(col_name):
    cleaned = col_name.strip()#Your code here; whatever you want to do to col_name. Hint: think back to str methods.
    return cleaned
```


```python
# This is a list comprehension. It applies your clean function to every item in the list.
# We then reassign that to df.columns
# You shouldn't have to change anything here.
# Your function above should work appropriately here.
df.columns = [clean(col) for col in df.columns] 
```


```python
# Checking the output, we can see the results.
df.columns
```

## Group the Data by Day of Week and Plot the Sum of The Numeric Columns


```python
df.DATE = pd.to_datetime(df.DATE)
```


```python
df['Dayofweek'] = df.DATE.dt.dayofweek
```


```python
grouped = df.groupby('Dayofweek').sum()
grouped.plot(kind='barh')
```


```python
df.DATE.dt.dayofweek?
```

## Group the Data by Weekend/Weekday and Plot the Sum of the Numeric Columns


```python
grouped = grouped.reset_index()
grouped.head()
```


```python
grouped['IsWeekend'] = grouped.Dayofweek.map({0:False,1:False,2:False,3:False,4:False,5:True,6:True})
wkend = grouped.groupby('IsWeekend').mean()
wkend[['ENTRIES', 'EXITS']].plot(kind='barh')
```


```python
# What Not To Do
grouped['IsWeekend'] = grouped.Dayofweek.map({0:False,1:False,2:False,3:False,4:False,5:True,6:True})
wkend = grouped.groupby('IsWeekend').sum()
wkend[['ENTRIES', 'EXITS']].plot(kind='barh')
```

## Analysis Question: 

What is misleading about the day of week and weekend/weekday charts you just plotted?


```python
# Answer: The raw data for entries/exits is cumulative. 
# As such, you would first need to order the data by time and station, 
# and then calculate the difference in order to produce meaningful aggregations.
```

## Drop a couple of columns


```python
df.head(2)
```


```python
df = df.drop(['C/A', 'SCP'], axis=1)
df.head(2)
```

## Summary

Great! You practiced your data cleanup skills using Pandas.
