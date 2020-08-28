## Pandas Data Cleaning

In this checkpoint you will be doing some preprocessing for a dataset for the videogame FIFA19 (https://www.kaggle.com/karangadiya/fifa19).  The dataset contains both data for the game as well as information about the players' real life careers.


```python
# Run this cell without changes to import the necessary libraries
import pandas as pd
import numpy as np
import warnings
warnings.filterwarnings('ignore')
```

### 1. Read the CSV file into a pandas dataframe**

The data you'll be working with is found in a file called `'./data/fifa.csv'`.  

Use your knowledge of pandas to:
- create a new dataframe using the csv data and assign that dataframe to the variable `df`


- Look at the first five rows of `df`, and then assign them to the variable `df_first_five`

- Display the number of rows and columns of `df` as a tuple, and then assign that tuple to the variable `df_rows_columns`

*Hint: use the attributes of a `df` - in other words, `df.something` - in order to do the last two bullet points above!*


```python
### BEGIN SOLUTION


from test_scripts.test_class import Test
test = Test()

df = pd.read_csv('./data/fifa.csv')

test.save(df,'df')
test.save(df.head(), 'df_head')
test.save(df.shape, 'df_shape')
### END SOLUTION
```


```python
### BEGIN HIDDEN TESTS

from test_scripts.test_class import Test
test = Test()

test.run_test(df, 
              'df', 
              "looks like you didn't import the dataframe correctly or didn't assign it to 'df'?"
             )

test.run_test(df_first_five, 
              'df_head',
              "looks like you didn't assign the first five rows correctly to 'df_first_five'? "
             )

test.run_test(df_rows_columns,
              'df_shape',
              "looks like you didn't assign the (rows, columns) tuple correctly to 'df_rows_columns'?"
             )

### END HIDDEN TESTS
```

### 2. Drop rows with missing values from a specific column, `Release Clause`
    
#### A. Drop rows for which the value in the column `Release Clause` is None or not given. 

(This is part of a soccer player's contract dealing with being bought out by another team.)**

#### B. Check the shape of the dataframe after you drop, and assign a tuple with rows and columns to the variable `post_drop`


```python
### BEGIN SOLUTION

df.dropna(subset=['Release Clause'],inplace=True)
test.save(df, 'df_release')
test.save(df.shape, 'post_drop')

### END SOLUTION
```


```python
### BEGIN HIDDEN TESTS

test.run_test(df, 
              'df_release', 
              "looks like the right number of rows weren't dropped?"
             )

test.run_test(post_drop,
              'post_drop',
              "looks like the (rows, columns) tuple wasn't assigned correctly to 'post_drop'?"
             )
### END HIDDEN TESTS
```

### 3. Convert the `Release Clause` Price from Euros to Dollars

Now that there are no missing values, we can change the values in the `Release Clause` column from Euro to Dollar amounts.


#### Change the values in the `Release Clause` contract from Euros to the appropriate number of Dollars

- Assume the current exchange rate is `1 Euro = 1.2 Dollars`
- Make sure that the column **inside of the dataframe** is changed!


```python
### BEGIN SOLUTION

df['Release Clause'] = df['Release Clause'] * 1.2

test.save(df['Release Clause'].mean(), 'new_mean')
test.save(df, 'df_in_dollars')

### END SOLUTION
```


```python
### BEGIN HIDDEN TESTS

test.run_test(df['Release Clause'].mean(), 
              'new_mean', 
              "looks like you didn't multiply the column by 1.2 and put those new values in the dataframe?",
              'float'
             )

### END HIDDEN TESTS
```
