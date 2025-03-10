---
jupyter:
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
  language_info:
    codemirror_mode:
      name: ipython
      version: 3
    file_extension: .py
    mimetype: text/x-python
    name: python
    nbconvert_exporter: python
    pygments_lexer: ipython3
    version: 3.10.11
  nbformat: 4
  nbformat_minor: 2
---

::: {.cell .markdown}
Learn how to summarize the columns available in an R data frame. You
will also learn how to chain operations together with the pipe operator,
and how to compute grouped summaries using.
:::

::: {.cell .markdown}
## Welcome!

Hey there! Ready for the first lesson?

The dfply package makes it possible to do R\'s dplyr-style data
manipulation with pipes in python on pandas DataFrames.

[dfply website here](https://github.com/kieferk/dfply)

[![](https://www.rforecology.com/pipes_image0.png "https://github.com/kieferk/dfply"){width=\"600\"}](https://github.com/kieferk/dfply)
:::

::: {.cell .code}
``` python

import pandas as pd
import seaborn as sns
from dfply import *

# Load the dataset
cars = sns.load_dataset('mpg')

# Display the first 3 rows
cars >> head(3)
```

::: {.output .execute_result execution_count="4"}
```{=html}
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
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevrolet chevelle malibu</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>usa</td>
      <td>buick skylark 320</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>usa</td>
      <td>plymouth satellite</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
## The \>\> and \>\>= {#the--and-}

dfply works directly on pandas DataFrames, chaining operations on the
data with the \>\> operator, or alternatively starting with \>\>= for
inplace operations.

*The X DataFrame symbol*

The DataFrame as it is passed through the piping operations is
represented by the symbol X. It records the actions you want to take
(represented by the Intention class), but does not evaluate them until
the appropriate time. Operations on the DataFrame are deferred.
Selecting two of the columns, for example, can be done using the
symbolic X DataFrame during the piping operations.
:::

::: {.cell .markdown}
### Exercise 1. {#exercise-1}

Select the columns \'mpg\' and \'horsepower\' from the cars DataFrame.
:::

::: {.cell .code execution_count="3"}
``` python
# your solution goes here
cars >> select(cars.horsepower, cars.mpg) >> head(3)
```

::: {.output .execute_result execution_count="3"}
```{=html}
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
      <th>horsepower</th>
      <th>mpg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>130.0</td>
      <td>18.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>165.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>150.0</td>
      <td>18.0</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
## Selecting and dropping

There are two functions for selection, inverse of each other: select and
drop. The select and drop functions accept string labels, integer
positions, and/or symbolically represented column names (X.column). They
also accept symbolic \"selection filter\" functions, which will be
covered shortly.

### Exercise 2. {#exercise-2}

Select the columns \'mpg\' and \'horsepower\' from the cars DataFrame
using the drop function.
:::

::: {.cell .code execution_count="2"}
``` python
# your solution goes here
cars >> drop(~X.mpg, ~X.horsepower)
```

::: {.output .execute_result execution_count="2"}
```{=html}
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
      <th>mpg</th>
      <th>horsepower</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>130.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>165.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>150.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16.0</td>
      <td>150.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17.0</td>
      <td>140.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>393</th>
      <td>27.0</td>
      <td>86.0</td>
    </tr>
    <tr>
      <th>394</th>
      <td>44.0</td>
      <td>52.0</td>
    </tr>
    <tr>
      <th>395</th>
      <td>32.0</td>
      <td>84.0</td>
    </tr>
    <tr>
      <th>396</th>
      <td>28.0</td>
      <td>79.0</td>
    </tr>
    <tr>
      <th>397</th>
      <td>31.0</td>
      <td>82.0</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 2 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
## Selection using \~ {#selection-using-}

One particularly nice thing about dplyr\'s selection functions is that
you can drop columns inside of a select statement by putting a
subtraction sign in front, like so: \... %\>% select(-col). The same can
be done in dfply, but instead of the subtraction operator you use the
tilde \~.

### Exercise 3. {#exercise-3}

Select all columns except \'model_year\', and \'name\' from the cars
DataFrame.
:::

::: {.cell .code execution_count="6"}
``` python
# your solution goes here 
cars >> select(~X.model_year, ~X.name)
```

::: {.output .execute_result execution_count="6"}
```{=html}
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
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>origin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16.0</td>
      <td>8</td>
      <td>304.0</td>
      <td>150.0</td>
      <td>3433</td>
      <td>12.0</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17.0</td>
      <td>8</td>
      <td>302.0</td>
      <td>140.0</td>
      <td>3449</td>
      <td>10.5</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>393</th>
      <td>27.0</td>
      <td>4</td>
      <td>140.0</td>
      <td>86.0</td>
      <td>2790</td>
      <td>15.6</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>394</th>
      <td>44.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>52.0</td>
      <td>2130</td>
      <td>24.6</td>
      <td>europe</td>
    </tr>
    <tr>
      <th>395</th>
      <td>32.0</td>
      <td>4</td>
      <td>135.0</td>
      <td>84.0</td>
      <td>2295</td>
      <td>11.6</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>396</th>
      <td>28.0</td>
      <td>4</td>
      <td>120.0</td>
      <td>79.0</td>
      <td>2625</td>
      <td>18.6</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>397</th>
      <td>31.0</td>
      <td>4</td>
      <td>119.0</td>
      <td>82.0</td>
      <td>2720</td>
      <td>19.4</td>
      <td>usa</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 7 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
## Filtering columns

The vanilla select and drop functions are useful, but there are a
variety of selection functions inspired by dplyr available to make
selecting and dropping columns a breeze. These functions are intended to
be put inside of the select and drop functions, and can be paired with
the \~ inverter.

First, a quick rundown of the available functions:

-   starts_with(prefix): find columns that start with a string prefix.
-   ends_with(suffix): find columns that end with a string suffix.
-   contains(substr): find columns that contain a substring in their
    name.
-   everything(): all columns.
-   columns_between(start_col, end_col, inclusive=True): find columns
    between a specified start and end column. The inclusive boolean
    keyword argument indicates whether the end column should be included
    or not.
-   columns_to(end_col, inclusive=True): get columns up to a specified
    end column. The inclusive argument indicates whether the ending
    column should be included or not.
-   columns_from(start_col): get the columns starting at a specified
    column.

### Exercise 4. {#exercise-4}

The selection filter functions are best explained by example. Let\'s say
I wanted to select only the columns that started with a \"c\":
:::

::: {.cell .code execution_count="7"}
``` python
# your solution goes here
cars >> select(starts_with("c"))
```

::: {.output .execute_result execution_count="7"}
```{=html}
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
      <th>cylinders</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>393</th>
      <td>4</td>
    </tr>
    <tr>
      <th>394</th>
      <td>4</td>
    </tr>
    <tr>
      <th>395</th>
      <td>4</td>
    </tr>
    <tr>
      <th>396</th>
      <td>4</td>
    </tr>
    <tr>
      <th>397</th>
      <td>4</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 1 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
### Exercise 5. {#exercise-5}

Select the columns that contain the substring \"e\" from the cars
DataFrame.
:::

::: {.cell .code execution_count="8"}
``` python
# your solution goes here
cars >> select(contains("e"))
```

::: {.output .execute_result execution_count="8"}
```{=html}
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
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>chevrolet chevelle malibu</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>buick skylark 320</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>plymouth satellite</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8</td>
      <td>304.0</td>
      <td>150.0</td>
      <td>3433</td>
      <td>12.0</td>
      <td>70</td>
      <td>amc rebel sst</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
      <td>302.0</td>
      <td>140.0</td>
      <td>3449</td>
      <td>10.5</td>
      <td>70</td>
      <td>ford torino</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>393</th>
      <td>4</td>
      <td>140.0</td>
      <td>86.0</td>
      <td>2790</td>
      <td>15.6</td>
      <td>82</td>
      <td>ford mustang gl</td>
    </tr>
    <tr>
      <th>394</th>
      <td>4</td>
      <td>97.0</td>
      <td>52.0</td>
      <td>2130</td>
      <td>24.6</td>
      <td>82</td>
      <td>vw pickup</td>
    </tr>
    <tr>
      <th>395</th>
      <td>4</td>
      <td>135.0</td>
      <td>84.0</td>
      <td>2295</td>
      <td>11.6</td>
      <td>82</td>
      <td>dodge rampage</td>
    </tr>
    <tr>
      <th>396</th>
      <td>4</td>
      <td>120.0</td>
      <td>79.0</td>
      <td>2625</td>
      <td>18.6</td>
      <td>82</td>
      <td>ford ranger</td>
    </tr>
    <tr>
      <th>397</th>
      <td>4</td>
      <td>119.0</td>
      <td>82.0</td>
      <td>2720</td>
      <td>19.4</td>
      <td>82</td>
      <td>chevy s-10</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 7 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
### Exercise 6. {#exercise-6}

Select the columns that are between \'mpg\' and \'origin\' from the cars
DataFrame.
:::

::: {.cell .code execution_count="9"}
``` python
# your solution goes here
cars >> select(columns_between("mpg", "origin"))
```

::: {.output .execute_result execution_count="9"}
```{=html}
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
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16.0</td>
      <td>8</td>
      <td>304.0</td>
      <td>150.0</td>
      <td>3433</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17.0</td>
      <td>8</td>
      <td>302.0</td>
      <td>140.0</td>
      <td>3449</td>
      <td>10.5</td>
      <td>70</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>393</th>
      <td>27.0</td>
      <td>4</td>
      <td>140.0</td>
      <td>86.0</td>
      <td>2790</td>
      <td>15.6</td>
      <td>82</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>394</th>
      <td>44.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>52.0</td>
      <td>2130</td>
      <td>24.6</td>
      <td>82</td>
      <td>europe</td>
    </tr>
    <tr>
      <th>395</th>
      <td>32.0</td>
      <td>4</td>
      <td>135.0</td>
      <td>84.0</td>
      <td>2295</td>
      <td>11.6</td>
      <td>82</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>396</th>
      <td>28.0</td>
      <td>4</td>
      <td>120.0</td>
      <td>79.0</td>
      <td>2625</td>
      <td>18.6</td>
      <td>82</td>
      <td>usa</td>
    </tr>
    <tr>
      <th>397</th>
      <td>31.0</td>
      <td>4</td>
      <td>119.0</td>
      <td>82.0</td>
      <td>2720</td>
      <td>19.4</td>
      <td>82</td>
      <td>usa</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 8 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
## Subsetting and filtering

### row_slice()

Slices of rows can be selected with the row_slice() function. You can
pass single integer indices or a list of indices to select rows as with.
This is going to be the same as using pandas\' .iloc.

#### Exercise 7. {#exercise-7}

Select the first three rows from the cars DataFrame.
:::

::: {.cell .code execution_count="11"}
``` python
# your solution goes here
cars >> row_slice([0, 1, 2])
```

::: {.output .execute_result execution_count="11"}
```{=html}
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
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevrolet chevelle malibu</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>usa</td>
      <td>buick skylark 320</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>usa</td>
      <td>plymouth satellite</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
### distinct()

Selection of unique rows is done with distinct(), which similarly passes
arguments and keyword arguments through to the DataFrame\'s
.drop_duplicates() method.

#### Exercise 8. {#exercise-8}

Select the unique rows from the \'origin\' column in the cars DataFrame.
:::

::: {.cell .code execution_count="12"}
``` python
# your solution goes here
cars >> distinct(X.origin)
```

::: {.output .execute_result execution_count="12"}
```{=html}
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
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevrolet chevelle malibu</td>
    </tr>
    <tr>
      <th>14</th>
      <td>24.0</td>
      <td>4</td>
      <td>113.0</td>
      <td>95.0</td>
      <td>2372</td>
      <td>15.0</td>
      <td>70</td>
      <td>japan</td>
      <td>toyota corona mark ii</td>
    </tr>
    <tr>
      <th>19</th>
      <td>26.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>46.0</td>
      <td>1835</td>
      <td>20.5</td>
      <td>70</td>
      <td>europe</td>
      <td>volkswagen 1131 deluxe sedan</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
## mask()

Filtering rows with logical criteria is done with mask(), which accepts
boolean arrays \"masking out\" False labeled rows and keeping True
labeled rows. These are best created with logical statements on symbolic
Series objects as shown below. Multiple criteria can be supplied as
arguments and their intersection will be used as the mask.

### Exercise 9. {#exercise-9}

Filter the cars DataFrame to only include rows where the \'mpg\' is
greater than 20, origin Japan, and display the first three rows:
:::

::: {.cell .code execution_count="13"}
``` python
# your solution goes here
cars >> mask(X.mpg > 20, X.origin == "japan") >> head(3)
```

::: {.output .execute_result execution_count="13"}
```{=html}
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
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>14</th>
      <td>24.0</td>
      <td>4</td>
      <td>113.0</td>
      <td>95.0</td>
      <td>2372</td>
      <td>15.0</td>
      <td>70</td>
      <td>japan</td>
      <td>toyota corona mark ii</td>
    </tr>
    <tr>
      <th>18</th>
      <td>27.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>88.0</td>
      <td>2130</td>
      <td>14.5</td>
      <td>70</td>
      <td>japan</td>
      <td>datsun pl510</td>
    </tr>
    <tr>
      <th>29</th>
      <td>27.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>88.0</td>
      <td>2130</td>
      <td>14.5</td>
      <td>71</td>
      <td>japan</td>
      <td>datsun pl510</td>
    </tr>
  </tbody>
</table>
</div>
```
:::
:::

::: {.cell .markdown}
## pull()

The pull() function is used to extract a single column from a DataFrame
as a pandas Series. This is useful for passing a single column to a
function or for further manipulation.

### Exercise 10. {#exercise-10}

Extract the \'mpg\' column from the cars DataFrame, japanese origin,
model year 70s, and display the first three rows.
:::

::: {.cell .code execution_count="35"}
``` python
# your solution goes here
```
:::

::: {.cell .markdown}
## DataFrame transformation

*mutate()*

The mutate() function is used to create new columns or modify existing
columns. It accepts keyword arguments of the form new_column_name =
new_column_value, where new_column_value is a symbolic Series object.

### Exercise 11. {#exercise-11}

Create a new column \'mpg_per_cylinder\' in the cars DataFrame that is
the result of dividing the \'mpg\' column by the \'cylinders\' column.
:::

::: {.cell .code execution_count="15"}
``` python
# your solution goes here
cars >> mutate(mpg_per_cylinder=X.mpg / X.cylinders)
```

::: {.output .execute_result execution_count="15"}
```{=html}
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
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model_year</th>
      <th>origin</th>
      <th>name</th>
      <th>mpg_per_cylinder</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130.0</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>chevrolet chevelle malibu</td>
      <td>2.250</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165.0</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>usa</td>
      <td>buick skylark 320</td>
      <td>1.875</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150.0</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>usa</td>
      <td>plymouth satellite</td>
      <td>2.250</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16.0</td>
      <td>8</td>
      <td>304.0</td>
      <td>150.0</td>
      <td>3433</td>
      <td>12.0</td>
      <td>70</td>
      <td>usa</td>
      <td>amc rebel sst</td>
      <td>2.000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17.0</td>
      <td>8</td>
      <td>302.0</td>
      <td>140.0</td>
      <td>3449</td>
      <td>10.5</td>
      <td>70</td>
      <td>usa</td>
      <td>ford torino</td>
      <td>2.125</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>393</th>
      <td>27.0</td>
      <td>4</td>
      <td>140.0</td>
      <td>86.0</td>
      <td>2790</td>
      <td>15.6</td>
      <td>82</td>
      <td>usa</td>
      <td>ford mustang gl</td>
      <td>6.750</td>
    </tr>
    <tr>
      <th>394</th>
      <td>44.0</td>
      <td>4</td>
      <td>97.0</td>
      <td>52.0</td>
      <td>2130</td>
      <td>24.6</td>
      <td>82</td>
      <td>europe</td>
      <td>vw pickup</td>
      <td>11.000</td>
    </tr>
    <tr>
      <th>395</th>
      <td>32.0</td>
      <td>4</td>
      <td>135.0</td>
      <td>84.0</td>
      <td>2295</td>
      <td>11.6</td>
      <td>82</td>
      <td>usa</td>
      <td>dodge rampage</td>
      <td>8.000</td>
    </tr>
    <tr>
      <th>396</th>
      <td>28.0</td>
      <td>4</td>
      <td>120.0</td>
      <td>79.0</td>
      <td>2625</td>
      <td>18.6</td>
      <td>82</td>
      <td>usa</td>
      <td>ford ranger</td>
      <td>7.000</td>
    </tr>
    <tr>
      <th>397</th>
      <td>31.0</td>
      <td>4</td>
      <td>119.0</td>
      <td>82.0</td>
      <td>2720</td>
      <td>19.4</td>
      <td>82</td>
      <td>usa</td>
      <td>chevy s-10</td>
      <td>7.750</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 10 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
*transmute()*

The transmute() function is a combination of a mutate and a selection of
the created variables.

### Exercise 12. {#exercise-12}

Create a new column \'mpg_per_cylinder\' in the cars DataFrame that is
the result of dividing the \'mpg\' column by the \'cylinders\' column,
and display only the new column.
:::

::: {.cell .code execution_count="16"}
``` python
# your solution goes here
cars >> transmute(mpg_per_cylinder=X.mpg / X.cylinders)
```

::: {.output .execute_result execution_count="16"}
```{=html}
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
      <th>mpg_per_cylinder</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.250</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.875</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2.250</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2.125</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>393</th>
      <td>6.750</td>
    </tr>
    <tr>
      <th>394</th>
      <td>11.000</td>
    </tr>
    <tr>
      <th>395</th>
      <td>8.000</td>
    </tr>
    <tr>
      <th>396</th>
      <td>7.000</td>
    </tr>
    <tr>
      <th>397</th>
      <td>7.750</td>
    </tr>
  </tbody>
</table>
<p>398 rows × 1 columns</p>
</div>
```
:::
:::

::: {.cell .markdown}
## Grouping

*group_by() and ungroup()*

The group_by() function is used to group the DataFrame by one or more
columns. This is useful for creating groups of rows that can be
summarized or transformed together. The ungroup() function is used to
remove the grouping.

### Exercise 13. {#exercise-13}

Group the cars DataFrame by the \'origin\' column and calculate the lead
of the \'mpg\' column.
:::

::: {.cell .code}
``` python
# your solution goes here
```
:::

::: {.cell .markdown}
## Reshaping

*arrange()*

The arrange() function is used to sort the DataFrame by one or more
columns. This is useful for reordering the rows of the DataFrame.

### Exercise 14. {#exercise-14}

Sort the cars DataFrame by the \'mpg\' column in descending order.
:::

::: {.cell .code}
``` python
# your solution goes here
```
:::

::: {.cell .markdown}
*rename()*

The rename() function is used to rename columns in the DataFrame. It
accepts keyword arguments of the form new_column_name = old_column_name.

### Exercise 15. {#exercise-15}

Rename the \'mpg\' column to \'miles_per_gallon\' in the cars DataFrame.
:::

::: {.cell .code}
``` python
# your solution goes here
```
:::

::: {.cell .markdown}
*gather()*

The gather() function is used to reshape the DataFrame from wide to long
format. It accepts keyword arguments of the form new_column_name =
new_column_value, where new_column_value is a symbolic Series object.

### Exercise 16. {#exercise-16}

Reshape the cars DataFrame from wide to long format by gathering the
columns \'mpg\', \'horsepower\', \'weight\', \'acceleration\', and
\'displacement\' into a new column \'variable\' and their values into a
new column \'value\'.
:::

::: {.cell .code}
``` python
# your solution goes here
```
:::

::: {.cell .markdown}
*spread()*

Likewise, you can transform a \"long\" DataFrame into a \"wide\" format
with the spread(key, values) function. Converting the previously created
elongated DataFrame for example would be done like so.

### Exercise 17. {#exercise-17}

Reshape the cars DataFrame from long to wide format by spreading the
\'variable\' column into columns and their values into the \'value\'
column.
:::

::: {.cell .code}
``` python
# your solution goes here
```
:::

::: {.cell .markdown}
## Summarization

*summarize()*

The summarize() function is used to calculate summary statistics for
groups of rows. It accepts keyword arguments of the form new_column_name
= new_column_value, where new_column_value is a symbolic Series object.

### Exercise 18. {#exercise-18}

Calculate the mean \'mpg\' for each group of \'origin\' in the cars
DataFrame.
:::

::: {.cell .code}
``` python
# your solution goes here
```
:::

::: {.cell .markdown}
*summarize_each()*

The summarize_each() function is used to calculate summary statistics
for groups of rows. It accepts keyword arguments of the form
new_column_name = new_column_value, where new_column_value is a symbolic
Series object.

### Exercise 19. {#exercise-19}

Calculate the mean \'mpg\' and \'horsepower\' for each group of
\'origin\' in the cars DataFrame.
:::

::: {.cell .code}
``` python
# your solution goes here
```
:::

::: {.cell .markdown}
*summarize() can of course be used with groupings as well.*

### Exercise 20. {#exercise-20}

Calculate the mean \'mpg\' for each group of \'origin\' and
\'model_year\' in the cars DataFrame.
:::

::: {.cell .code}
``` python
# your solution goes here
```
:::
