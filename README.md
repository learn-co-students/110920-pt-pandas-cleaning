## Pandas Data Cleaning

In this checkpoint you will be doing some preprocessing for a dataset for the videogame FIFA19 (https://www.kaggle.com/karangadiya/fifa19).  The dataset contains both data for the game as well as information about the players' real life careers.


```python
# Run this cell without changes to import the necessary libraries
import pandas as pd
import numpy as np
import warnings
warnings.filterwarnings('ignore')
```

### 1. Read the CSV file into a pandas dataframe

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
df_first_five = df.head()
df_rows_columns = df.shape

test.save(df,'df')
test.save(df_first_five, 'df_head')
test.save(df_rows_columns, 'df_shape')

### END SOLUTION
```


```python
#PLACE ALL WORK FOR THE ABOVE QUESTION ABOVE THIS CELL

#THIS UNALTERABLE CELL CONTAINS HIDDEN TESTS

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

### 2. List some data cleaning steps that might have to be done on raw data

=== BEGIN MARK SCHEME ===

- Drop nulls
- Change column names
- Look for nonsensical data values

=== END MARK SCHEME ===

### 3. Drop rows with missing values from a specific column, `Release Clause`
    
#### A. Drop rows for which the value in the column `Release Clause` is None or not given. 

(This is part of a soccer player's contract dealing with being bought out by another team.)

#### B. Check the shape of the dataframe after you drop, and assign a tuple with the number of rows and columns (in that order) to the variable `post_drop`


```python
### BEGIN SOLUTION

from test_scripts.test_class import Test
test = Test()

df.dropna(subset=['Release Clause'],inplace=True)
post_drop = df.shape

test.save(df, 'df_release')
test.save(post_drop, 'post_drop')

### END SOLUTION
```


```python
#PLACE ALL WORK FOR THE ABOVE QUESTION ABOVE THIS CELL

#THIS UNALTERABLE CELL CONTAINS HIDDEN TESTS

### BEGIN HIDDEN TESTS

from test_scripts.test_class import Test
test = Test()

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

### 4. Convert the `Release Clause` Price from Euros to Dollars

Now that there are no missing values, we can change the values in the `Release Clause` column from Euro to Dollar amounts.


#### Change the values in the `Release Clause` contract from Euros to the appropriate number of Dollars

- Assume the current exchange rate is `1 Euro = 1.2 Dollars`
- Make sure that the column **inside of the dataframe** is changed!


```python
### BEGIN SOLUTION

from test_scripts.test_class import Test
test = Test()

df['Release Clause'] = df['Release Clause'] * 1.2

test.save(df['Release Clause'].mean(), 'new_mean')
test.save(df, 'df_in_dollars')

### END SOLUTION
```


```python
#PLACE ALL WORK FOR THE ABOVE QUESTION ABOVE THIS CELL

#THIS UNALTERABLE CELL CONTAINS HIDDEN TESTS

### BEGIN HIDDEN TESTS

from test_scripts.test_class import Test
test = Test()

test.run_test(df['Release Clause'].mean(), 
              'new_mean', 
              "looks like you didn't multiply the column by 1.2 and put those new values in the dataframe?",
              'float'
             )

### END HIDDEN TESTS
```

### 5. Descriptive Statistics

#### A) What are the mean age and the median age for the players in this dataset?
- Assign the mean age to `mean_age` and the median age to `median_age`


```python
### BEGIN SOLUTION

from test_scripts.test_class import Test
test = Test()

mean_age = df['Age'].mean()
median_age = df['Age'].median()

test.save(mean_age, 'desc_stats_mean_age')
test.save(median_age, 'desc_stats_median_age')\

### END SOLUTION
```


```python
#PLACE ALL WORK FOR THE ABOVE QUESTION ABOVE THIS CELL

#THIS UNALTERABLE CELL CONTAINS HIDDEN TESTS

### BEGIN HIDDEN TESTS

from test_scripts.test_class import Test
test = Test()

test.run_test(mean_age, 
              'desc_stats_mean_age', 
              "looks like you didn't calculate the mean age correctly?",
              'float'
             )

test.run_test(median_age, 
              'desc_stats_median_age', 
              "looks like you didn't calculate the median age correctly?",
              'float'
             )

### END HIDDEN TESTS
```

#### In your own words, how are the mean and median of this data related to each other, and what do these values tell us about the distribution of the variable 'Age'?

=== BEGIN MARK SCHEME ===

"""
Mean age = 25.23 
Median age = 25

The average age of all players in the league is 25.22 years. 
The center of the dataset rests at 25. Since mean and median are pretty similar, 
age seems to be slightly skewed towards the older end of the spectrum.
"""

=== END MARK SCHEME ===

#### B) Who is the oldest player from Argentina and how old is he?

- Store a list with your answer in the format `[name, age]` as the variable `oldest_argentine`
- Make sure `age` is stored as an `int`!
- Use the `Nationality` column


```python
### SOLUTION

from test_scripts.test_class import Test
test = Test()

argentines = df.loc[df['Nationality'] == 'Argentina']

oldest_argentine = argentines.loc[argentines['Age'].idxmax(), ['Name', 'Age']].tolist()

test.save(oldest_argentine[0], 'oldest_argentine_name')
test.save(oldest_argentine[1], 'oldest_argentine_age')
test.save(oldest_argentine, 'oldest_argentine')

### END SOLUTION
```


```python
#PLACE ALL WORK FOR THE ABOVE QUESTION ABOVE THIS CELL

#THIS UNALTERABLE CELL CONTAINS HIDDEN TESTS

### BEGIN HIDDEN TESTS

from test_scripts.test_class import Test
test = Test()

test.run_test(oldest_argentine[0], 
              'oldest_argentine_name', 
              "looks like you didn't calculate the name correctly or save it as the first item in the list?",
             )

test.run_test(oldest_argentine[1], 
              'oldest_argentine_age', 
              "looks like you didn't calculate the age correctly or save it as the second item in the list?",
              'float'
             )

test.run_test(oldest_argentine,
              'oldest_argentine',
              "looks like you didn't put the list together correctly?"
             )

### END HIDDEN TESTS
```
