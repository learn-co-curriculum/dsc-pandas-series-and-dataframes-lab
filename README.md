
# Understanding Pandas Series and DataFrames - Lab

## Introduction

In this lab, let's get some hands-on practice working with data cleanup using Pandas.

## Objectives
You will be able to:

* Manipulate columns in DataFrames (`df.rename()`, `df.drop()`) 
* Manipulate the index in DataFrames (`df.reindex()`, `df.drop()`, `df.rename()`) 
* Manipulate column datatypes 

## Let's get started!


```python
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
# __SOLUTION__ 
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
df = pd.read_csv('turnstile_180901.txt')
print(df.shape)
df.head()
```

    (197625, 11)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>C/A</th>
      <th>UNIT</th>
      <th>SCP</th>
      <th>STATION</th>
      <th>LINENAME</th>
      <th>DIVISION</th>
      <th>DATE</th>
      <th>TIME</th>
      <th>DESC</th>
      <th>ENTRIES</th>
      <th>EXITS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A002</td>
      <td>R051</td>
      <td>02-00-00</td>
      <td>59 ST</td>
      <td>NQR456W</td>
      <td>BMT</td>
      <td>08/25/2018</td>
      <td>00:00:00</td>
      <td>REGULAR</td>
      <td>6736067</td>
      <td>2283184</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A002</td>
      <td>R051</td>
      <td>02-00-00</td>
      <td>59 ST</td>
      <td>NQR456W</td>
      <td>BMT</td>
      <td>08/25/2018</td>
      <td>04:00:00</td>
      <td>REGULAR</td>
      <td>6736087</td>
      <td>2283188</td>
    </tr>
    <tr>
      <th>2</th>
      <td>A002</td>
      <td>R051</td>
      <td>02-00-00</td>
      <td>59 ST</td>
      <td>NQR456W</td>
      <td>BMT</td>
      <td>08/25/2018</td>
      <td>08:00:00</td>
      <td>REGULAR</td>
      <td>6736105</td>
      <td>2283229</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A002</td>
      <td>R051</td>
      <td>02-00-00</td>
      <td>59 ST</td>
      <td>NQR456W</td>
      <td>BMT</td>
      <td>08/25/2018</td>
      <td>12:00:00</td>
      <td>REGULAR</td>
      <td>6736180</td>
      <td>2283314</td>
    </tr>
    <tr>
      <th>4</th>
      <td>A002</td>
      <td>R051</td>
      <td>02-00-00</td>
      <td>59 ST</td>
      <td>NQR456W</td>
      <td>BMT</td>
      <td>08/25/2018</td>
      <td>16:00:00</td>
      <td>REGULAR</td>
      <td>6736349</td>
      <td>2283384</td>
    </tr>
  </tbody>
</table>
</div>




```python
# __SOLUTION__ 
df = pd.read_csv('turnstile_180901.txt')
print(df.shape)
df.head()
```

## Rename all the columns to lower case


```python
#Your code here
```


```python
# __SOLUTION__ 
new_cols = [col.lower() for col in df.columns]
#set equal to df.columns if you want the change to take place
new_cols
```

## Change the Index to be the Line Names


```python
#Your code here
```


```python
# __SOLUTION__ 
df = df.set_index('LINENAME')
df.head()
```

## Remove the index


```python
# Your code here
```


```python
# __SOLUTION__ 
df = df.reset_index() 
df.head()
```

## Create another column 'Num_Lines' that is a count of how many lines pass through a station. Then sort your DataFrame by this column in descending order
*Hint: According to the [data dictionary](http://web.mta.info/developers/resources/nyct/turnstile/ts_Field_Description.txt), LINENAME represents all train lines that can be boarded at a given station. Normally lines are represented by one character. For example, LINENAME 456NQR represents trains 4, 5, 6, N, Q, and R.*


```python
# Your code here
```


```python
# __SOLUTION__ 
df['Num_Lines'] = df.LINENAME.map(lambda x: len(x))
```

## Write a function to clean a column name


```python
def clean(col_name):
    cleaned = #Your code here; whatever you want to do to col_name. Hint: think back to str methods.
    return cleaned
```


```python
# __SOLUTION__ 
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
#  __SOLUTION__ 
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


```python
# __SOLUTION__ 
# Checking the output, we can see the results.
df.columns
```

## Group the Data by Day of Week and Plot the Sum of The Numeric Columns


```python
# Your code here
```


```python
# __SOLUTION__ 
df.DATE = pd.to_datetime(df.DATE)
```


```python
# __SOLUTION__ 
df['Dayofweek'] = df.DATE.dt.dayofweek
```


```python
# __SOLUTION__ 
grouped = df.groupby('Dayofweek').sum()
grouped.plot(kind='barh')
```


```python
# __SOLUTION__ 
df.DATE.dt.dayofweek?
```

## Group the Data by Weekend/Weekday and Plot the Sum of the Numeric Columns


```python
#Your code here
```


```python
# __SOLUTION__ 
grouped = grouped.reset_index()
grouped.head()
```


```python
# __SOLUTION__ 
grouped['IsWeekend'] = grouped.Dayofweek.map({0:False,1:False,2:False,3:False,4:False,5:True,6:True})
wkend = grouped.groupby('IsWeekend').mean()
wkend[['ENTRIES', 'EXITS']].plot(kind='barh')
```


```python
# __SOLUTION__ 
# What Not To Do
grouped['IsWeekend'] = grouped.Dayofweek.map({0:False,1:False,2:False,3:False,4:False,5:True,6:True})
wkend = grouped.groupby('IsWeekend').sum()
wkend[['ENTRIES', 'EXITS']].plot(kind='barh')
```

## Analysis Question: 

What is misleading about the day of week and weekend/weekday charts you just plotted?


```python
# Your answer here 
```


```python
# __SOLUTION__ 
# Answer: The raw data for entries/exits is cumulative. 
# As such, you would first need to order the data by time and station, 
# and then calculate the difference in order to produce meaningful aggregations.
```

## Drop a couple of columns


```python
# Your code here
```


```python
# __SOLUTION__ 
df.head(2)
```


```python
# __SOLUTION__ 
df = df.drop(['C/A', 'SCP'], axis=1)
df.head(2)
```

## Summary

Great! You practiced your data cleanup skills using Pandas.
