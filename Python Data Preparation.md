# Walmart Data Analysis

## Data Loading & Exploration :

### Importing dependencies:


```python

import pandas as pd 
import numpy as np

```

### MYSQL Toolkit :


```python
import pymysql 
from sqlalchemy import create_engine 
```

### Loading Dataset:


```python
df = pd.read_csv("Walmart.csv", encoding_errors = 'ignore' )
```

### Exploring Dataset


```python
df.shape
```




    (10051, 11)




```python
df.head()
```




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
      <th>invoice_id</th>
      <th>Branch</th>
      <th>City</th>
      <th>category</th>
      <th>unit_price</th>
      <th>quantity</th>
      <th>date</th>
      <th>time</th>
      <th>payment_method</th>
      <th>rating</th>
      <th>profit_margin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>WALM003</td>
      <td>San Antonio</td>
      <td>Health and beauty</td>
      <td>$74.69</td>
      <td>7.0</td>
      <td>05/01/19</td>
      <td>13:08:00</td>
      <td>Ewallet</td>
      <td>9.1</td>
      <td>0.48</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>WALM048</td>
      <td>Harlingen</td>
      <td>Electronic accessories</td>
      <td>$15.28</td>
      <td>5.0</td>
      <td>08/03/19</td>
      <td>10:29:00</td>
      <td>Cash</td>
      <td>9.6</td>
      <td>0.48</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>WALM067</td>
      <td>Haltom City</td>
      <td>Home and lifestyle</td>
      <td>$46.33</td>
      <td>7.0</td>
      <td>03/03/19</td>
      <td>13:23:00</td>
      <td>Credit card</td>
      <td>7.4</td>
      <td>0.33</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>WALM064</td>
      <td>Bedford</td>
      <td>Health and beauty</td>
      <td>$58.22</td>
      <td>8.0</td>
      <td>27/01/19</td>
      <td>20:33:00</td>
      <td>Ewallet</td>
      <td>8.4</td>
      <td>0.33</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>WALM013</td>
      <td>Irving</td>
      <td>Sports and travel</td>
      <td>$86.31</td>
      <td>7.0</td>
      <td>08/02/19</td>
      <td>10:37:00</td>
      <td>Ewallet</td>
      <td>5.3</td>
      <td>0.48</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.describe()
```




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
      <th>invoice_id</th>
      <th>quantity</th>
      <th>rating</th>
      <th>profit_margin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>10051.000000</td>
      <td>10020.000000</td>
      <td>10051.000000</td>
      <td>10051.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>5025.741220</td>
      <td>2.353493</td>
      <td>5.825659</td>
      <td>0.393791</td>
    </tr>
    <tr>
      <th>std</th>
      <td>2901.174372</td>
      <td>1.602658</td>
      <td>1.763991</td>
      <td>0.090669</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>0.180000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2513.500000</td>
      <td>1.000000</td>
      <td>4.000000</td>
      <td>0.330000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>5026.000000</td>
      <td>2.000000</td>
      <td>6.000000</td>
      <td>0.330000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>7538.500000</td>
      <td>3.000000</td>
      <td>7.000000</td>
      <td>0.480000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>10000.000000</td>
      <td>10.000000</td>
      <td>10.000000</td>
      <td>0.570000</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 10051 entries, 0 to 10050
    Data columns (total 11 columns):
     #   Column          Non-Null Count  Dtype  
    ---  ------          --------------  -----  
     0   invoice_id      10051 non-null  int64  
     1   Branch          10051 non-null  object 
     2   City            10051 non-null  object 
     3   category        10051 non-null  object 
     4   unit_price      10020 non-null  object 
     5   quantity        10020 non-null  float64
     6   date            10051 non-null  object 
     7   time            10051 non-null  object 
     8   payment_method  10051 non-null  object 
     9   rating          10051 non-null  float64
     10  profit_margin   10051 non-null  float64
    dtypes: float64(3), int64(1), object(7)
    memory usage: 863.9+ KB
    

## Data Cleaning & Transformation : 


```python
# find out if there any duplicates and total of them
df.duplicated().sum()
```




    51




```python
df.isnull().sum()
```




    invoice_id         0
    Branch             0
    City               0
    category           0
    unit_price        31
    quantity          31
    date               0
    time               0
    payment_method     0
    rating             0
    profit_margin      0
    dtype: int64




```python
# Remove duplicates
df.drop_duplicates(inplace = True)
```


```python
# verify if duplicates has been removed 
df.duplicated().sum()
```




    0




```python
# drop rows has missing record 
df.dropna(inplace = True)
```


```python
# check out if the rows has been removed 
df.isnull().sum()
```




    invoice_id        0
    Branch            0
    City              0
    category          0
    unit_price        0
    quantity          0
    date              0
    time              0
    payment_method    0
    rating            0
    profit_margin     0
    dtype: int64




```python
df.dtypes
```




    invoice_id          int64
    Branch             object
    City               object
    category           object
    unit_price         object
    quantity          float64
    date               object
    time               object
    payment_method     object
    rating            float64
    profit_margin     float64
    dtype: object




```python
# remove '$' and change data type for the unit price column 
df['unit_price']= df['unit_price'].str.replace('$','').astype(float)
df.head()
```




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
      <th>invoice_id</th>
      <th>Branch</th>
      <th>City</th>
      <th>category</th>
      <th>unit_price</th>
      <th>quantity</th>
      <th>date</th>
      <th>time</th>
      <th>payment_method</th>
      <th>rating</th>
      <th>profit_margin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>WALM003</td>
      <td>San Antonio</td>
      <td>Health and beauty</td>
      <td>74.69</td>
      <td>7.0</td>
      <td>05/01/19</td>
      <td>13:08:00</td>
      <td>Ewallet</td>
      <td>9.1</td>
      <td>0.48</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>WALM048</td>
      <td>Harlingen</td>
      <td>Electronic accessories</td>
      <td>15.28</td>
      <td>5.0</td>
      <td>08/03/19</td>
      <td>10:29:00</td>
      <td>Cash</td>
      <td>9.6</td>
      <td>0.48</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>WALM067</td>
      <td>Haltom City</td>
      <td>Home and lifestyle</td>
      <td>46.33</td>
      <td>7.0</td>
      <td>03/03/19</td>
      <td>13:23:00</td>
      <td>Credit card</td>
      <td>7.4</td>
      <td>0.33</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>WALM064</td>
      <td>Bedford</td>
      <td>Health and beauty</td>
      <td>58.22</td>
      <td>8.0</td>
      <td>27/01/19</td>
      <td>20:33:00</td>
      <td>Ewallet</td>
      <td>8.4</td>
      <td>0.33</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>WALM013</td>
      <td>Irving</td>
      <td>Sports and travel</td>
      <td>86.31</td>
      <td>7.0</td>
      <td>08/02/19</td>
      <td>10:37:00</td>
      <td>Ewallet</td>
      <td>5.3</td>
      <td>0.48</td>
    </tr>
  </tbody>
</table>
</div>




```python
# veryfing that changes has been done 
df.dtypes
```




    invoice_id          int64
    Branch             object
    City               object
    category           object
    unit_price        float64
    quantity          float64
    date               object
    time               object
    payment_method     object
    rating            float64
    profit_margin     float64
    dtype: object




```python
df.columns
```




    Index(['invoice_id', 'Branch', 'City', 'category', 'unit_price', 'quantity',
           'date', 'time', 'payment_method', 'rating', 'profit_margin'],
          dtype='object')




```python
df.columns = df.columns.str.lower()
df.columns
```




    Index(['invoice_id', 'branch', 'city', 'category', 'unit_price', 'quantity',
           'date', 'time', 'payment_method', 'rating', 'profit_margin', 'total'],
          dtype='object')




```python
# add total amount column 
df['total'] = df['unit_price'] * df['quantity']
df.head()
```




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
      <th>invoice_id</th>
      <th>branch</th>
      <th>city</th>
      <th>category</th>
      <th>unit_price</th>
      <th>quantity</th>
      <th>date</th>
      <th>time</th>
      <th>payment_method</th>
      <th>rating</th>
      <th>profit_margin</th>
      <th>total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>WALM003</td>
      <td>San Antonio</td>
      <td>Health and beauty</td>
      <td>74.69</td>
      <td>7.0</td>
      <td>05/01/19</td>
      <td>13:08:00</td>
      <td>Ewallet</td>
      <td>9.1</td>
      <td>0.48</td>
      <td>522.83</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>WALM048</td>
      <td>Harlingen</td>
      <td>Electronic accessories</td>
      <td>15.28</td>
      <td>5.0</td>
      <td>08/03/19</td>
      <td>10:29:00</td>
      <td>Cash</td>
      <td>9.6</td>
      <td>0.48</td>
      <td>76.40</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>WALM067</td>
      <td>Haltom City</td>
      <td>Home and lifestyle</td>
      <td>46.33</td>
      <td>7.0</td>
      <td>03/03/19</td>
      <td>13:23:00</td>
      <td>Credit card</td>
      <td>7.4</td>
      <td>0.33</td>
      <td>324.31</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>WALM064</td>
      <td>Bedford</td>
      <td>Health and beauty</td>
      <td>58.22</td>
      <td>8.0</td>
      <td>27/01/19</td>
      <td>20:33:00</td>
      <td>Ewallet</td>
      <td>8.4</td>
      <td>0.33</td>
      <td>465.76</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>WALM013</td>
      <td>Irving</td>
      <td>Sports and travel</td>
      <td>86.31</td>
      <td>7.0</td>
      <td>08/02/19</td>
      <td>10:37:00</td>
      <td>Ewallet</td>
      <td>5.3</td>
      <td>0.48</td>
      <td>604.17</td>
    </tr>
  </tbody>
</table>
</div>



## Connect & Load Dataset To **MYSQL**


```python
#mysql 
#host = localhost
#port = 3306
#user = root
#password = ****
```


```python
#myesql connection
engine_mysql = create_engine("mysql+pymysql://root:***@localhost:3306/walmart")

try:
    engine_mysql
    print("Connection Successed to PSQL")
except:
    print("Unable to Connect")
```

    Connection Successed to PSQL
    


```python
# import the dataset to mysql

df.to_sql(name= "walmart", con = engine_mysql, if_exists = "append", index = False)
```







```python

```
