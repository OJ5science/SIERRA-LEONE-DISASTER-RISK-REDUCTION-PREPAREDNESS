## THIS IS THE GAMEPLAN, LOCK IN!
# Are We Ready? Building Disaster Resilience in Africa Through Data-Driven Preparedness Assessment and Investment Prioritisation

## 1. Project Overview

## 2. Import Libraries

## 3. Load Data

## 4. Inspect the Dataset

## 5. Data Quality Assessment

## 6. Descriptive Statistics

## 7. Exploratory Data Analysis

## 8. Correlation Analysis

## 9. Regression Analysis

## 10. Preparedness Gap Assessment

## 11. Policy Implications

## 12. Conclusions

# Are We Ready? Building Disaster Resilience in Africa Through Data-Driven Preparedness Assessment and Investment Prioritisation

## Aim

To assess disaster preparedness and humanitarian resilience across African countries, identify the socioeconomic, governance and health-system factors associated with preparedness gaps, and develop a decision-support framework that highlights priority areas for investment under constrained funding environments.

This notebook contains the statistical analyses used to answer the project's research questions.

*IMPORT LIBRARIES*


```python
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt

import statsmodels.api as sm
import statsmodels.formula.api as smf

from scipy import stats
```


```python
import pandas as pd

file = r"C:\Users\OJ\Documents\V2 DISASTER PREPAREDNESS DATA.xlsx"

df = pd.read_excel(
    file,
    sheet_name="AFRICA MASTER SHEET"
)

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
      <th>ISO3</th>
      <th>REGION</th>
      <th>COUNTRY_NAME</th>
      <th>GDP_PER_CAPITA</th>
      <th>TOTAL_POPULATION</th>
      <th>WGI_SCORE</th>
      <th>GHSI</th>
      <th>INFORM_RISK_SCORE</th>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <th>GINI</th>
      <th>DISASTER COUNT</th>
      <th>DEATHS FROM DISASTER COUNT</th>
      <th>TOTAL PEOPLE AFFECTED BY DISASTER</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AGO</td>
      <td>Africa</td>
      <td>Angola</td>
      <td>3129.476623</td>
      <td>39040039</td>
      <td>36.073211</td>
      <td>29.1</td>
      <td>6.0</td>
      <td>33.870996</td>
      <td>36.746709</td>
      <td>51.3</td>
      <td>4227</td>
      <td>144285.0</td>
      <td>135483741</td>
    </tr>
    <tr>
      <th>1</th>
      <td>BDI</td>
      <td>Africa</td>
      <td>Burundi</td>
      <td>233.823013</td>
      <td>14390003</td>
      <td>31.778105</td>
      <td>22.1</td>
      <td>6.4</td>
      <td>4.147275</td>
      <td>7.834755</td>
      <td>37.5</td>
      <td>11307</td>
      <td>24675.0</td>
      <td>128452097</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BEN</td>
      <td>Africa</td>
      <td>Benin</td>
      <td>1658.273127</td>
      <td>14814460</td>
      <td>47.735147</td>
      <td>25.4</td>
      <td>5.6</td>
      <td>6.824348</td>
      <td>21.825727</td>
      <td>34.4</td>
      <td>1942</td>
      <td>1425.0</td>
      <td>35265168</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BFA</td>
      <td>Africa</td>
      <td>Burkina Faso</td>
      <td>1147.571316</td>
      <td>24074580</td>
      <td>34.329539</td>
      <td>29.8</td>
      <td>7.2</td>
      <td>22.556645</td>
      <td>34.168802</td>
      <td>37.4</td>
      <td>14581</td>
      <td>35174.0</td>
      <td>118481987</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BWA</td>
      <td>Africa</td>
      <td>Botswana</td>
      <td>7778.115109</td>
      <td>2562122</td>
      <td>59.446482</td>
      <td>33.6</td>
      <td>3.3</td>
      <td>369.456459</td>
      <td>88.148375</td>
      <td>NaN</td>
      <td>171</td>
      <td>35.0</td>
      <td>872777</td>
    </tr>
  </tbody>
</table>
</div>



*IDIOT CHECK - DATA SIZE, VARIABLES, MISSING STUFF, DUPLICATES ETC*


```python
df.shape
```




    (54, 14)




```python
df.columns.tolist()
```




    ['ISO3',
     'REGION',
     'COUNTRY_NAME',
     'GDP_PER_CAPITA',
     'TOTAL_POPULATION ',
     'WGI_SCORE',
     'GHSI',
     'INFORM_RISK_SCORE',
     'GOV_HEALTH_EXPENDITURE',
     'PRIVATE_HEALTH_EXPENDITURE',
     'GINI',
     'DISASTER COUNT',
     'DEATHS FROM DISASTER COUNT',
     'TOTAL PEOPLE AFFECTED BY DISASTER']




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 54 entries, 0 to 53
    Data columns (total 14 columns):
     #   Column                             Non-Null Count  Dtype  
    ---  ------                             --------------  -----  
     0   ISO3                               54 non-null     object 
     1   REGION                             54 non-null     object 
     2   COUNTRY_NAME                       54 non-null     object 
     3   GDP_PER_CAPITA                     52 non-null     float64
     4   TOTAL_POPULATION                   54 non-null     int64  
     5   WGI_SCORE                          54 non-null     float64
     6   GHSI                               54 non-null     float64
     7   INFORM_RISK_SCORE                  54 non-null     float64
     8   GOV_HEALTH_EXPENDITURE             54 non-null     float64
     9   PRIVATE_HEALTH_EXPENDITURE         54 non-null     float64
     10  GINI                               35 non-null     float64
     11  DISASTER COUNT                     54 non-null     int64  
     12  DEATHS FROM DISASTER COUNT         53 non-null     float64
     13  TOTAL PEOPLE AFFECTED BY DISASTER  54 non-null     int64  
    dtypes: float64(8), int64(3), object(3)
    memory usage: 6.0+ KB
    


```python
df.describe().T
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
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>GDP_PER_CAPITA</th>
      <td>52.0</td>
      <td>3.101761e+03</td>
      <td>3.423582e+03</td>
      <td>233.823013</td>
      <td>1.072306e+03</td>
      <td>1.915983e+03</td>
      <td>4.090364e+03</td>
      <td>1.944938e+04</td>
    </tr>
    <tr>
      <th>TOTAL_POPULATION</th>
      <td>54.0</td>
      <td>2.864588e+07</td>
      <td>4.161571e+07</td>
      <td>122730.000000</td>
      <td>3.221363e+06</td>
      <td>1.495709e+07</td>
      <td>3.448337e+07</td>
      <td>2.375278e+08</td>
    </tr>
    <tr>
      <th>WGI_SCORE</th>
      <td>54.0</td>
      <td>3.818565e+01</td>
      <td>1.266076e+01</td>
      <td>9.091619</td>
      <td>3.039883e+01</td>
      <td>3.707822e+01</td>
      <td>4.625575e+01</td>
      <td>6.622202e+01</td>
    </tr>
    <tr>
      <th>GHSI</th>
      <td>54.0</td>
      <td>2.910926e+01</td>
      <td>5.763328e+00</td>
      <td>16.000000</td>
      <td>2.612500e+01</td>
      <td>2.885000e+01</td>
      <td>3.262500e+01</td>
      <td>4.580000e+01</td>
    </tr>
    <tr>
      <th>INFORM_RISK_SCORE</th>
      <td>54.0</td>
      <td>5.375926e+00</td>
      <td>1.612210e+00</td>
      <td>1.400000</td>
      <td>4.300000e+00</td>
      <td>5.350000e+00</td>
      <td>6.475000e+00</td>
      <td>8.500000e+00</td>
    </tr>
    <tr>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <td>54.0</td>
      <td>7.095570e+01</td>
      <td>1.147624e+02</td>
      <td>3.140486</td>
      <td>7.980482e+00</td>
      <td>2.027518e+01</td>
      <td>6.162940e+01</td>
      <td>5.384608e+02</td>
    </tr>
    <tr>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <td>54.0</td>
      <td>5.428884e+01</td>
      <td>6.180306e+01</td>
      <td>5.760867</td>
      <td>1.556775e+01</td>
      <td>3.343180e+01</td>
      <td>7.529891e+01</td>
      <td>3.309364e+02</td>
    </tr>
    <tr>
      <th>GINI</th>
      <td>35.0</td>
      <td>3.844000e+01</td>
      <td>6.584572e+00</td>
      <td>28.500000</td>
      <td>3.380000e+01</td>
      <td>3.740000e+01</td>
      <td>4.135000e+01</td>
      <td>5.410000e+01</td>
    </tr>
    <tr>
      <th>DISASTER COUNT</th>
      <td>54.0</td>
      <td>9.196500e+03</td>
      <td>1.325536e+04</td>
      <td>30.000000</td>
      <td>7.725000e+02</td>
      <td>3.342500e+03</td>
      <td>1.269975e+04</td>
      <td>5.717900e+04</td>
    </tr>
    <tr>
      <th>DEATHS FROM DISASTER COUNT</th>
      <td>53.0</td>
      <td>2.097147e+04</td>
      <td>3.860760e+04</td>
      <td>2.000000</td>
      <td>3.270000e+02</td>
      <td>1.701000e+03</td>
      <td>2.001100e+04</td>
      <td>1.466520e+05</td>
    </tr>
    <tr>
      <th>TOTAL PEOPLE AFFECTED BY DISASTER</th>
      <td>54.0</td>
      <td>2.243375e+08</td>
      <td>4.879260e+08</td>
      <td>148365.000000</td>
      <td>9.033180e+06</td>
      <td>5.596255e+07</td>
      <td>1.337258e+08</td>
      <td>2.239187e+09</td>
    </tr>
  </tbody>
</table>
</div>




```python
missing = pd.DataFrame({
    "Missing Values": df.isnull().sum(),
    "Percent Missing": round(df.isnull().mean() * 100, 2)
})

missing.sort_values("Percent Missing", ascending=False)
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
      <th>Missing Values</th>
      <th>Percent Missing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>GINI</th>
      <td>19</td>
      <td>35.19</td>
    </tr>
    <tr>
      <th>GDP_PER_CAPITA</th>
      <td>2</td>
      <td>3.70</td>
    </tr>
    <tr>
      <th>DEATHS FROM DISASTER COUNT</th>
      <td>1</td>
      <td>1.85</td>
    </tr>
    <tr>
      <th>COUNTRY_NAME</th>
      <td>0</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>REGION</th>
      <td>0</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>ISO3</th>
      <td>0</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>WGI_SCORE</th>
      <td>0</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>TOTAL_POPULATION</th>
      <td>0</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>GHSI</th>
      <td>0</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>INFORM_RISK_SCORE</th>
      <td>0</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <td>0</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <td>0</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>DISASTER COUNT</th>
      <td>0</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>TOTAL PEOPLE AFFECTED BY DISASTER</th>
      <td>0</td>
      <td>0.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
df["ISO3"].duplicated().sum()
```




    np.int64(0)




```python
df.columns.tolist()
```




    ['ISO3',
     'REGION',
     'COUNTRY_NAME',
     'GDP_PER_CAPITA',
     'TOTAL_POPULATION ',
     'WGI_SCORE',
     'GHSI',
     'INFORM_RISK_SCORE',
     'GOV_HEALTH_EXPENDITURE',
     'PRIVATE_HEALTH_EXPENDITURE',
     'GINI',
     'DISASTER COUNT',
     'DEATHS FROM DISASTER COUNT',
     'TOTAL PEOPLE AFFECTED BY DISASTER']




```python
df.columns = df.columns.str.strip()
```


```python
df.columns.tolist()
```




    ['ISO3',
     'REGION',
     'COUNTRY_NAME',
     'GDP_PER_CAPITA',
     'TOTAL_POPULATION',
     'WGI_SCORE',
     'GHSI',
     'INFORM_RISK_SCORE',
     'GOV_HEALTH_EXPENDITURE',
     'PRIVATE_HEALTH_EXPENDITURE',
     'GINI',
     'DISASTER COUNT',
     'DEATHS FROM DISASTER COUNT',
     'TOTAL PEOPLE AFFECTED BY DISASTER']




```python
numeric_columns = [
    "GDP_PER_CAPITA",
    "TOTAL_POPULATION",
    "WGI_SCORE",
    "GHSI",
    "INFORM_RISK_SCORE",
    "GOV_HEALTH_EXPENDITURE",
    "PRIVATE_HEALTH_EXPENDITURE",
    "GINI",
    "DISASTER COUNT",
    "DEATHS FROM DISASTER COUNT",
    "TOTAL PEOPLE AFFECTED BY DISASTER"
]

df[numeric_columns].dtypes
```




    GDP_PER_CAPITA                       float64
    TOTAL_POPULATION                       int64
    WGI_SCORE                            float64
    GHSI                                 float64
    INFORM_RISK_SCORE                    float64
    GOV_HEALTH_EXPENDITURE               float64
    PRIVATE_HEALTH_EXPENDITURE           float64
    GINI                                 float64
    DISASTER COUNT                         int64
    DEATHS FROM DISASTER COUNT           float64
    TOTAL PEOPLE AFFECTED BY DISASTER      int64
    dtype: object



# LET'S USE LOG

THERES NO 0 DEATHS IN MY SHEET SO MAYBE 0S WERE BLANKS


```python
import numpy as np

df_clean = df.copy()

df_clean["log_GDP"] = np.log(df_clean["GDP_PER_CAPITA"])

df_clean["log_population"] = np.log(df_clean["TOTAL_POPULATION"])

df_clean["log_disaster_count"] = np.log1p(df_clean["DISASTER COUNT"])

df_clean["log_disaster_deaths"] = np.log1p(df_clean["DEATHS FROM DISASTER COUNT"])

df_clean["log_people_affected"] = np.log1p(df_clean["TOTAL PEOPLE AFFECTED BY DISASTER"])

df_clean["log_gov_health"] = np.log(df_clean["GOV_HEALTH_EXPENDITURE"])

df_clean["log_private_health"] = np.log(df_clean["PRIVATE_HEALTH_EXPENDITURE"])
```


```python
df_clean.describe().T
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
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>GDP_PER_CAPITA</th>
      <td>52.0</td>
      <td>3.101761e+03</td>
      <td>3.423582e+03</td>
      <td>233.823013</td>
      <td>1.072306e+03</td>
      <td>1.915983e+03</td>
      <td>4.090364e+03</td>
      <td>1.944938e+04</td>
    </tr>
    <tr>
      <th>TOTAL_POPULATION</th>
      <td>54.0</td>
      <td>2.864588e+07</td>
      <td>4.161571e+07</td>
      <td>122730.000000</td>
      <td>3.221363e+06</td>
      <td>1.495709e+07</td>
      <td>3.448337e+07</td>
      <td>2.375278e+08</td>
    </tr>
    <tr>
      <th>WGI_SCORE</th>
      <td>54.0</td>
      <td>3.818565e+01</td>
      <td>1.266076e+01</td>
      <td>9.091619</td>
      <td>3.039883e+01</td>
      <td>3.707822e+01</td>
      <td>4.625575e+01</td>
      <td>6.622202e+01</td>
    </tr>
    <tr>
      <th>GHSI</th>
      <td>54.0</td>
      <td>2.910926e+01</td>
      <td>5.763328e+00</td>
      <td>16.000000</td>
      <td>2.612500e+01</td>
      <td>2.885000e+01</td>
      <td>3.262500e+01</td>
      <td>4.580000e+01</td>
    </tr>
    <tr>
      <th>INFORM_RISK_SCORE</th>
      <td>54.0</td>
      <td>5.375926e+00</td>
      <td>1.612210e+00</td>
      <td>1.400000</td>
      <td>4.300000e+00</td>
      <td>5.350000e+00</td>
      <td>6.475000e+00</td>
      <td>8.500000e+00</td>
    </tr>
    <tr>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <td>54.0</td>
      <td>7.095570e+01</td>
      <td>1.147624e+02</td>
      <td>3.140486</td>
      <td>7.980482e+00</td>
      <td>2.027518e+01</td>
      <td>6.162940e+01</td>
      <td>5.384608e+02</td>
    </tr>
    <tr>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <td>54.0</td>
      <td>5.428884e+01</td>
      <td>6.180306e+01</td>
      <td>5.760867</td>
      <td>1.556775e+01</td>
      <td>3.343180e+01</td>
      <td>7.529891e+01</td>
      <td>3.309364e+02</td>
    </tr>
    <tr>
      <th>GINI</th>
      <td>35.0</td>
      <td>3.844000e+01</td>
      <td>6.584572e+00</td>
      <td>28.500000</td>
      <td>3.380000e+01</td>
      <td>3.740000e+01</td>
      <td>4.135000e+01</td>
      <td>5.410000e+01</td>
    </tr>
    <tr>
      <th>DISASTER COUNT</th>
      <td>54.0</td>
      <td>9.196500e+03</td>
      <td>1.325536e+04</td>
      <td>30.000000</td>
      <td>7.725000e+02</td>
      <td>3.342500e+03</td>
      <td>1.269975e+04</td>
      <td>5.717900e+04</td>
    </tr>
    <tr>
      <th>DEATHS FROM DISASTER COUNT</th>
      <td>53.0</td>
      <td>2.097147e+04</td>
      <td>3.860760e+04</td>
      <td>2.000000</td>
      <td>3.270000e+02</td>
      <td>1.701000e+03</td>
      <td>2.001100e+04</td>
      <td>1.466520e+05</td>
    </tr>
    <tr>
      <th>TOTAL PEOPLE AFFECTED BY DISASTER</th>
      <td>54.0</td>
      <td>2.243375e+08</td>
      <td>4.879260e+08</td>
      <td>148365.000000</td>
      <td>9.033180e+06</td>
      <td>5.596255e+07</td>
      <td>1.337258e+08</td>
      <td>2.239187e+09</td>
    </tr>
    <tr>
      <th>log_GDP</th>
      <td>52.0</td>
      <td>7.610631e+00</td>
      <td>9.179152e-01</td>
      <td>5.454564</td>
      <td>6.977197e+00</td>
      <td>7.557782e+00</td>
      <td>8.316386e+00</td>
      <td>9.875571e+00</td>
    </tr>
    <tr>
      <th>log_population</th>
      <td>54.0</td>
      <td>1.621811e+01</td>
      <td>1.629276e+00</td>
      <td>11.717742</td>
      <td>1.498304e+01</td>
      <td>1.652065e+01</td>
      <td>1.735555e+01</td>
      <td>1.928580e+01</td>
    </tr>
    <tr>
      <th>log_disaster_count</th>
      <td>54.0</td>
      <td>7.928443e+00</td>
      <td>1.848601e+00</td>
      <td>3.433987</td>
      <td>6.650002e+00</td>
      <td>8.114747e+00</td>
      <td>9.449145e+00</td>
      <td>1.095396e+01</td>
    </tr>
    <tr>
      <th>log_disaster_deaths</th>
      <td>53.0</td>
      <td>7.679772e+00</td>
      <td>2.771148e+00</td>
      <td>1.098612</td>
      <td>5.793014e+00</td>
      <td>7.439559e+00</td>
      <td>9.904087e+00</td>
      <td>1.189582e+01</td>
    </tr>
    <tr>
      <th>log_people_affected</th>
      <td>54.0</td>
      <td>1.757108e+01</td>
      <td>2.074297e+00</td>
      <td>11.907437</td>
      <td>1.601588e+01</td>
      <td>1.783789e+01</td>
      <td>1.871104e+01</td>
      <td>2.152938e+01</td>
    </tr>
    <tr>
      <th>log_gov_health</th>
      <td>54.0</td>
      <td>3.229149e+00</td>
      <td>1.421226e+00</td>
      <td>1.144377</td>
      <td>2.075562e+00</td>
      <td>3.009389e+00</td>
      <td>4.120091e+00</td>
      <td>6.288715e+00</td>
    </tr>
    <tr>
      <th>log_private_health</th>
      <td>54.0</td>
      <td>3.499326e+00</td>
      <td>9.988960e-01</td>
      <td>1.751088</td>
      <td>2.745089e+00</td>
      <td>3.509264e+00</td>
      <td>4.321448e+00</td>
      <td>5.801926e+00</td>
    </tr>
  </tbody>
</table>
</div>



# Exploratory Data Analysis

The aim of this section is to understand the distribution of the variables, identify outliers and describe the current state of disaster preparedness and resilience across African countries.

This section addresses Research Question 1:

"Where are African countries currently positioned in terms of disaster preparedness and resilience?"


```python
import matplotlib.pyplot as plt

def plot_histogram(column, bins=15):

    plt.figure(figsize=(8,5))

    plt.hist(df_clean[column],
             bins=bins,
             edgecolor="black")

    plt.title(column)

    plt.xlabel(column)

    plt.ylabel("Frequency")

    plt.grid(alpha=0.3)

    plt.show()
```


```python
plot_histogram("log_GDP")

plot_histogram("log_population")

plot_histogram("WGI_SCORE")

plot_histogram("GHSI")

plot_histogram("INFORM_RISK_SCORE")

plot_histogram("log_gov_health")

plot_histogram("log_private_health")

plot_histogram("GINI")

plot_histogram("log_disaster_count")

plot_histogram("log_disaster_deaths")

plot_histogram("log_people_affected")
```


    
![png](output_22_0.png)
    



    
![png](output_22_1.png)
    



    
![png](output_22_2.png)
    



    
![png](output_22_3.png)
    



    
![png](output_22_4.png)
    



    
![png](output_22_5.png)
    



    
![png](output_22_6.png)
    



    
![png](output_22_7.png)
    



    
![png](output_22_8.png)
    



    
![png](output_22_9.png)
    



    
![png](output_22_10.png)
    



```python
def plot_boxplot(column):

    plt.figure(figsize=(8,2))

    plt.boxplot(df_clean[column].dropna(),
                vert=False)

    plt.title(column)

    plt.show()
```


```python
plot_boxplot("log_GDP")

plot_boxplot("WGI_SCORE")

plot_boxplot("GHSI")

plot_boxplot("INFORM_RISK_SCORE")

plot_boxplot("GINI")

plot_boxplot("log_disaster_deaths")
```


    
![png](output_24_0.png)
    



    
![png](output_24_1.png)
    



    
![png](output_24_2.png)
    



    
![png](output_24_3.png)
    



    
![png](output_24_4.png)
    



    
![png](output_24_5.png)
    


## COUNTRY RANKINGS, NEED TO REBRAND THIS NAME LATER

MOST PREPARED


```python
prepared = (
    df_clean
    .sort_values(by="GHSI", ascending=False)
    [["COUNTRY_NAME", "GHSI"]]
    .reset_index(drop=True)
)

prepared.index = prepared.index + 1

prepared.head(10)
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
      <th>COUNTRY_NAME</th>
      <th>GHSI</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>South Africa</td>
      <td>45.8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mauritius</td>
      <td>39.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kenya</td>
      <td>38.8</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nigeria</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Ethiopia</td>
      <td>37.8</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Uganda</td>
      <td>36.5</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Liberia</td>
      <td>35.7</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Ghana</td>
      <td>34.3</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Cabo Verde</td>
      <td>34.1</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Morocco</td>
      <td>33.6</td>
    </tr>
  </tbody>
</table>
</div>




```python
prepared = (
    df_clean
    .sort_values(by="GHSI", ascending=False)
    [["COUNTRY_NAME", "GHSI"]]
    .reset_index(drop=True)
)

prepared.index = prepared.index 

prepared.tail(10)
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
      <th>COUNTRY_NAME</th>
      <th>GHSI</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>44</th>
      <td>Comoros</td>
      <td>24.9</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Chad</td>
      <td>23.9</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Burundi</td>
      <td>22.1</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Gabon</td>
      <td>21.8</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Eritrea</td>
      <td>21.4</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Guinea-Bissau</td>
      <td>21.4</td>
    </tr>
    <tr>
      <th>50</th>
      <td>South Sudan</td>
      <td>21.3</td>
    </tr>
    <tr>
      <th>51</th>
      <td>Central African Republic</td>
      <td>18.6</td>
    </tr>
    <tr>
      <th>52</th>
      <td>Equatorial Guinea</td>
      <td>17.4</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Somalia, Fed. Rep.</td>
      <td>16.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
import matplotlib.pyplot as plt

def histogram(variable, title):

    plt.figure(figsize=(8,5))

    plt.hist(df_clean[variable].dropna(),
             bins=12,
             edgecolor="black")

    plt.title(title, fontsize=14)

    plt.xlabel(variable)

    plt.ylabel("Number of Countries")

    plt.grid(alpha=0.3)

    plt.show()
```


```python
histogram("GHSI","Global Health Security Index")

histogram("INFORM_RISK_SCORE","INFORM Risk Score")

histogram("WGI_SCORE","Government Effectiveness")

histogram("log_GDP","Log GDP per Capita")

histogram("log_population","Log Population")

histogram("GINI","Gini Index")

histogram("log_gov_health","Log Government Health Expenditure")

histogram("log_private_health","Log Private Health Expenditure")

histogram("log_disaster_deaths","Log Disaster Deaths")
```


    
![png](output_30_0.png)
    



    
![png](output_30_1.png)
    



    
![png](output_30_2.png)
    



    
![png](output_30_3.png)
    



    
![png](output_30_4.png)
    



    
![png](output_30_5.png)
    



    
![png](output_30_6.png)
    



    
![png](output_30_7.png)
    



    
![png](output_30_8.png)
    



```python
def boxplot(variable, title):

    plt.figure(figsize=(8,2))

    plt.boxplot(df_clean[variable].dropna(),
                vert=False)

    plt.title(title)

    plt.show()
```


```python
boxplot("GHSI","Global Health Security Index")

boxplot("INFORM_RISK_SCORE","INFORM Risk Score")

boxplot("log_GDP","Log GDP")

boxplot("GINI","Gini Index")

boxplot("log_disaster_deaths","Log Disaster Deaths")
```


    
![png](output_32_0.png)
    



    
![png](output_32_1.png)
    



    
![png](output_32_2.png)
    



    
![png](output_32_3.png)
    



    
![png](output_32_4.png)
    


## SIERRA LEONE


```python
df_clean[df_clean["COUNTRY_NAME"]=="Sierra Leone"]
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
      <th>ISO3</th>
      <th>REGION</th>
      <th>COUNTRY_NAME</th>
      <th>GDP_PER_CAPITA</th>
      <th>TOTAL_POPULATION</th>
      <th>WGI_SCORE</th>
      <th>GHSI</th>
      <th>INFORM_RISK_SCORE</th>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <th>...</th>
      <th>DISASTER COUNT</th>
      <th>DEATHS FROM DISASTER COUNT</th>
      <th>TOTAL PEOPLE AFFECTED BY DISASTER</th>
      <th>log_GDP</th>
      <th>log_population</th>
      <th>log_disaster_count</th>
      <th>log_disaster_deaths</th>
      <th>log_people_affected</th>
      <th>log_gov_health</th>
      <th>log_private_health</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>40</th>
      <td>SLE</td>
      <td>Africa</td>
      <td>Sierra Leone</td>
      <td>846.296172</td>
      <td>8819794</td>
      <td>32.536134</td>
      <td>32.7</td>
      <td>5.1</td>
      <td>5.978927</td>
      <td>19.589493</td>
      <td>...</td>
      <td>5011</td>
      <td>327.0</td>
      <td>28648583</td>
      <td>6.740869</td>
      <td>15.992509</td>
      <td>8.51959</td>
      <td>5.793014</td>
      <td>17.170615</td>
      <td>1.788241</td>
      <td>2.974993</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 21 columns</p>
</div>



african averages


```python
df_clean.mean(numeric_only=True).round(2)
```




    GDP_PER_CAPITA                       3.101760e+03
    TOTAL_POPULATION                     2.864588e+07
    WGI_SCORE                            3.819000e+01
    GHSI                                 2.911000e+01
    INFORM_RISK_SCORE                    5.380000e+00
    GOV_HEALTH_EXPENDITURE               7.096000e+01
    PRIVATE_HEALTH_EXPENDITURE           5.429000e+01
    GINI                                 3.844000e+01
    DISASTER COUNT                       9.196500e+03
    DEATHS FROM DISASTER COUNT           2.097147e+04
    TOTAL PEOPLE AFFECTED BY DISASTER    2.243375e+08
    log_GDP                              7.610000e+00
    log_population                       1.622000e+01
    log_disaster_count                   7.930000e+00
    log_disaster_deaths                  7.680000e+00
    log_people_affected                  1.757000e+01
    log_gov_health                       3.230000e+00
    log_private_health                   3.500000e+00
    dtype: float64



get out of log form in analysis book

salone vs africa 


```python
sl = df_clean[df_clean["COUNTRY_NAME"]=="Sierra Leone"]

comparison = pd.DataFrame({
    "Sierra Leone": sl.iloc[0],
    "Africa Average": df_clean.mean(numeric_only=True)
})

comparison
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
      <th>Sierra Leone</th>
      <th>Africa Average</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>COUNTRY_NAME</th>
      <td>Sierra Leone</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>DEATHS FROM DISASTER COUNT</th>
      <td>327.0</td>
      <td>2.097147e+04</td>
    </tr>
    <tr>
      <th>DISASTER COUNT</th>
      <td>5011</td>
      <td>9.196500e+03</td>
    </tr>
    <tr>
      <th>GDP_PER_CAPITA</th>
      <td>846.296172</td>
      <td>3.101761e+03</td>
    </tr>
    <tr>
      <th>GHSI</th>
      <td>32.7</td>
      <td>2.910926e+01</td>
    </tr>
    <tr>
      <th>GINI</th>
      <td>35.7</td>
      <td>3.844000e+01</td>
    </tr>
    <tr>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <td>5.978927</td>
      <td>7.095570e+01</td>
    </tr>
    <tr>
      <th>INFORM_RISK_SCORE</th>
      <td>5.1</td>
      <td>5.375926e+00</td>
    </tr>
    <tr>
      <th>ISO3</th>
      <td>SLE</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <td>19.589493</td>
      <td>5.428884e+01</td>
    </tr>
    <tr>
      <th>REGION</th>
      <td>Africa</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>TOTAL PEOPLE AFFECTED BY DISASTER</th>
      <td>28648583</td>
      <td>2.243375e+08</td>
    </tr>
    <tr>
      <th>TOTAL_POPULATION</th>
      <td>8819794</td>
      <td>2.864588e+07</td>
    </tr>
    <tr>
      <th>WGI_SCORE</th>
      <td>32.536134</td>
      <td>3.818565e+01</td>
    </tr>
    <tr>
      <th>log_GDP</th>
      <td>6.740869</td>
      <td>7.610631e+00</td>
    </tr>
    <tr>
      <th>log_disaster_count</th>
      <td>8.51959</td>
      <td>7.928443e+00</td>
    </tr>
    <tr>
      <th>log_disaster_deaths</th>
      <td>5.793014</td>
      <td>7.679772e+00</td>
    </tr>
    <tr>
      <th>log_gov_health</th>
      <td>1.788241</td>
      <td>3.229149e+00</td>
    </tr>
    <tr>
      <th>log_people_affected</th>
      <td>17.170615</td>
      <td>1.757108e+01</td>
    </tr>
    <tr>
      <th>log_population</th>
      <td>15.992509</td>
      <td>1.621811e+01</td>
    </tr>
    <tr>
      <th>log_private_health</th>
      <td>2.974993</td>
      <td>3.499326e+00</td>
    </tr>
  </tbody>
</table>
</div>




```python
import matplotlib.pyplot as plt
```

correlations


```python
corr = df_clean[[
    "log_GDP",
    "log_population",
    "WGI_SCORE",
    "GHSI",
    "INFORM_RISK_SCORE",
    "log_gov_health",
    "log_private_health",
    "GINI",
    "log_disaster_count",
    "log_disaster_deaths",
    "log_people_affected"
]].corr()

corr.round(2)
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
      <th>log_GDP</th>
      <th>log_population</th>
      <th>WGI_SCORE</th>
      <th>GHSI</th>
      <th>INFORM_RISK_SCORE</th>
      <th>log_gov_health</th>
      <th>log_private_health</th>
      <th>GINI</th>
      <th>log_disaster_count</th>
      <th>log_disaster_deaths</th>
      <th>log_people_affected</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>log_GDP</th>
      <td>1.00</td>
      <td>-0.44</td>
      <td>0.56</td>
      <td>0.23</td>
      <td>-0.72</td>
      <td>0.90</td>
      <td>0.82</td>
      <td>-0.01</td>
      <td>-0.40</td>
      <td>-0.53</td>
      <td>-0.44</td>
    </tr>
    <tr>
      <th>log_population</th>
      <td>-0.44</td>
      <td>1.00</td>
      <td>-0.16</td>
      <td>0.25</td>
      <td>0.68</td>
      <td>-0.41</td>
      <td>-0.37</td>
      <td>0.25</td>
      <td>0.84</td>
      <td>0.78</td>
      <td>0.88</td>
    </tr>
    <tr>
      <th>WGI_SCORE</th>
      <td>0.56</td>
      <td>-0.16</td>
      <td>1.00</td>
      <td>0.59</td>
      <td>-0.65</td>
      <td>0.64</td>
      <td>0.40</td>
      <td>-0.09</td>
      <td>-0.34</td>
      <td>-0.48</td>
      <td>-0.37</td>
    </tr>
    <tr>
      <th>GHSI</th>
      <td>0.23</td>
      <td>0.25</td>
      <td>0.59</td>
      <td>1.00</td>
      <td>-0.20</td>
      <td>0.33</td>
      <td>0.24</td>
      <td>0.19</td>
      <td>0.15</td>
      <td>-0.06</td>
      <td>0.09</td>
    </tr>
    <tr>
      <th>INFORM_RISK_SCORE</th>
      <td>-0.72</td>
      <td>0.68</td>
      <td>-0.65</td>
      <td>-0.20</td>
      <td>1.00</td>
      <td>-0.71</td>
      <td>-0.60</td>
      <td>0.24</td>
      <td>0.73</td>
      <td>0.84</td>
      <td>0.76</td>
    </tr>
    <tr>
      <th>log_gov_health</th>
      <td>0.90</td>
      <td>-0.41</td>
      <td>0.64</td>
      <td>0.33</td>
      <td>-0.71</td>
      <td>1.00</td>
      <td>0.76</td>
      <td>0.12</td>
      <td>-0.36</td>
      <td>-0.53</td>
      <td>-0.44</td>
    </tr>
    <tr>
      <th>log_private_health</th>
      <td>0.82</td>
      <td>-0.37</td>
      <td>0.40</td>
      <td>0.24</td>
      <td>-0.60</td>
      <td>0.76</td>
      <td>1.00</td>
      <td>-0.24</td>
      <td>-0.24</td>
      <td>-0.41</td>
      <td>-0.31</td>
    </tr>
    <tr>
      <th>GINI</th>
      <td>-0.01</td>
      <td>0.25</td>
      <td>-0.09</td>
      <td>0.19</td>
      <td>0.24</td>
      <td>0.12</td>
      <td>-0.24</td>
      <td>1.00</td>
      <td>0.21</td>
      <td>0.19</td>
      <td>0.15</td>
    </tr>
    <tr>
      <th>log_disaster_count</th>
      <td>-0.40</td>
      <td>0.84</td>
      <td>-0.34</td>
      <td>0.15</td>
      <td>0.73</td>
      <td>-0.36</td>
      <td>-0.24</td>
      <td>0.21</td>
      <td>1.00</td>
      <td>0.80</td>
      <td>0.95</td>
    </tr>
    <tr>
      <th>log_disaster_deaths</th>
      <td>-0.53</td>
      <td>0.78</td>
      <td>-0.48</td>
      <td>-0.06</td>
      <td>0.84</td>
      <td>-0.53</td>
      <td>-0.41</td>
      <td>0.19</td>
      <td>0.80</td>
      <td>1.00</td>
      <td>0.83</td>
    </tr>
    <tr>
      <th>log_people_affected</th>
      <td>-0.44</td>
      <td>0.88</td>
      <td>-0.37</td>
      <td>0.09</td>
      <td>0.76</td>
      <td>-0.44</td>
      <td>-0.31</td>
      <td>0.15</td>
      <td>0.95</td>
      <td>0.83</td>
      <td>1.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
pip install seaborn
```

    Requirement already satisfied: seaborn in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (0.13.2)
    Requirement already satisfied: numpy!=1.24.0,>=1.20 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from seaborn) (2.0.2)
    Requirement already satisfied: pandas>=1.2 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from seaborn) (2.3.0)
    Requirement already satisfied: matplotlib!=3.6.1,>=3.4 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from seaborn) (3.10.3)
    Requirement already satisfied: contourpy>=1.0.1 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (1.3.2)
    Requirement already satisfied: cycler>=0.10 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (0.12.1)
    Requirement already satisfied: fonttools>=4.22.0 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (4.58.2)
    Requirement already satisfied: kiwisolver>=1.3.1 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (1.4.8)
    Requirement already satisfied: packaging>=20.0 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (24.1)
    Requirement already satisfied: pillow>=8 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (11.2.1)
    Requirement already satisfied: pyparsing>=2.3.1 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (3.2.3)
    Requirement already satisfied: python-dateutil>=2.7 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (2.9.0.post0)
    Requirement already satisfied: pytz>=2020.1 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from pandas>=1.2->seaborn) (2025.2)
    Requirement already satisfied: tzdata>=2022.7 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from pandas>=1.2->seaborn) (2025.2)
    Requirement already satisfied: six>=1.5 in c:\users\oj\appdata\local\programs\python\python312\lib\site-packages (from python-dateutil>=2.7->matplotlib!=3.6.1,>=3.4->seaborn) (1.16.0)
    Note: you may need to restart the kernel to use updated packages.
    

    
    [notice] A new release of pip is available: 24.0 -> 26.1.2
    [notice] To update, run: python.exe -m pip install --upgrade pip
    


```python
import seaborn as sns
import matplotlib.pyplot as plt
```


```python
corr = df_clean[[
    "log_GDP",
    "log_population",
    "WGI_SCORE",
    "GHSI",
    "INFORM_RISK_SCORE",
    "log_gov_health",
    "log_private_health",
    "GINI",
    "log_disaster_count",
    "log_disaster_deaths",
    "log_people_affected"
]].corr()

plt.figure(figsize=(12,10))

sns.heatmap(
    corr,
    annot=True,          # Show correlation values
    fmt=".2f",           # Two decimal places
    cmap="coolwarm",
    vmin=-1,
    vmax=1,
    linewidths=0.5,
    square=True,
    cbar_kws={"label": "Correlation"}
)

plt.title("Correlation Matrix of Disaster Preparedness Variables", fontsize=16)
plt.xticks(rotation=45, ha="right")
plt.yticks(rotation=0)

plt.tight_layout()

plt.show()
```


    
![png](output_45_0.png)
    


## ## Correlation Analysis

A correlation analysis was conducted to provide an initial assessment of the relationships between the study variables before fitting regression models. Correlation coefficients measure the strength and direction of the linear association between two variables, ranging from -1 (perfect negative relationship) to +1 (perfect positive relationship).

The purpose of this analysis was not to establish causation but to identify preliminary patterns within the data, assess whether the observed relationships aligned with theoretical expectations, and identify potential multicollinearity among the predictor variables. Detecting strong correlations between independent variables is important because highly correlated predictors can reduce the stability and interpretability of multiple regression models.

The results indicated several expected relationships, including positive associations between economic capacity and health expenditure, and between population size and disaster outcomes. Government effectiveness demonstrated one of the strongest positive associations with preparedness, while INFORM Risk showed strong negative associations with economic capacity and governance indicators. These findings provided an initial understanding of the data structure and informed the specification of the subsequent regression models.

Following this exploratory analysis, multiple linear regression was used to examine which socioeconomic, governance and health system factors were independently associated with disaster preparedness after controlling for the influence of the other variables.


# Simple Linear Regression

The correlation analysis identified several potential relationships between preparedness and the socioeconomic, governance and health system variables. However, correlations do not account for uncertainty or quantify the statistical relationship between variables.

Simple linear regression was therefore used to examine the association between each predictor and disaster preparedness individually. These models provide an initial assessment of the direction, magnitude and statistical significance of each relationship before constructing a multivariable model.


```python
import statsmodels.formula.api as smf
```


```python
def simple_regression(outcome, predictor, data):

    model = smf.ols(
        formula=f"{outcome} ~ {predictor}",
        data=data
    ).fit()

    print("=" * 70)
    print(f"{outcome} ~ {predictor}")
    print("=" * 70)

    print(model.summary())
```


```python
simple_regression("GHSI", "log_GDP", df_clean)

simple_regression("GHSI", "WGI_SCORE", df_clean)

simple_regression("GHSI", "log_gov_health", df_clean)

simple_regression("GHSI", "log_private_health", df_clean)

simple_regression("GHSI", "log_population", df_clean)
```

    ======================================================================
    GHSI ~ log_GDP
    ======================================================================
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   GHSI   R-squared:                       0.051
    Model:                            OLS   Adj. R-squared:                  0.032
    Method:                 Least Squares   F-statistic:                     2.673
    Date:                Wed, 08 Jul 2026   Prob (F-statistic):              0.108
    Time:                        13:32:37   Log-Likelihood:                -162.09
    No. Observations:                  52   AIC:                             328.2
    Df Residuals:                      50   BIC:                             332.1
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Intercept     18.8305      6.515      2.890      0.006       5.744      31.917
    log_GDP        1.3898      0.850      1.635      0.108      -0.318       3.097
    ==============================================================================
    Omnibus:                        1.053   Durbin-Watson:                   2.020
    Prob(Omnibus):                  0.591   Jarque-Bera (JB):                0.382
    Skew:                           0.036   Prob(JB):                        0.826
    Kurtosis:                       3.414   Cond. No.                         65.7
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    ======================================================================
    GHSI ~ WGI_SCORE
    ======================================================================
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   GHSI   R-squared:                       0.351
    Model:                            OLS   Adj. R-squared:                  0.339
    Method:                 Least Squares   F-statistic:                     28.16
    Date:                Wed, 08 Jul 2026   Prob (F-statistic):           2.33e-06
    Time:                        13:32:37   Log-Likelihood:                -159.01
    No. Observations:                  54   AIC:                             322.0
    Df Residuals:                      52   BIC:                             326.0
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Intercept     18.8061      2.043      9.203      0.000      14.706      22.907
    WGI_SCORE      0.2698      0.051      5.307      0.000       0.168       0.372
    ==============================================================================
    Omnibus:                        7.914   Durbin-Watson:                   1.998
    Prob(Omnibus):                  0.019   Jarque-Bera (JB):                7.053
    Skew:                           0.768   Prob(JB):                       0.0294
    Kurtosis:                       3.882   Cond. No.                         129.
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    ======================================================================
    GHSI ~ log_gov_health
    ======================================================================
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   GHSI   R-squared:                       0.108
    Model:                            OLS   Adj. R-squared:                  0.091
    Method:                 Least Squares   F-statistic:                     6.284
    Date:                Wed, 08 Jul 2026   Prob (F-statistic):             0.0153
    Time:                        13:32:37   Log-Likelihood:                -167.62
    No. Observations:                  54   AIC:                             339.2
    Df Residuals:                      52   BIC:                             343.2
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==================================================================================
                         coef    std err          t      P>|t|      [0.025      0.975]
    ----------------------------------------------------------------------------------
    Intercept         24.8094      1.871     13.259      0.000      21.055      28.564
    log_gov_health     1.3316      0.531      2.507      0.015       0.266       2.397
    ==============================================================================
    Omnibus:                        0.360   Durbin-Watson:                   2.021
    Prob(Omnibus):                  0.835   Jarque-Bera (JB):                0.074
    Skew:                           0.082   Prob(JB):                        0.964
    Kurtosis:                       3.075   Cond. No.                         9.42
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    ======================================================================
    GHSI ~ log_private_health
    ======================================================================
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   GHSI   R-squared:                       0.055
    Model:                            OLS   Adj. R-squared:                  0.037
    Method:                 Least Squares   F-statistic:                     3.043
    Date:                Wed, 08 Jul 2026   Prob (F-statistic):             0.0870
    Time:                        13:32:37   Log-Likelihood:                -169.16
    No. Observations:                  54   AIC:                             342.3
    Df Residuals:                      52   BIC:                             346.3
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ======================================================================================
                             coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------------
    Intercept             24.3618      2.828      8.614      0.000      18.687      30.037
    log_private_health     1.3567      0.778      1.745      0.087      -0.204       2.917
    ==============================================================================
    Omnibus:                        0.278   Durbin-Watson:                   1.944
    Prob(Omnibus):                  0.870   Jarque-Bera (JB):                0.018
    Skew:                          -0.030   Prob(JB):                        0.991
    Kurtosis:                       3.065   Cond. No.                         14.3
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    ======================================================================
    GHSI ~ log_population
    ======================================================================
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   GHSI   R-squared:                       0.064
    Model:                            OLS   Adj. R-squared:                  0.046
    Method:                 Least Squares   F-statistic:                     3.548
    Date:                Wed, 08 Jul 2026   Prob (F-statistic):             0.0652
    Time:                        13:32:37   Log-Likelihood:                -168.92
    No. Observations:                  54   AIC:                             341.8
    Df Residuals:                      52   BIC:                             345.8
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==================================================================================
                         coef    std err          t      P>|t|      [0.025      0.975]
    ----------------------------------------------------------------------------------
    Intercept         14.6105      7.735      1.889      0.065      -0.912      30.133
    log_population     0.8940      0.475      1.884      0.065      -0.058       1.846
    ==============================================================================
    Omnibus:                        1.046   Durbin-Watson:                   2.087
    Prob(Omnibus):                  0.593   Jarque-Bera (JB):                0.447
    Skew:                           0.169   Prob(JB):                        0.800
    Kurtosis:                       3.291   Cond. No.                         165.
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    

seperate gini as it has smaller sample


```python
gini_df = df_clean.dropna(subset=["GINI"])

simple_regression("GHSI", "GINI", gini_df)
```

    ======================================================================
    GHSI ~ GINI
    ======================================================================
                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   GHSI   R-squared:                       0.037
    Model:                            OLS   Adj. R-squared:                  0.008
    Method:                 Least Squares   F-statistic:                     1.263
    Date:                Wed, 08 Jul 2026   Prob (F-statistic):              0.269
    Time:                        13:34:53   Log-Likelihood:                -109.41
    No. Observations:                  35   AIC:                             222.8
    Df Residuals:                      33   BIC:                             225.9
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Intercept     23.0978      5.764      4.007      0.000      11.370      34.825
    GINI           0.1662      0.148      1.124      0.269      -0.135       0.467
    ==============================================================================
    Omnibus:                        1.068   Durbin-Watson:                   1.812
    Prob(Omnibus):                  0.586   Jarque-Bera (JB):                0.323
    Skew:                           0.156   Prob(JB):                        0.851
    Kurtosis:                       3.353   Cond. No.                         234.
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    

| Predictor          |        R² |    p-value | Interpretation                                                        |
| ------------------ | --------: | ---------: | --------------------------------------------------------------------- |
| **WGI_SCORE**      | **0.351** | **<0.001** | Strong evidence of a positive association with preparedness.          |
| **log_gov_health** |     0.108 |  **0.015** | Public health expenditure is positively associated with preparedness. |
| log_population     |     0.064 |      0.065 | Weak evidence of a positive association.                              |
| log_private_health |     0.055 |      0.087 | Weak evidence.                                                        |
| log_GDP            |     0.051 |      0.108 | No statistically significant association.                             |
| GINI               |     0.037 |      0.269 | No evidence of an association.                                        |


## Simple Linear Regression

Simple linear regression models were fitted to examine the association between each explanatory variable and the Global Health Security Index (GHSI) individually. These analyses provide an initial assessment of the strength, direction and statistical significance of each relationship before considering all predictors simultaneously.

The results indicated that Government Effectiveness was the strongest individual predictor of preparedness, explaining approximately 35% of the variation in GHSI scores across African countries. Government health expenditure also demonstrated a statistically significant positive association with preparedness, although its explanatory power was considerably lower. In contrast, GDP per capita, private health expenditure, population size and income inequality showed relatively weak individual associations with GHSI.

These findings suggest that governance may play a more important role in disaster preparedness than economic capacity alone. However, because many explanatory variables are themselves correlated, these individual models cannot determine whether each predictor has an independent association with preparedness. A multiple linear regression model was therefore fitted to estimate the effect of each variable while controlling for the influence of the others.



```python
main_model = smf.ols(
    """
    GHSI ~
    log_GDP +
    WGI_SCORE +
    log_gov_health +
    log_private_health +
    log_population +
    GINI
    """,
    data=gini_df
).fit()

print(main_model.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   GHSI   R-squared:                       0.546
    Model:                            OLS   Adj. R-squared:                  0.449
    Method:                 Least Squares   F-statistic:                     5.612
    Date:                Wed, 08 Jul 2026   Prob (F-statistic):           0.000632
    Time:                        13:38:43   Log-Likelihood:                -96.247
    No. Observations:                  35   AIC:                             206.5
    Df Residuals:                      28   BIC:                             217.4
    Df Model:                           6                                         
    Covariance Type:            nonrobust                                         
    ======================================================================================
                             coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------------
    Intercept            -24.3018     15.688     -1.549      0.133     -56.437       7.833
    log_GDP                0.6552      2.246      0.292      0.773      -3.946       5.257
    WGI_SCORE              0.2514      0.108      2.334      0.027       0.031       0.472
    log_gov_health         0.4073      1.632      0.250      0.805      -2.935       3.750
    log_private_health     0.2851      1.704      0.167      0.868      -3.205       3.775
    log_population         2.0371      0.544      3.744      0.001       0.922       3.152
    GINI                   0.0868      0.148      0.587      0.562      -0.216       0.390
    ==============================================================================
    Omnibus:                        0.055   Durbin-Watson:                   2.433
    Prob(Omnibus):                  0.973   Jarque-Bera (JB):                0.245
    Skew:                          -0.058   Prob(JB):                        0.884
    Kurtosis:                       2.606   Cond. No.                     1.28e+03
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 1.28e+03. This might indicate that there are
    strong multicollinearity or other numerical problems.
    


```python
from statsmodels.stats.outliers_influence import variance_inflation_factor
```


```python
X = gini_df[
    [
        "log_GDP",
        "WGI_SCORE",
        "log_gov_health",
        "log_private_health",
        "log_population",
        "GINI"
    ]
]
```


```python
import statsmodels.api as sm

X = sm.add_constant(X)
```

## Variance Inflation Factor  check
Why VIF?

my regression already hint at multicollinearity.

VIF tells us:

VIF < 5 → generally acceptable.
5–10 → moderate multicollinearity.
>10 → high multicollinearity.

If GDP and government health expenditure both have very high VIFs, that would explain why neither is statistically significant 
when they're included together.

Why this could become one of the main conclusions
I may end up with a result like:

Wealth alone does not independently explain preparedness once governance is taken into account. Instead, government effectiveness 
emerges as the most robust predictor of preparedness across African countries.

That's a much more interesting policy finding than "rich countries are more prepared.
I will run the VIF analysis next, and decide whether to keep all predictors, remove some, or consider an alternative model specification.


```python
vif = pd.DataFrame()

vif["Variable"] = X.columns

vif["VIF"] = [
    variance_inflation_factor(X.values, i)
    for i in range(X.shape[1])
]

vif
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
      <th>Variable</th>
      <th>VIF</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>const</td>
      <td>481.086152</td>
    </tr>
    <tr>
      <th>1</th>
      <td>log_GDP</td>
      <td>6.705828</td>
    </tr>
    <tr>
      <th>2</th>
      <td>WGI_SCORE</td>
      <td>2.231067</td>
    </tr>
    <tr>
      <th>3</th>
      <td>log_gov_health</td>
      <td>6.799881</td>
    </tr>
    <tr>
      <th>4</th>
      <td>log_private_health</td>
      <td>5.178736</td>
    </tr>
    <tr>
      <th>5</th>
      <td>log_population</td>
      <td>1.274027</td>
    </tr>
    <tr>
      <th>6</th>
      <td>GINI</td>
      <td>1.799060</td>
    </tr>
  </tbody>
</table>
</div>



if you have more money, you spend on health, makes sense

### Multicollinearity Assessment

Variance Inflation Factors (VIFs) were calculated to assess multicollinearity among the explanatory variables prior to interpreting the multiple regression model. Most predictors exhibited low to moderate VIF values, indicating acceptable levels of collinearity. GDP per capita, government health expenditure and private health expenditure demonstrated the highest VIF values, reflecting the expected relationship between national wealth and health investment. However, no predictor exceeded commonly used thresholds indicative of severe multicollinearity (VIF > 10). Consequently, all variables were retained in the final model because each represented a distinct theoretical construct relevant to disaster preparedness.


lets make deaths fairer e.g deaths_per_100k, then perhaps log transform them because absolute measures are harsh on large populations 


```python
df_clean["DEATHS_PER_100K"] = (
    df_clean["DEATHS FROM DISASTER COUNT"] /
    df_clean["TOTAL_POPULATION"]
) * 100000

df_clean["AFFECTED_PER_100K"] = (
    df_clean["TOTAL PEOPLE AFFECTED BY DISASTER"] /
    df_clean["TOTAL_POPULATION"]
) * 100000
```


```python
df_clean["log_deaths_per_100k"] = np.log1p(df_clean["DEATHS_PER_100K"])

df_clean["log_affected_per_100k"] = np.log1p(df_clean["AFFECTED_PER_100K"])
```


```python
df_clean[[
    "COUNTRY_NAME",
    "DEATHS_PER_100K",
    "AFFECTED_PER_100K",
    "log_deaths_per_100k",
    "log_affected_per_100k"
]].head()
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
      <th>COUNTRY_NAME</th>
      <th>DEATHS_PER_100K</th>
      <th>AFFECTED_PER_100K</th>
      <th>log_deaths_per_100k</th>
      <th>log_affected_per_100k</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Angola</td>
      <td>369.582110</td>
      <td>347037.924322</td>
      <td>5.915075</td>
      <td>12.757192</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Burundi</td>
      <td>171.473210</td>
      <td>892648.159976</td>
      <td>5.150242</td>
      <td>13.701949</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Benin</td>
      <td>9.618980</td>
      <td>238045.585192</td>
      <td>2.362643</td>
      <td>12.380222</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Burkina Faso</td>
      <td>146.104314</td>
      <td>492145.603371</td>
      <td>4.991142</td>
      <td>13.106532</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Botswana</td>
      <td>1.366055</td>
      <td>34064.615190</td>
      <td>0.861224</td>
      <td>10.436044</td>
    </tr>
  </tbody>
</table>
</div>



## Standardising Disaster Outcomes

The previous analyses used the total number of disaster deaths and the total number of people affected. However, these absolute figures are heavily influenced by population size. Larger countries will generally experience more deaths and more people affected simply because they have larger populations, making direct comparisons between countries less meaningful.

To allow fair comparisons across African countries, disaster outcomes were standardised as rates per 100,000 population. This provides a better measure of the relative humanitarian impact experienced by each country's population rather than the total number of people affected.

The subsequent regression analyses therefore focus on the following outcomes:

* **Global Health Security Index (GHSI):** to identify the socioeconomic, governance and health system factors associated with disaster preparedness.
* **Disaster deaths per 100,000 population:** to examine which factors are associated with lower disaster mortality.
* **People affected per 100,000 population:** to examine which factors are associated with reducing the overall humanitarian impact of disasters.

Together, these models assess not only what is associated with preparedness, but also whether preparedness and broader national characteristics are associated with improved humanitarian outcomes.



```python
df_clean["DEATHS_PER_100K"] = (
    df_clean["DEATHS FROM DISASTER COUNT"] /
    df_clean["TOTAL_POPULATION"]
) * 100000

df_clean["AFFECTED_PER_100K"] = (
    df_clean["TOTAL PEOPLE AFFECTED BY DISASTER"] /
    df_clean["TOTAL_POPULATION"]
) * 100000
```


```python
df_clean["log_DEATHS_PER_100K"] = np.log1p(df_clean["DEATHS_PER_100K"])

df_clean["log_AFFECTED_PER_100K"] = np.log1p(df_clean["AFFECTED_PER_100K"])
```


```python
df_clean[[
    "COUNTRY_NAME",
    "DEATHS_PER_100K",
    "AFFECTED_PER_100K",
    "log_DEATHS_PER_100K",
    "log_AFFECTED_PER_100K"
]].head()
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
      <th>COUNTRY_NAME</th>
      <th>DEATHS_PER_100K</th>
      <th>AFFECTED_PER_100K</th>
      <th>log_DEATHS_PER_100K</th>
      <th>log_AFFECTED_PER_100K</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Angola</td>
      <td>369.582110</td>
      <td>347037.924322</td>
      <td>5.915075</td>
      <td>12.757192</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Burundi</td>
      <td>171.473210</td>
      <td>892648.159976</td>
      <td>5.150242</td>
      <td>13.701949</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Benin</td>
      <td>9.618980</td>
      <td>238045.585192</td>
      <td>2.362643</td>
      <td>12.380222</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Burkina Faso</td>
      <td>146.104314</td>
      <td>492145.603371</td>
      <td>4.991142</td>
      <td>13.106532</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Botswana</td>
      <td>1.366055</td>
      <td>34064.615190</td>
      <td>0.861224</td>
      <td>10.436044</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_clean[[
    "DEATHS_PER_100K",
    "AFFECTED_PER_100K",
    "log_DEATHS_PER_100K",
    "log_AFFECTED_PER_100K"
]].describe().round(2)
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
      <th>DEATHS_PER_100K</th>
      <th>AFFECTED_PER_100K</th>
      <th>log_DEATHS_PER_100K</th>
      <th>log_AFFECTED_PER_100K</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>53.00</td>
      <td>54.00</td>
      <td>53.00</td>
      <td>54.00</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>94.86</td>
      <td>726232.26</td>
      <td>3.05</td>
      <td>12.87</td>
    </tr>
    <tr>
      <th>std</th>
      <td>231.06</td>
      <td>1473319.13</td>
      <td>1.69</td>
      <td>1.00</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.96</td>
      <td>34064.62</td>
      <td>0.68</td>
      <td>10.44</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>4.85</td>
      <td>220034.64</td>
      <td>1.77</td>
      <td>12.30</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>13.23</td>
      <td>381941.71</td>
      <td>2.66</td>
      <td>12.85</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>60.71</td>
      <td>615879.06</td>
      <td>4.12</td>
      <td>13.33</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1508.68</td>
      <td>10385665.66</td>
      <td>7.32</td>
      <td>16.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
histogram(
    "log_DEATHS_PER_100K",
    "Log Disaster Deaths per 100,000 Population"
)

histogram(
    "log_AFFECTED_PER_100K",
    "Log People Affected per 100,000 Population"
)
```


    
![png](output_72_0.png)
    



    
![png](output_72_1.png)
    



```python
corr = df_clean[[
    "log_GDP",
    "log_population",
    "WGI_SCORE",
    "GHSI",
    "INFORM_RISK_SCORE",
    "log_gov_health",
    "log_private_health",
    "GINI",
    "log_DEATHS_PER_100K",
    "log_AFFECTED_PER_100K"
]].corr()

corr.round(2)
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
      <th>log_GDP</th>
      <th>log_population</th>
      <th>WGI_SCORE</th>
      <th>GHSI</th>
      <th>INFORM_RISK_SCORE</th>
      <th>log_gov_health</th>
      <th>log_private_health</th>
      <th>GINI</th>
      <th>log_DEATHS_PER_100K</th>
      <th>log_AFFECTED_PER_100K</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>log_GDP</th>
      <td>1.00</td>
      <td>-0.44</td>
      <td>0.56</td>
      <td>0.23</td>
      <td>-0.72</td>
      <td>0.90</td>
      <td>0.82</td>
      <td>-0.01</td>
      <td>-0.47</td>
      <td>-0.19</td>
    </tr>
    <tr>
      <th>log_population</th>
      <td>-0.44</td>
      <td>1.00</td>
      <td>-0.16</td>
      <td>0.25</td>
      <td>0.68</td>
      <td>-0.41</td>
      <td>-0.37</td>
      <td>0.25</td>
      <td>0.32</td>
      <td>0.20</td>
    </tr>
    <tr>
      <th>WGI_SCORE</th>
      <td>0.56</td>
      <td>-0.16</td>
      <td>1.00</td>
      <td>0.59</td>
      <td>-0.65</td>
      <td>0.64</td>
      <td>0.40</td>
      <td>-0.09</td>
      <td>-0.64</td>
      <td>-0.51</td>
    </tr>
    <tr>
      <th>GHSI</th>
      <td>0.23</td>
      <td>0.25</td>
      <td>0.59</td>
      <td>1.00</td>
      <td>-0.20</td>
      <td>0.33</td>
      <td>0.24</td>
      <td>0.19</td>
      <td>-0.36</td>
      <td>-0.22</td>
    </tr>
    <tr>
      <th>INFORM_RISK_SCORE</th>
      <td>-0.72</td>
      <td>0.68</td>
      <td>-0.65</td>
      <td>-0.20</td>
      <td>1.00</td>
      <td>-0.71</td>
      <td>-0.60</td>
      <td>0.24</td>
      <td>0.70</td>
      <td>0.47</td>
    </tr>
    <tr>
      <th>log_gov_health</th>
      <td>0.90</td>
      <td>-0.41</td>
      <td>0.64</td>
      <td>0.33</td>
      <td>-0.71</td>
      <td>1.00</td>
      <td>0.76</td>
      <td>0.12</td>
      <td>-0.47</td>
      <td>-0.25</td>
    </tr>
    <tr>
      <th>log_private_health</th>
      <td>0.82</td>
      <td>-0.37</td>
      <td>0.40</td>
      <td>0.24</td>
      <td>-0.60</td>
      <td>0.76</td>
      <td>1.00</td>
      <td>-0.24</td>
      <td>-0.32</td>
      <td>-0.03</td>
    </tr>
    <tr>
      <th>GINI</th>
      <td>-0.01</td>
      <td>0.25</td>
      <td>-0.09</td>
      <td>0.19</td>
      <td>0.24</td>
      <td>0.12</td>
      <td>-0.24</td>
      <td>1.00</td>
      <td>0.08</td>
      <td>-0.13</td>
    </tr>
    <tr>
      <th>log_DEATHS_PER_100K</th>
      <td>-0.47</td>
      <td>0.32</td>
      <td>-0.64</td>
      <td>-0.36</td>
      <td>0.70</td>
      <td>-0.47</td>
      <td>-0.32</td>
      <td>0.08</td>
      <td>1.00</td>
      <td>0.53</td>
    </tr>
    <tr>
      <th>log_AFFECTED_PER_100K</th>
      <td>-0.19</td>
      <td>0.20</td>
      <td>-0.51</td>
      <td>-0.22</td>
      <td>0.47</td>
      <td>-0.25</td>
      <td>-0.03</td>
      <td>-0.13</td>
      <td>0.53</td>
      <td>1.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_clean.columns.tolist()
```




    ['ISO3',
     'REGION',
     'COUNTRY_NAME',
     'GDP_PER_CAPITA',
     'TOTAL_POPULATION',
     'WGI_SCORE',
     'GHSI',
     'INFORM_RISK_SCORE',
     'GOV_HEALTH_EXPENDITURE',
     'PRIVATE_HEALTH_EXPENDITURE',
     'GINI',
     'DISASTER COUNT',
     'DEATHS FROM DISASTER COUNT',
     'TOTAL PEOPLE AFFECTED BY DISASTER',
     'log_GDP',
     'log_population',
     'log_disaster_count',
     'log_disaster_deaths',
     'log_people_affected',
     'log_gov_health',
     'log_private_health',
     'DEATHS_PER_100K',
     'AFFECTED_PER_100K',
     'log_deaths_per_100k',
     'log_affected_per_100k',
     'log_DEATHS_PER_100K',
     'log_AFFECTED_PER_100K']




```python
gini_df = df_clean.dropna(subset=["GINI"])
```


```python
gini_df.columns.tolist()
```




    ['ISO3',
     'REGION',
     'COUNTRY_NAME',
     'GDP_PER_CAPITA',
     'TOTAL_POPULATION',
     'WGI_SCORE',
     'GHSI',
     'INFORM_RISK_SCORE',
     'GOV_HEALTH_EXPENDITURE',
     'PRIVATE_HEALTH_EXPENDITURE',
     'GINI',
     'DISASTER COUNT',
     'DEATHS FROM DISASTER COUNT',
     'TOTAL PEOPLE AFFECTED BY DISASTER',
     'log_GDP',
     'log_population',
     'log_disaster_count',
     'log_disaster_deaths',
     'log_people_affected',
     'log_gov_health',
     'log_private_health',
     'DEATHS_PER_100K',
     'AFFECTED_PER_100K',
     'log_deaths_per_100k',
     'log_affected_per_100k',
     'log_DEATHS_PER_100K',
     'log_AFFECTED_PER_100K']




```python
death_model = smf.ols(
    """
    log_DEATHS_PER_100K ~
    log_GDP +
    WGI_SCORE +
    log_gov_health +
    log_private_health +
    log_population +
    GINI
    """,
    data=gini_df
).fit()

print(death_model.summary())
```

                                 OLS Regression Results                            
    ===============================================================================
    Dep. Variable:     log_DEATHS_PER_100K   R-squared:                       0.376
    Model:                             OLS   Adj. R-squared:                  0.243
    Method:                  Least Squares   F-statistic:                     2.815
    Date:                 Wed, 08 Jul 2026   Prob (F-statistic):             0.0285
    Time:                         14:00:40   Log-Likelihood:                -53.841
    No. Observations:                   35   AIC:                             121.7
    Df Residuals:                       28   BIC:                             132.6
    Df Model:                            6                                         
    Covariance Type:             nonrobust                                         
    ======================================================================================
                             coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------------
    Intercept              6.0176      4.671      1.288      0.208      -3.550      15.585
    log_GDP               -1.3869      0.669     -2.074      0.047      -2.757      -0.017
    WGI_SCORE             -0.0321      0.032     -1.002      0.325      -0.098       0.034
    log_gov_health         0.2692      0.486      0.554      0.584      -0.726       1.264
    log_private_health     0.7351      0.507      1.449      0.158      -0.304       1.774
    log_population         0.2739      0.162      1.691      0.102      -0.058       0.606
    GINI                   0.0163      0.044      0.370      0.714      -0.074       0.106
    ==============================================================================
    Omnibus:                        0.630   Durbin-Watson:                   1.522
    Prob(Omnibus):                  0.730   Jarque-Bera (JB):                0.373
    Skew:                           0.251   Prob(JB):                        0.830
    Kurtosis:                       2.936   Cond. No.                     1.28e+03
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 1.28e+03. This might indicate that there are
    strong multicollinearity or other numerical problems.
    

## Interpreting Model 2 (Disaster Deaths per 100,000 Population)

This model looks at which country characteristics are associated with lower disaster death rates after taking population size into account.

Overall, the model was statistically significant, meaning that the variables included were able to explain some of the differences in disaster mortality between African countries.

The main finding was that **GDP per capita** was the only statistically significant predictor. Countries with higher GDP per capita tended to experience fewer disaster deaths per 100,000 people. This suggests that wealthier countries may have greater resources to prepare for, respond to and recover from disasters, helping to reduce loss of life.

The other variables, including government effectiveness, government health expenditure, private health expenditure, population size and income inequality, were not statistically significant in this model. This does not necessarily mean these factors are unimportant. Instead, it suggests that once all variables were considered together, there was not enough evidence to conclude that they were independently associated with disaster mortality within this sample of African countries.


## The Story So Far

So far, two different questions have been answered.

**Model 1 asked:** *What is associated with better disaster preparedness?*

The strongest finding was that **government effectiveness** was the most important predictor. Countries with more effective governments generally had higher Global Health Security Index (GHSI) scores, suggesting that strong institutions and governance are closely linked to preparedness.

**Model 2 asked:** *What is associated with fewer disaster deaths?*

Here, the main predictor was **GDP per capita**. Wealthier countries tended to experience fewer disaster deaths per 100,000 people, suggesting that economic resources may help reduce the human impact of disasters.

Taken together, these findings suggest that preparedness and disaster outcomes may not be driven by exactly the same factors. Effective governance appears to be most closely associated with building preparedness, while greater economic capacity appears to be more closely associated with reducing disaster mortality. The final model will examine which factors are associated with reducing the overall number of people affected by disasters, helping to complete the overall picture.


## Model 3: People Affected by Disasters

The final regression model examines the factors associated with the overall humanitarian impact of disasters, measured as the number of people affected per 100,000 population.

Unlike disaster mortality, which captures the most severe consequence of disasters, the number of people affected reflects the wider impact on communities, including displacement, injury, loss of livelihoods and disruption to essential services. Standardising this outcome by population size enables meaningful comparisons between countries of different sizes.

Together, the three regression models provide a more comprehensive understanding of disaster preparedness and resilience by examining factors associated with preparedness capacity, disaster mortality and the broader humanitarian consequences of disasters.



```python
affected_model = smf.ols(
    """
    log_AFFECTED_PER_100K ~
    log_GDP +
    WGI_SCORE +
    log_gov_health +
    log_private_health +
    log_population +
    GINI
    """,
    data=gini_df
).fit()

print(affected_model.summary())
```

                                  OLS Regression Results                             
    =================================================================================
    Dep. Variable:     log_AFFECTED_PER_100K   R-squared:                       0.480
    Model:                               OLS   Adj. R-squared:                  0.369
    Method:                    Least Squares   F-statistic:                     4.308
    Date:                   Wed, 08 Jul 2026   Prob (F-statistic):            0.00337
    Time:                           14:05:28   Log-Likelihood:                -29.084
    No. Observations:                     35   AIC:                             72.17
    Df Residuals:                         28   BIC:                             83.06
    Df Model:                              6                                         
    Covariance Type:               nonrobust                                         
    ======================================================================================
                             coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------------
    Intercept             14.4831      2.302      6.290      0.000       9.767      19.199
    log_GDP               -0.6711      0.330     -2.035      0.051      -1.346       0.004
    WGI_SCORE             -0.0358      0.016     -2.264      0.031      -0.068      -0.003
    log_gov_health         0.2694      0.240      1.125      0.270      -0.221       0.760
    log_private_health     0.5132      0.250      2.053      0.050       0.001       1.025
    log_population         0.1696      0.080      2.124      0.043       0.006       0.333
    GINI                  -0.0185      0.022     -0.851      0.402      -0.063       0.026
    ==============================================================================
    Omnibus:                        0.389   Durbin-Watson:                   1.831
    Prob(Omnibus):                  0.823   Jarque-Bera (JB):                0.490
    Skew:                          -0.220   Prob(JB):                        0.783
    Kurtosis:                       2.623   Cond. No.                     1.28e+03
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 1.28e+03. This might indicate that there are
    strong multicollinearity or other numerical problems.
    

## Model 3: People Affected by Disasters

This model examined the factors associated with the number of people affected by disasters per 100,000 population after accounting for differences in country size.

The overall model was statistically significant, indicating that the selected socioeconomic, governance and health system variables explained a meaningful proportion of the variation in humanitarian impacts across African countries.

Government effectiveness was significantly associated with fewer people affected by disasters, suggesting that countries with stronger institutions may be better able to prepare for, respond to and manage disaster events, thereby reducing their overall impact on communities.

GDP per capita also showed a negative association with the number of people affected and was close to conventional levels of statistical significance, indicating that greater economic capacity may contribute to reducing disaster impacts. In contrast, private health expenditure and population size were positively associated with the number of people affected, while government health expenditure and income inequality were not statistically significant predictors within the multivariable model.

Overall, these findings suggest that effective governance appears to play an important role in limiting the wider humanitarian consequences of disasters.


## Summary of Findings

The three regression models provide complementary insights into disaster preparedness and resilience across African countries.

The first model showed that **government effectiveness** was the strongest predictor of disaster preparedness, indicating that countries with more effective institutions generally achieved higher Global Health Security Index (GHSI) scores.

The second model examined disaster mortality and found that **GDP per capita** was the strongest predictor of lower disaster deaths per 100,000 population. This suggests that greater economic resources may help reduce loss of life during disasters.

The third model focused on the wider humanitarian impact of disasters and found that **government effectiveness** was again an important predictor, with countries exhibiting stronger governance tending to have fewer people affected by disasters relative to their population.

Taken together, these findings suggest that preparedness and resilience are influenced by different factors. Strong governance appears to be closely associated with building preparedness and reducing the wider impacts of disasters, while economic capacity appears to be particularly important for reducing disaster mortality. These results support the idea that improving disaster resilience requires more than increased financial investment alone; strengthening institutions and governance may also play a critical role.


# ... hold on wait a minute, y'all thought I was finished?

## Preparedness Gap Assessment

The regression models identify the factors associated with disaster preparedness across African countries. However, policymakers also need to understand whether their country is performing as expected given its socioeconomic circumstances.

To address this, predicted preparedness scores were calculated using the multiple regression model. These predicted values represent the level of preparedness expected for each country based on its economic capacity, governance, health expenditure, population size and income inequality.

The difference between the observed and predicted preparedness scores was then calculated. Countries performing above their predicted level may represent examples of comparatively efficient preparedness systems, whereas countries performing below their predicted level may have preparedness gaps that could potentially be addressed through targeted policy interventions.

This analysis provides the bridge between statistical modelling and practical policy recommendations by highlighting where preparedness may be stronger or weaker than expected.



```python
df_clean["Predicted_GHSI"] = main_model.predict(df_clean)
```


```python
df_clean["Preparedness_Gap"] = (
    df_clean["GHSI"] -
    df_clean["Predicted_GHSI"]
)
```


```python
gap_table = df_clean[[
    "COUNTRY_NAME",
    "GHSI",
    "Predicted_GHSI",
    "Preparedness_Gap"
]].reset_index(drop=True)

gap_table.insert(0, "No.", range(1, len(gap_table) + 1))

gap_table.head()
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
      <th>No.</th>
      <th>COUNTRY_NAME</th>
      <th>GHSI</th>
      <th>Predicted_GHSI</th>
      <th>Preparedness_Gap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Angola</td>
      <td>29.1</td>
      <td>32.561479</td>
      <td>-3.461479</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Burundi</td>
      <td>22.1</td>
      <td>25.255597</td>
      <td>-3.155597</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Benin</td>
      <td>25.4</td>
      <td>30.835455</td>
      <td>-5.435455</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Burkina Faso</td>
      <td>29.8</td>
      <td>29.088532</td>
      <td>0.711468</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Botswana</td>
      <td>33.6</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## Identifying Preparedness Gaps

The preparedness gap analysis identifies countries that perform better or worse than expected given their socioeconomic characteristics.

Countries with positive preparedness gaps have achieved higher preparedness scores than predicted by the regression model, suggesting that their preparedness systems may be performing relatively well despite their circumstances.

Conversely, countries with negative preparedness gaps have lower preparedness scores than expected. These countries may have unrealised preparedness potential and could benefit from targeted policy interventions to strengthen disaster resilience.

This analysis provides an evidence-based method for prioritising countries for further investigation and forms the basis for the policy recommendations developed in the final stage of the project.



```python
best = (
    df_clean
    .sort_values("Preparedness_Gap", ascending=False)
    [["COUNTRY_NAME","Preparedness_Gap"]]
    .reset_index(drop=True)
)

best.index += 1

best.head(15)
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
      <th>COUNTRY_NAME</th>
      <th>Preparedness_Gap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>South Africa</td>
      <td>7.338995</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sierra Leone</td>
      <td>7.154269</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Nigeria</td>
      <td>5.395459</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Ethiopia</td>
      <td>5.383347</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Comoros</td>
      <td>4.460666</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Kenya</td>
      <td>4.373908</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Uganda</td>
      <td>3.373395</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Zimbabwe</td>
      <td>3.114406</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Gambia, The</td>
      <td>2.976511</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Seychelles</td>
      <td>2.897811</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Mali</td>
      <td>2.365577</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Madagascar</td>
      <td>2.224843</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Togo</td>
      <td>0.731141</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Burkina Faso</td>
      <td>0.711468</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Niger</td>
      <td>0.182792</td>
    </tr>
  </tbody>
</table>
</div>




```python
worst = (
    df_clean
    .sort_values("Preparedness_Gap")
    [["COUNTRY_NAME","Preparedness_Gap"]]
    .reset_index(drop=True)
)

worst.index += 1

worst.head(15)
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
      <th>COUNTRY_NAME</th>
      <th>Preparedness_Gap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Egypt, Arab Rep.</td>
      <td>-8.576018</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Equatorial Guinea</td>
      <td>-7.220100</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Benin</td>
      <td>-5.435455</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Zambia</td>
      <td>-5.150620</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Central African Republic</td>
      <td>-3.537904</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Angola</td>
      <td>-3.461479</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Chad</td>
      <td>-3.383488</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Burundi</td>
      <td>-3.155597</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Tanzania</td>
      <td>-2.574168</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Cote d'Ivoire</td>
      <td>-1.947852</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Cameroon</td>
      <td>-1.932015</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Congo, Dem. Rep.</td>
      <td>-1.530215</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Senegal</td>
      <td>-1.470768</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Tunisia</td>
      <td>-1.152060</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Rwanda</td>
      <td>-0.745204</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_clean.loc[
    df_clean["COUNTRY_NAME"]=="Sierra Leone",
    [
        "COUNTRY_NAME",
        "GHSI",
        "Predicted_GHSI",
        "Preparedness_Gap",
        "WGI_SCORE",
        "INFORM_RISK_SCORE",
        "GOV_HEALTH_EXPENDITURE",
        "PRIVATE_HEALTH_EXPENDITURE",
        "GINI"
    ]
]
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
      <th>COUNTRY_NAME</th>
      <th>GHSI</th>
      <th>Predicted_GHSI</th>
      <th>Preparedness_Gap</th>
      <th>WGI_SCORE</th>
      <th>INFORM_RISK_SCORE</th>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <th>GINI</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>40</th>
      <td>Sierra Leone</td>
      <td>32.7</td>
      <td>25.545731</td>
      <td>7.154269</td>
      <td>32.536134</td>
      <td>5.1</td>
      <td>5.978927</td>
      <td>19.589493</td>
      <td>35.7</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(13,10))

plot = (
    df_clean
    .sort_values("Preparedness_Gap")
)

plt.barh(
    plot["COUNTRY_NAME"],
    plot["Preparedness_Gap"]
)

plt.axvline(
    0,
    linestyle="--",
    linewidth=1
)

plt.xlabel("Preparedness Gap (Observed − Predicted GHSI)")
plt.ylabel("Country")
plt.title("Preparedness Performance Relative to Model Expectations")

plt.tight_layout()

plt.show()
```


    
![png](output_93_0.png)
    


## Preparedness Gap Analysis

The preparedness gap analysis compared each country's observed Global Health Security Index (GHSI) score with the score predicted by the multiple regression model. This provides an indication of whether a country is performing better or worse than expected given its levels of economic development, governance, health expenditure, population size and income inequality.

Countries with **positive preparedness gaps** achieved higher GHSI scores than predicted by the model. These countries can be considered to be outperforming expectations, suggesting that factors not captured within the current analysis may be contributing positively to their preparedness. Such factors could include effective national leadership, successful preparedness programmes, external technical support, strong public health institutions or other context-specific policies. These countries warrant further investigation to better understand the practices and systems that may explain their comparatively strong performance.

Countries with **negative preparedness gaps** achieved lower GHSI scores than predicted by the model. These countries may possess greater preparedness potential than is currently being realised and may therefore represent priorities for further policy attention and investment.

One particularly notable finding is that **Sierra Leone** recorded one of the largest positive preparedness gaps in the dataset. This indicates that the country is performing better than would be expected based on its socioeconomic and governance characteristics alone. However, this finding should not be interpreted as evidence that Sierra Leone has achieved a high level of preparedness. Rather, it suggests that the country is outperforming expectations relative to its current circumstances.

Despite this encouraging result, Sierra Leone's overall GHSI score remains comparatively low by international standards, indicating that substantial opportunities remain to strengthen national preparedness. Therefore, the objective should not simply be to continue outperforming expectations, but to increase the country's absolute preparedness capacity through continued investment in health security, governance, surveillance, emergency response capabilities and broader system resilience.

The preparedness gap analysis therefore serves two complementary purposes. First, it identifies countries that may offer valuable lessons in effective preparedness despite resource constraints. Second, it highlights countries where preparedness appears weaker than expected and where targeted policy interventions may have the greatest impact. These findings provide the foundation for the next stage of the project, which focuses on identifying priority preparedness gaps and translating the statistical evidence into practical, evidence-based policy recommendations.


# # Stage 3 – Prioritising Preparedness

The previous analyses identified the factors associated with disaster preparedness and humanitarian outcomes across African countries. However, statistical relationships alone do not provide sufficient guidance for policymakers.

The objective of this stage is to translate the quantitative findings into practical priorities for strengthening disaster preparedness. Rather than producing another country ranking, this stage seeks to identify where governments should focus their efforts to achieve the greatest improvements in preparedness and resilience.

Priority areas are determined by combining three sources of evidence:

* the regression analyses identifying the strongest predictors of preparedness and disaster outcomes;
* the preparedness gap assessment, highlighting countries performing above or below expectations; and
* country-specific indicators describing governance, health system capacity, disaster risk and humanitarian outcomes.

This stage provides the evidence base for the final policy recommendations and decision-support dashboard.
Summary of Quantitative Findings

Before moving into the policy prioritisation stage, it is useful to summarise the main findings from the statistical analyses and consider what they mean in the context of disaster preparedness across Africa.

## Key Findings

### Correlation Analysis

* Government effectiveness showed one of the strongest positive relationships with disaster preparedness (GHSI).
* GDP per capita was strongly correlated with government and private health expenditure, indicating that wealthier countries generally invest more in health systems.
* INFORM Risk was strongly associated with higher disaster deaths and lower levels of governance and economic development.
* Several variables measuring disaster outcomes were highly correlated with each other, which was expected because they capture related aspects of disaster impact.

### Model 1 – What predicts disaster preparedness?

**Outcome:** Global Health Security Index (GHSI)

Key findings:

* Government effectiveness was the strongest independent predictor of preparedness.
* Population size was also positively associated with preparedness.
* GDP per capita was not a significant predictor once governance and health expenditure were taken into account.
* Government and private health expenditure were not independently significant after controlling for the other variables.
* Income inequality (Gini) showed no significant association with preparedness.

**Interpretation**

These findings suggest that strong institutions and effective governance may be more important than national wealth alone when building disaster preparedness.

---

### Model 2 – What predicts disaster mortality?

**Outcome:** Disaster deaths per 100,000 population

Key findings:

* GDP per capita was the only statistically significant predictor.
* Wealthier countries tended to experience fewer disaster deaths.
* Governance, health expenditure and inequality were not independently associated with mortality after controlling for the other variables.

**Interpretation**

Economic capacity appears to play an important role in reducing mortality during disasters, potentially through stronger infrastructure, emergency response capacity and access to healthcare.

---

### Model 3 – What predicts the wider humanitarian impact of disasters?

**Outcome:** People affected per 100,000 population

Key findings:

* Government effectiveness was significantly associated with fewer people affected by disasters.
* GDP per capita showed a negative relationship and was close to statistical significance.
* Private health expenditure and population size were positively associated with the number of people affected.
* Government health expenditure and income inequality were not significant predictors.

**Interpretation**

Strong governance appears to reduce the wider humanitarian consequences of disasters, while economic development may also contribute to limiting their impact.

---

### Preparedness Gap Analysis

Key findings:

* South Africa and Sierra Leone were among the countries that performed substantially better than predicted by the regression model.
* Egypt and Equatorial Guinea were among the countries performing below expectations.
* Sierra Leone demonstrated one of the largest positive preparedness gaps, indicating that it is achieving a higher level of preparedness than would be expected given its socioeconomic context.

**Interpretation**

This does **not** mean Sierra Leone is highly prepared. Rather, it suggests that the country is making comparatively effective use of its available resources. Despite this, its overall preparedness remains relatively low, meaning significant opportunities for improvement still exist.

---

## Critical Evaluation

Several important limitations should be considered when interpreting these findings.

* The analysis identifies **associations**, not causal relationships. It cannot demonstrate that improving governance or increasing GDP will directly improve preparedness.
* The sample includes only African countries, meaning the findings should not automatically be generalised to other regions.
* The Global Health Security Index represents one measure of preparedness and may not capture every aspect of national resilience.
* Some variables, particularly the Gini Index, contained missing observations, reducing the sample size for the regression analyses.
* Moderate multicollinearity was observed between GDP and health expenditure variables. This was expected because wealthier countries generally spend more on health. Variance Inflation Factor (VIF) analysis indicated that multicollinearity was not severe enough to justify removing these variables from the models.
* The regression models explain a substantial proportion of the variation in preparedness and disaster outcomes, but not all of it. Other factors—such as political stability, conflict, climate exposure, donor support, disaster governance arrangements and health system organisation—may also influence preparedness but were not included in the present analysis.

---

## Overall Conclusions

Overall, the analyses suggest three broad conclusions:

* **Government effectiveness** appears to be the strongest and most consistent predictor of disaster preparedness across African countries.
* **Economic capacity** appears to play a greater role in reducing disaster mortality than in determining preparedness itself.
* Countries performing better than expected, such as Sierra Leone, provide valuable opportunities to investigate what factors beyond economic resources contribute to stronger preparedness.

These findings provide the evidence base for the next stage of the project, which translates the statistical results into practical policy priorities and recommendations for strengthening disaster preparedness under constrained financial conditions.


# Stage 3 – Prioritising Preparedness

The statistical analyses identified the factors most strongly associated with disaster preparedness and humanitarian outcomes across African countries. However, statistical findings alone do not tell decision-makers what actions they should take.

The purpose of this stage is to translate the quantitative findings into practical priorities for governments. Rather than simply identifying which countries perform well or poorly, the focus shifts towards understanding which areas of preparedness are likely to require the greatest attention.

This stage combines evidence from the regression analyses, preparedness gap assessment and country-level indicators to identify priority areas for strengthening resilience. Particular attention is given to Sierra Leone as the primary policy case study, while comparisons with other African countries provide a broader evidence base for identifying good practice and potential areas for improvement.



```python
country_profile = df_clean[[
    "COUNTRY_NAME",
    "GHSI",
    "Predicted_GHSI",
    "Preparedness_Gap",
    "WGI_SCORE",
    "INFORM_RISK_SCORE",
    "GOV_HEALTH_EXPENDITURE",
    "PRIVATE_HEALTH_EXPENDITURE",
    "DEATHS_PER_100K",
    "AFFECTED_PER_100K"
]].copy()

# Reset the index
country_profile.reset_index(drop=True, inplace=True)

# Start numbering from 1
country_profile.index = range(1, len(country_profile) + 1)

country_profile.head()
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
      <th>COUNTRY_NAME</th>
      <th>GHSI</th>
      <th>Predicted_GHSI</th>
      <th>Preparedness_Gap</th>
      <th>WGI_SCORE</th>
      <th>INFORM_RISK_SCORE</th>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <th>DEATHS_PER_100K</th>
      <th>AFFECTED_PER_100K</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Angola</td>
      <td>29.1</td>
      <td>32.561479</td>
      <td>-3.461479</td>
      <td>36.073211</td>
      <td>6.0</td>
      <td>33.870996</td>
      <td>36.746709</td>
      <td>369.582110</td>
      <td>347037.924322</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Burundi</td>
      <td>22.1</td>
      <td>25.255597</td>
      <td>-3.155597</td>
      <td>31.778105</td>
      <td>6.4</td>
      <td>4.147275</td>
      <td>7.834755</td>
      <td>171.473210</td>
      <td>892648.159976</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Benin</td>
      <td>25.4</td>
      <td>30.835455</td>
      <td>-5.435455</td>
      <td>47.735147</td>
      <td>5.6</td>
      <td>6.824348</td>
      <td>21.825727</td>
      <td>9.618980</td>
      <td>238045.585192</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Burkina Faso</td>
      <td>29.8</td>
      <td>29.088532</td>
      <td>0.711468</td>
      <td>34.329539</td>
      <td>7.2</td>
      <td>22.556645</td>
      <td>34.168802</td>
      <td>146.104314</td>
      <td>492145.603371</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Botswana</td>
      <td>33.6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>59.446482</td>
      <td>3.3</td>
      <td>369.456459</td>
      <td>88.148375</td>
      <td>1.366055</td>
      <td>34064.615190</td>
    </tr>
  </tbody>
</table>
</div>




```python
country_profile = country_profile.sort_values(
    "Preparedness_Gap",
    ascending=False
)

country_profile.reset_index(drop=True, inplace=True)

country_profile.index += 1

country_profile.head(10)
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
      <th>COUNTRY_NAME</th>
      <th>GHSI</th>
      <th>Predicted_GHSI</th>
      <th>Preparedness_Gap</th>
      <th>WGI_SCORE</th>
      <th>INFORM_RISK_SCORE</th>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <th>DEATHS_PER_100K</th>
      <th>AFFECTED_PER_100K</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>South Africa</td>
      <td>45.8</td>
      <td>38.461005</td>
      <td>7.338995</td>
      <td>46.947812</td>
      <td>5.1</td>
      <td>330.611544</td>
      <td>194.939375</td>
      <td>5.513742</td>
      <td>407678.004397</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sierra Leone</td>
      <td>32.7</td>
      <td>25.545731</td>
      <td>7.154269</td>
      <td>32.536134</td>
      <td>5.1</td>
      <td>5.978927</td>
      <td>19.589493</td>
      <td>3.707570</td>
      <td>324821.452746</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Nigeria</td>
      <td>38.0</td>
      <td>32.604541</td>
      <td>5.395459</td>
      <td>31.768344</td>
      <td>7.1</td>
      <td>9.581414</td>
      <td>49.702417</td>
      <td>60.708267</td>
      <td>942705.268473</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Ethiopia</td>
      <td>37.8</td>
      <td>32.416653</td>
      <td>5.383347</td>
      <td>38.779817</td>
      <td>7.0</td>
      <td>7.625400</td>
      <td>17.820949</td>
      <td>63.040309</td>
      <td>411080.568198</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Comoros</td>
      <td>24.9</td>
      <td>20.439334</td>
      <td>4.460666</td>
      <td>27.840484</td>
      <td>3.3</td>
      <td>13.206058</td>
      <td>61.653875</td>
      <td>1.472509</td>
      <td>432777.593400</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Kenya</td>
      <td>38.8</td>
      <td>34.426092</td>
      <td>4.373908</td>
      <td>45.548292</td>
      <td>6.2</td>
      <td>38.154097</td>
      <td>29.696648</td>
      <td>27.443622</td>
      <td>769594.295175</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Uganda</td>
      <td>36.5</td>
      <td>33.126605</td>
      <td>3.373395</td>
      <td>44.532423</td>
      <td>6.5</td>
      <td>9.855858</td>
      <td>15.431913</td>
      <td>32.421980</td>
      <td>199132.227460</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Zimbabwe</td>
      <td>32.4</td>
      <td>29.285594</td>
      <td>3.114406</td>
      <td>32.136983</td>
      <td>5.3</td>
      <td>20.359019</td>
      <td>14.262123</td>
      <td>3.816930</td>
      <td>244887.440383</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Gambia, The</td>
      <td>28.7</td>
      <td>25.723489</td>
      <td>2.976511</td>
      <td>40.921900</td>
      <td>4.6</td>
      <td>11.566334</td>
      <td>9.685658</td>
      <td>5.492377</td>
      <td>200875.981054</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Seychelles</td>
      <td>31.8</td>
      <td>28.902189</td>
      <td>2.897811</td>
      <td>63.527352</td>
      <td>1.4</td>
      <td>538.460835</td>
      <td>227.826955</td>
      <td>1.629593</td>
      <td>120887.313615</td>
    </tr>
  </tbody>
</table>
</div>




```python
sl = country_profile[
    country_profile["COUNTRY_NAME"]=="Sierra Leone"
]

sl.T
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
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>COUNTRY_NAME</th>
      <td>Sierra Leone</td>
    </tr>
    <tr>
      <th>GHSI</th>
      <td>32.7</td>
    </tr>
    <tr>
      <th>Predicted_GHSI</th>
      <td>25.545731</td>
    </tr>
    <tr>
      <th>Preparedness_Gap</th>
      <td>7.154269</td>
    </tr>
    <tr>
      <th>WGI_SCORE</th>
      <td>32.536134</td>
    </tr>
    <tr>
      <th>INFORM_RISK_SCORE</th>
      <td>5.1</td>
    </tr>
    <tr>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <td>5.978927</td>
    </tr>
    <tr>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <td>19.589493</td>
    </tr>
    <tr>
      <th>DEATHS_PER_100K</th>
      <td>3.70757</td>
    </tr>
    <tr>
      <th>AFFECTED_PER_100K</th>
      <td>324821.452746</td>
    </tr>
  </tbody>
</table>
</div>




```python
country_profile["Governance"] = np.where(
    country_profile["WGI_SCORE"] >= country_profile["WGI_SCORE"].median(),
    "Good",
    "Needs Improvement"
)

country_profile["Preparedness"] = np.where(
    country_profile["GHSI"] >= country_profile["GHSI"].median(),
    "Good",
    "Needs Improvement"
)

country_profile["Risk"] = np.where(
    country_profile["INFORM_RISK_SCORE"] >= country_profile["INFORM_RISK_SCORE"].median(),
    "High",
    "Lower"
)
```


```python
country_profile[
    country_profile["COUNTRY_NAME"]=="Sierra Leone"
].T
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
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>COUNTRY_NAME</th>
      <td>Sierra Leone</td>
    </tr>
    <tr>
      <th>GHSI</th>
      <td>32.7</td>
    </tr>
    <tr>
      <th>Predicted_GHSI</th>
      <td>25.545731</td>
    </tr>
    <tr>
      <th>Preparedness_Gap</th>
      <td>7.154269</td>
    </tr>
    <tr>
      <th>WGI_SCORE</th>
      <td>32.536134</td>
    </tr>
    <tr>
      <th>INFORM_RISK_SCORE</th>
      <td>5.1</td>
    </tr>
    <tr>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <td>5.978927</td>
    </tr>
    <tr>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <td>19.589493</td>
    </tr>
    <tr>
      <th>DEATHS_PER_100K</th>
      <td>3.70757</td>
    </tr>
    <tr>
      <th>AFFECTED_PER_100K</th>
      <td>324821.452746</td>
    </tr>
    <tr>
      <th>Governance</th>
      <td>Needs Improvement</td>
    </tr>
    <tr>
      <th>Preparedness</th>
      <td>Good</td>
    </tr>
    <tr>
      <th>Risk</th>
      <td>Lower</td>
    </tr>
  </tbody>
</table>
</div>



## Developing the Preparedness Priority Framework

The final stage of the quantitative analysis translates the statistical findings into a practical decision-support framework.

Rather than simply ranking countries, the objective is to identify the areas where each country may benefit most from strengthening disaster preparedness.

Each country is assessed across several key domains that were shown to be important throughout the analysis, including preparedness, governance, disaster risk, health system investment and humanitarian outcomes.

For each indicator, countries are classified into performance categories using quartiles. This allows countries to be compared against the wider African distribution rather than against an arbitrary threshold.

The resulting Preparedness Priority Framework provides a consistent evidence-based summary of each country's strengths, weaknesses and priority areas for improvement. This framework forms the basis of the interactive dashboard and the policy recommendations developed in the final stage of the project.



```python
country_profile["GHSI_Level"] = pd.qcut(
    country_profile["GHSI"],
    q=4,
    labels=[
        "Priority Improvement",
        "Moderate",
        "Strong",
        "Very Strong"
    ]
)

country_profile["Governance_Level"] = pd.qcut(
    country_profile["WGI_SCORE"],
    q=4,
    labels=[
        "Priority Improvement",
        "Moderate",
        "Strong",
        "Very Strong"
    ]
)

country_profile["Risk_Level"] = pd.qcut(
    country_profile["INFORM_RISK_SCORE"],
    q=4,
    labels=[
        "Low",
        "Moderate",
        "High",
        "Very High"
    ]
)

country_profile["Mortality_Level"] = pd.qcut(
    country_profile["DEATHS_PER_100K"],
    q=4,
    labels=[
        "Low",
        "Moderate",
        "High",
        "Very High"
    ]
)

country_profile["Impact_Level"] = pd.qcut(
    country_profile["AFFECTED_PER_100K"],
    q=4,
    labels=[
        "Low",
        "Moderate",
        "High",
        "Very High"
    ]
)
```


```python
country_profile[
    country_profile["COUNTRY_NAME"]=="Sierra Leone"
][[
    "COUNTRY_NAME",
    "GHSI_Level",
    "Governance_Level",
    "Risk_Level",
    "Mortality_Level",
    "Impact_Level"
]]
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
      <th>COUNTRY_NAME</th>
      <th>GHSI_Level</th>
      <th>Governance_Level</th>
      <th>Risk_Level</th>
      <th>Mortality_Level</th>
      <th>Impact_Level</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>Sierra Leone</td>
      <td>Very Strong</td>
      <td>Moderate</td>
      <td>Moderate</td>
      <td>Low</td>
      <td>Moderate</td>
    </tr>
  </tbody>
</table>
</div>




```python
report_cards = country_profile[[
    "COUNTRY_NAME",
    "GHSI",
    "GHSI_Level",
    "Predicted_GHSI",
    "Preparedness_Gap",
    "WGI_SCORE",
    "Governance_Level",
    "INFORM_RISK_SCORE",
    "Risk_Level",
    "DEATHS_PER_100K",
    "Mortality_Level",
    "AFFECTED_PER_100K",
    "Impact_Level"
]].copy()

report_cards = report_cards.sort_values("COUNTRY_NAME")

report_cards.reset_index(drop=True, inplace=True)
report_cards.index += 1

report_cards.head()
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
      <th>COUNTRY_NAME</th>
      <th>GHSI</th>
      <th>GHSI_Level</th>
      <th>Predicted_GHSI</th>
      <th>Preparedness_Gap</th>
      <th>WGI_SCORE</th>
      <th>Governance_Level</th>
      <th>INFORM_RISK_SCORE</th>
      <th>Risk_Level</th>
      <th>DEATHS_PER_100K</th>
      <th>Mortality_Level</th>
      <th>AFFECTED_PER_100K</th>
      <th>Impact_Level</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Algeria</td>
      <td>26.2</td>
      <td>Moderate</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>46.456308</td>
      <td>Very Strong</td>
      <td>4.3</td>
      <td>Low</td>
      <td>29.157603</td>
      <td>High</td>
      <td>354976.948397</td>
      <td>Moderate</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Angola</td>
      <td>29.1</td>
      <td>Strong</td>
      <td>32.561479</td>
      <td>-3.461479</td>
      <td>36.073211</td>
      <td>Moderate</td>
      <td>6.0</td>
      <td>High</td>
      <td>369.582110</td>
      <td>Very High</td>
      <td>347037.924322</td>
      <td>Moderate</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Benin</td>
      <td>25.4</td>
      <td>Priority Improvement</td>
      <td>30.835455</td>
      <td>-5.435455</td>
      <td>47.735147</td>
      <td>Very Strong</td>
      <td>5.6</td>
      <td>High</td>
      <td>9.618980</td>
      <td>Moderate</td>
      <td>238045.585192</td>
      <td>Moderate</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Botswana</td>
      <td>33.6</td>
      <td>Very Strong</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>59.446482</td>
      <td>Very Strong</td>
      <td>3.3</td>
      <td>Low</td>
      <td>1.366055</td>
      <td>Low</td>
      <td>34064.615190</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Burkina Faso</td>
      <td>29.8</td>
      <td>Strong</td>
      <td>29.088532</td>
      <td>0.711468</td>
      <td>34.329539</td>
      <td>Moderate</td>
      <td>7.2</td>
      <td>Very High</td>
      <td>146.104314</td>
      <td>Very High</td>
      <td>492145.603371</td>
      <td>High</td>
    </tr>
  </tbody>
</table>
</div>




```python
country = "Sierra Leone"

report_cards[
    report_cards["COUNTRY_NAME"] == country
].T
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
      <th>44</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>COUNTRY_NAME</th>
      <td>Sierra Leone</td>
    </tr>
    <tr>
      <th>GHSI</th>
      <td>32.7</td>
    </tr>
    <tr>
      <th>GHSI_Level</th>
      <td>Very Strong</td>
    </tr>
    <tr>
      <th>Predicted_GHSI</th>
      <td>25.545731</td>
    </tr>
    <tr>
      <th>Preparedness_Gap</th>
      <td>7.154269</td>
    </tr>
    <tr>
      <th>WGI_SCORE</th>
      <td>32.536134</td>
    </tr>
    <tr>
      <th>Governance_Level</th>
      <td>Moderate</td>
    </tr>
    <tr>
      <th>INFORM_RISK_SCORE</th>
      <td>5.1</td>
    </tr>
    <tr>
      <th>Risk_Level</th>
      <td>Moderate</td>
    </tr>
    <tr>
      <th>DEATHS_PER_100K</th>
      <td>3.70757</td>
    </tr>
    <tr>
      <th>Mortality_Level</th>
      <td>Low</td>
    </tr>
    <tr>
      <th>AFFECTED_PER_100K</th>
      <td>324821.452746</td>
    </tr>
    <tr>
      <th>Impact_Level</th>
      <td>Moderate</td>
    </tr>
  </tbody>
</table>
</div>



#for power bi


```python
report_cards.to_csv(
    "Country_Report_Cards.csv",
    index=False
)

print("Saved successfully.")
```

    Saved successfully.
    

# Stage 3 Summary – Country Preparedness Report Cards

The statistical analyses identified the key factors associated with disaster preparedness and humanitarian outcomes across African countries. While these findings are valuable, they are not presented in a format that is immediately useful for policymakers. Decision-makers are typically less interested in regression coefficients and statistical outputs than in understanding what the evidence means for their own country.

To address this, a **Country Preparedness Report Card** was developed for every African country. The report card translates the statistical findings into a concise country profile that summarises preparedness, governance, disaster risk and humanitarian outcomes in a format that is easy to interpret.

Each report card includes:

* Overall preparedness (Global Health Security Index)
* Predicted preparedness based on the regression model
* Preparedness Gap (Observed GHSI − Predicted GHSI)
* Government Effectiveness score
* INFORM Risk score
* Disaster deaths per 100,000 population
* People affected per 100,000 population

To improve interpretation, key indicators were grouped into performance categories using quartiles. This allowed countries to be classified relative to the wider African distribution rather than using arbitrary cut-off values. Indicators were labelled using categories such as **Priority Improvement**, **Moderate**, **Strong** and **Very Strong** for preparedness and governance, while disaster risk and humanitarian outcomes were classified from **Low** to **Very High**.

The Preparedness Gap provided an additional layer of interpretation by comparing each country's observed preparedness with the level predicted by the regression model. Positive values indicate



```python
from pathlib import Path

# CHANGE THIS TO WHERE YOU WANT THE PROJECT
project_folder = Path(r"C:\Users\OJ\Documents\Disaster Preparedness Project")

folders = [
    "Data",
    "Figures",
    "Tables",
    "Notebook",
    "Dashboard"
]

for folder in folders:
    (project_folder / folder).mkdir(parents=True, exist_ok=True)

print("Project folders created successfully!")
```

    Project folders created successfully!
    


```python
correlation_matrix = df_clean[[
    "log_GDP",
    "log_population",
    "WGI_SCORE",
    "GHSI",
    "INFORM_RISK_SCORE",
    "log_gov_health",
    "log_private_health",
    "GINI",
    "log_disaster_count",
    "log_disaster_deaths",
    "log_people_affected"
]].corr()

correlation_matrix
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
      <th>log_GDP</th>
      <th>log_population</th>
      <th>WGI_SCORE</th>
      <th>GHSI</th>
      <th>INFORM_RISK_SCORE</th>
      <th>log_gov_health</th>
      <th>log_private_health</th>
      <th>GINI</th>
      <th>log_disaster_count</th>
      <th>log_disaster_deaths</th>
      <th>log_people_affected</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>log_GDP</th>
      <td>1.000000</td>
      <td>-0.439983</td>
      <td>0.562838</td>
      <td>0.225282</td>
      <td>-0.721328</td>
      <td>0.901097</td>
      <td>0.821623</td>
      <td>-0.014714</td>
      <td>-0.396348</td>
      <td>-0.531591</td>
      <td>-0.442939</td>
    </tr>
    <tr>
      <th>log_population</th>
      <td>-0.439983</td>
      <td>1.000000</td>
      <td>-0.164049</td>
      <td>0.252727</td>
      <td>0.683675</td>
      <td>-0.413834</td>
      <td>-0.368063</td>
      <td>0.246296</td>
      <td>0.841432</td>
      <td>0.780685</td>
      <td>0.882597</td>
    </tr>
    <tr>
      <th>WGI_SCORE</th>
      <td>0.562838</td>
      <td>-0.164049</td>
      <td>1.000000</td>
      <td>0.592729</td>
      <td>-0.646824</td>
      <td>0.642192</td>
      <td>0.404425</td>
      <td>-0.085315</td>
      <td>-0.338565</td>
      <td>-0.484061</td>
      <td>-0.372206</td>
    </tr>
    <tr>
      <th>GHSI</th>
      <td>0.225282</td>
      <td>0.252727</td>
      <td>0.592729</td>
      <td>1.000000</td>
      <td>-0.202469</td>
      <td>0.328365</td>
      <td>0.235140</td>
      <td>0.192005</td>
      <td>0.153778</td>
      <td>-0.064625</td>
      <td>0.094289</td>
    </tr>
    <tr>
      <th>INFORM_RISK_SCORE</th>
      <td>-0.721328</td>
      <td>0.683675</td>
      <td>-0.646824</td>
      <td>-0.202469</td>
      <td>1.000000</td>
      <td>-0.708167</td>
      <td>-0.595826</td>
      <td>0.244863</td>
      <td>0.734092</td>
      <td>0.837946</td>
      <td>0.761147</td>
    </tr>
    <tr>
      <th>log_gov_health</th>
      <td>0.901097</td>
      <td>-0.413834</td>
      <td>0.642192</td>
      <td>0.328365</td>
      <td>-0.708167</td>
      <td>1.000000</td>
      <td>0.764929</td>
      <td>0.120477</td>
      <td>-0.363742</td>
      <td>-0.530080</td>
      <td>-0.444443</td>
    </tr>
    <tr>
      <th>log_private_health</th>
      <td>0.821623</td>
      <td>-0.368063</td>
      <td>0.404425</td>
      <td>0.235140</td>
      <td>-0.595826</td>
      <td>0.764929</td>
      <td>1.000000</td>
      <td>-0.236707</td>
      <td>-0.236884</td>
      <td>-0.411805</td>
      <td>-0.305892</td>
    </tr>
    <tr>
      <th>GINI</th>
      <td>-0.014714</td>
      <td>0.246296</td>
      <td>-0.085315</td>
      <td>0.192005</td>
      <td>0.244863</td>
      <td>0.120477</td>
      <td>-0.236707</td>
      <td>1.000000</td>
      <td>0.210674</td>
      <td>0.189756</td>
      <td>0.146728</td>
    </tr>
    <tr>
      <th>log_disaster_count</th>
      <td>-0.396348</td>
      <td>0.841432</td>
      <td>-0.338565</td>
      <td>0.153778</td>
      <td>0.734092</td>
      <td>-0.363742</td>
      <td>-0.236884</td>
      <td>0.210674</td>
      <td>1.000000</td>
      <td>0.802467</td>
      <td>0.953845</td>
    </tr>
    <tr>
      <th>log_disaster_deaths</th>
      <td>-0.531591</td>
      <td>0.780685</td>
      <td>-0.484061</td>
      <td>-0.064625</td>
      <td>0.837946</td>
      <td>-0.530080</td>
      <td>-0.411805</td>
      <td>0.189756</td>
      <td>0.802467</td>
      <td>1.000000</td>
      <td>0.833212</td>
    </tr>
    <tr>
      <th>log_people_affected</th>
      <td>-0.442939</td>
      <td>0.882597</td>
      <td>-0.372206</td>
      <td>0.094289</td>
      <td>0.761147</td>
      <td>-0.444443</td>
      <td>-0.305892</td>
      <td>0.146728</td>
      <td>0.953845</td>
      <td>0.833212</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Dashboard data
country_profile.to_csv(
    project_folder / "Data" / "Dashboard_Master.csv",
    index=False
)

# Report cards
report_cards.to_csv(
    project_folder / "Data" / "Country_Report_Cards.csv",
    index=False
)

# Clean master dataset
df_clean.to_excel(
    project_folder / "Data" / "Africa_Master.xlsx",
    index=False
)

print("Datasets exported.")# Table 1 - Descriptive Statistics
df_clean.describe().T.to_excel(
    project_folder / "Tables" / "Table_1_Descriptive_Statistics.xlsx"
)

# Table 2 - Correlation Matrix
correlation_matrix.to_excel(
    project_folder / "Tables" / "Table_2_Correlation_Matrix.xlsx"
)

# Table 3 - Country Report Cards
report_cards.to_excel(
    project_folder / "Tables" / "Table_3_Country_Report_Cards.xlsx",
    index=False
)

# Table 4 - Dashboard Master
country_profile.to_excel(
    project_folder / "Tables" / "Table_4_Dashboard_Master.xlsx",
    index=False
)

print("Tables exported successfully!")
```

    Datasets exported.
    Tables exported successfully!
    


```python
print(project_folder)country_profile.to_csv(
    project_folder / "Data" / "Dashboard_Master.csv",
    index=False
)

print("Dashboard_Master.csv saved!")
```


      Cell In[110], line 1
        print(project_folder)country_profile.to_csv(
                             ^
    SyntaxError: invalid syntax
    



```python
print(type(country_profile))
print(type(report_cards))
```

    <class 'pandas.core.frame.DataFrame'>
    <class 'pandas.core.frame.DataFrame'>
    


```python
country_profile.to_csv(
    project_folder / "Data" / "Dashboard_Master.csv",
    index=False
)

print("Dashboard_Master.csv saved!")
```

    Dashboard_Master.csv saved!
    


```python
report_cards.to_csv(
    project_folder / "Data" / "Country_Report_Cards.csv",
    index=False
)

print("Country_Report_Cards.csv saved!")
```

    Country_Report_Cards.csv saved!
    


```python
import os

print(os.listdir(project_folder / "Data"))
```

    ['Africa_Master.xlsx', 'Country_Report_Cards.csv', 'Dashboard_Master.csv']
    


```python
df_clean.columns
```




    Index(['ISO3', 'REGION', 'COUNTRY_NAME', 'GDP_PER_CAPITA', 'TOTAL_POPULATION',
           'WGI_SCORE', 'GHSI', 'INFORM_RISK_SCORE', 'GOV_HEALTH_EXPENDITURE',
           'PRIVATE_HEALTH_EXPENDITURE', 'GINI', 'DISASTER COUNT',
           'DEATHS FROM DISASTER COUNT', 'TOTAL PEOPLE AFFECTED BY DISASTER',
           'log_GDP', 'log_population', 'log_disaster_count',
           'log_disaster_deaths', 'log_people_affected', 'log_gov_health',
           'log_private_health', 'DEATHS_PER_100K', 'AFFECTED_PER_100K',
           'log_deaths_per_100k', 'log_affected_per_100k', 'log_DEATHS_PER_100K',
           'log_AFFECTED_PER_100K', 'Predicted_GHSI', 'Preparedness_Gap'],
          dtype='object')



## purely for dashboard


```python
import matplotlib.pyplot as plt
import seaborn as sns

# Variables for stakeholder dashboard
corr_vars = [
    "GHSI",
    "WGI_SCORE",
    "INFORM_RISK_SCORE",
    "log_gov_health",
    "log_private_health",
    "log_GDP",
    "GINI",
    "log_population"
]

# Create correlation matrix
corr = df_clean[corr_vars].corr()

# Rename variables
corr = corr.rename(
    index={
        "GHSI": "Preparedness\nScore",
        "WGI_SCORE": "Government\nEffectiveness",
        "INFORM_RISK_SCORE": "Disaster\nRisk",
        "log_gov_health": "Public Health\nSpending",
        "log_private_health": "Private Health\nSpending",
        "log_GDP": "GDP per\nCapita",
        "GINI": "Income\nInequality",
        "log_population": "Population"
    },
    columns={
        "GHSI": "Preparedness\nScore",
        "WGI_SCORE": "Government\nEffectiveness",
        "INFORM_RISK_SCORE": "Disaster\nRisk",
        "log_gov_health": "Public Health\nSpending",
        "log_private_health": "Private Health\nSpending",
        "log_GDP": "GDP per\nCapita",
        "GINI": "Income\nInequality",
        "log_population": "Population"
    }
)

# Plot
plt.figure(figsize=(10,8))

sns.heatmap(
    corr,
    annot=True,
    fmt=".2f",
    cmap="RdYlGn",
    center=0,
    linewidths=1,
    square=True,
    annot_kws={"size":10},
    cbar_kws={"label":"Correlation"}
)

plt.title(
    "Relationships Between Key Preparedness Indicators",
    fontsize=16,
    fontweight="bold",
    pad=20
)

plt.xticks(rotation=35, ha="right", fontsize=10)
plt.yticks(rotation=0, fontsize=10)

plt.tight_layout()

plt.savefig(
    "stakeholder_correlation_heatmap.png",
    dpi=300,
    bbox_inches="tight"
)

plt.show()
```


    
![png](output_120_0.png)
    



```python
plt.savefig("stakeholder_correlation_heatmap.png", dpi=300, bbox_inches="tight")
```


    <Figure size 640x480 with 0 Axes>



```python
from sklearn.preprocessing import StandardScaler
import statsmodels.api as sm
import pandas as pd

# Variables
X = gini_df[
    [
        "log_GDP",
        "WGI_SCORE",
        "log_gov_health",
        "log_private_health",
        "log_population",
        "GINI"
    ]
]

y = gini_df["GHSI"]

# Standardise
scaler = StandardScaler()
X_std = pd.DataFrame(
    scaler.fit_transform(X),
    columns=X.columns
)

y_std = StandardScaler().fit_transform(y.values.reshape(-1,1)).ravel()

# Fit model
X_std = sm.add_constant(X_std)
std_model = sm.OLS(y_std, X_std).fit()

print(std_model.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.546
    Model:                            OLS   Adj. R-squared:                  0.449
    Method:                 Least Squares   F-statistic:                     5.612
    Date:                Thu, 09 Jul 2026   Prob (F-statistic):           0.000632
    Time:                        21:31:56   Log-Likelihood:                -35.844
    No. Observations:                  35   AIC:                             85.69
    Df Residuals:                      28   BIC:                             96.58
    Df Model:                           6                                         
    Covariance Type:            nonrobust                                         
    ======================================================================================
                             coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------------
    const               2.776e-16      0.127   2.18e-15      1.000      -0.261       0.261
    log_GDP                0.0962      0.330      0.292      0.773      -0.579       0.772
    WGI_SCORE              0.4440      0.190      2.334      0.027       0.054       0.834
    log_gov_health         0.0829      0.332      0.250      0.805      -0.597       0.763
    log_private_health     0.0485      0.290      0.167      0.868      -0.545       0.642
    log_population         0.5381      0.144      3.744      0.001       0.244       0.832
    GINI                   0.1003      0.171      0.587      0.562      -0.250       0.450
    ==============================================================================
    Omnibus:                        0.055   Durbin-Watson:                   2.433
    Prob(Omnibus):                  0.973   Jarque-Bera (JB):                0.245
    Skew:                          -0.058   Prob(JB):                        0.884
    Kurtosis:                       2.606   Cond. No.                         5.62
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    


```python
import pandas as pd

drivers = pd.DataFrame({
    "Driver": [
        "Population",
        "Government Effectiveness",
        "Income Inequality",
        "GDP per Capita",
        "Public Health Spending",
        "Private Health Spending"
    ],
    "Standardised Effect": [
        0.5381,
        0.4440,
        0.1003,
        0.0962,
        0.0829,
        0.0485
    ],
    "Significant": [
        "Yes",
        "Yes",
        "No",
        "No",
        "No",
        "No"
    ]
})

drivers = drivers.sort_values(
    "Standardised Effect",
    ascending=False
)

drivers.to_csv(
    "preparedness_drivers.csv",
    index=False
)

drivers
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
      <th>Driver</th>
      <th>Standardised Effect</th>
      <th>Significant</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Population</td>
      <td>0.5381</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Government Effectiveness</td>
      <td>0.4440</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Income Inequality</td>
      <td>0.1003</td>
      <td>No</td>
    </tr>
    <tr>
      <th>3</th>
      <td>GDP per Capita</td>
      <td>0.0962</td>
      <td>No</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Public Health Spending</td>
      <td>0.0829</td>
      <td>No</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Private Health Spending</td>
      <td>0.0485</td>
      <td>No</td>
    </tr>
  </tbody>
</table>
</div>




```python
import os
print(os.getcwd())
```

    C:\Users\OJ
    


```python
import os

print([f for f in os.listdir(r"C:\Users\OJ") if f.endswith(".csv")])
```

    ['Country_Report_Cards.csv', 'preparedness_drivers.csv']
    

MACHINE LEARNING CONTINUED

# 11. Positive Deviance Analysis

## Aim

The purpose of this analysis is to identify countries whose observed preparedness differs substantially from that predicted by the regression model.

Whereas the regression analysis identified the structural drivers of preparedness, positive deviance analysis highlights countries that perform significantly better or worse than expected after accounting for socioeconomic, governance and health system characteristics.

Countries with positive residuals demonstrate higher preparedness than predicted and represent valuable policy case studies. Conversely, countries with negative residuals may indicate unrealised preparedness potential.

These findings will support evidence-based benchmarking and inform the policy recommendations developed for Sierra Leone.


```python
print(main_model.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   GHSI   R-squared:                       0.546
    Model:                            OLS   Adj. R-squared:                  0.449
    Method:                 Least Squares   F-statistic:                     5.612
    Date:                Fri, 10 Jul 2026   Prob (F-statistic):           0.000632
    Time:                        20:15:16   Log-Likelihood:                -96.247
    No. Observations:                  35   AIC:                             206.5
    Df Residuals:                      28   BIC:                             217.4
    Df Model:                           6                                         
    Covariance Type:            nonrobust                                         
    ======================================================================================
                             coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------------
    Intercept            -24.3018     15.688     -1.549      0.133     -56.437       7.833
    log_GDP                0.6552      2.246      0.292      0.773      -3.946       5.257
    WGI_SCORE              0.2514      0.108      2.334      0.027       0.031       0.472
    log_gov_health         0.4073      1.632      0.250      0.805      -2.935       3.750
    log_private_health     0.2851      1.704      0.167      0.868      -3.205       3.775
    log_population         2.0371      0.544      3.744      0.001       0.922       3.152
    GINI                   0.0868      0.148      0.587      0.562      -0.216       0.390
    ==============================================================================
    Omnibus:                        0.055   Durbin-Watson:                   2.433
    Prob(Omnibus):                  0.973   Jarque-Bera (JB):                0.245
    Skew:                          -0.058   Prob(JB):                        0.884
    Kurtosis:                       2.606   Cond. No.                     1.28e+03
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 1.28e+03. This might indicate that there are
    strong multicollinearity or other numerical problems.
    


```python
# ============================================================
# CALCULATE EXPECTED PREPAREDNESS
# ============================================================

gini_df["Expected Preparedness"] = main_model.predict(gini_df)

gini_df[
    ["COUNTRY_NAME", "GHSI", "Expected Preparedness"]
].head()
```

    C:\Users\OJ\AppData\Local\Temp\ipykernel_6524\644998204.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      gini_df["Expected Preparedness"] = main_model.predict(gini_df)
    




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
      <th>COUNTRY_NAME</th>
      <th>GHSI</th>
      <th>Expected Preparedness</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Angola</td>
      <td>29.1</td>
      <td>32.561479</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Burundi</td>
      <td>22.1</td>
      <td>25.255597</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Benin</td>
      <td>25.4</td>
      <td>30.835455</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Burkina Faso</td>
      <td>29.8</td>
      <td>29.088532</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Central African Republic</td>
      <td>18.6</td>
      <td>22.137904</td>
    </tr>
  </tbody>
</table>
</div>




```python
# ============================================================
# CALCULATE PREPAREDNESS GAP
# ============================================================

gini_df["Preparedness Gap"] = (
    gini_df["GHSI"] -
    gini_df["Expected Preparedness"]
)

gini_df[
    [
        "COUNTRY_NAME",
        "GHSI",
        "Expected Preparedness",
        "Preparedness Gap"
    ]
].head()
```

    C:\Users\OJ\AppData\Local\Temp\ipykernel_6524\952623989.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      gini_df["Preparedness Gap"] = (
    




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
      <th>COUNTRY_NAME</th>
      <th>GHSI</th>
      <th>Expected Preparedness</th>
      <th>Preparedness Gap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Angola</td>
      <td>29.1</td>
      <td>32.561479</td>
      <td>-3.461479</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Burundi</td>
      <td>22.1</td>
      <td>25.255597</td>
      <td>-3.155597</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Benin</td>
      <td>25.4</td>
      <td>30.835455</td>
      <td>-5.435455</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Burkina Faso</td>
      <td>29.8</td>
      <td>29.088532</td>
      <td>0.711468</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Central African Republic</td>
      <td>18.6</td>
      <td>22.137904</td>
      <td>-3.537904</td>
    </tr>
  </tbody>
</table>
</div>




```python
# ============================================================
# RANK COUNTRIES BY PREPAREDNESS GAP
# ============================================================

preparedness_rank = gini_df.sort_values(
    "Preparedness Gap",
    ascending=False
).reset_index(drop=True)

preparedness_rank[
    [
        "COUNTRY_NAME",
        "GHSI",
        "Expected Preparedness",
        "Preparedness Gap"
    ]
].head(10)
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
      <th>COUNTRY_NAME</th>
      <th>GHSI</th>
      <th>Expected Preparedness</th>
      <th>Preparedness Gap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>South Africa</td>
      <td>45.8</td>
      <td>38.461005</td>
      <td>7.338995</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sierra Leone</td>
      <td>32.7</td>
      <td>25.545731</td>
      <td>7.154269</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Nigeria</td>
      <td>38.0</td>
      <td>32.604541</td>
      <td>5.395459</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ethiopia</td>
      <td>37.8</td>
      <td>32.416653</td>
      <td>5.383347</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Comoros</td>
      <td>24.9</td>
      <td>20.439334</td>
      <td>4.460666</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Kenya</td>
      <td>38.8</td>
      <td>34.426092</td>
      <td>4.373908</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Uganda</td>
      <td>36.5</td>
      <td>33.126605</td>
      <td>3.373395</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Zimbabwe</td>
      <td>32.4</td>
      <td>29.285594</td>
      <td>3.114406</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Gambia, The</td>
      <td>28.7</td>
      <td>25.723489</td>
      <td>2.976511</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Seychelles</td>
      <td>31.8</td>
      <td>28.902189</td>
      <td>2.897811</td>
    </tr>
  </tbody>
</table>
</div>




```python
# ============================================================
# POSITIVE DEVIANTS
# ============================================================

positive_deviants = preparedness_rank.head(10).copy()

positive_deviants["Classification"] = "Positive Deviant"

positive_deviants
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
      <th>ISO3</th>
      <th>REGION</th>
      <th>COUNTRY_NAME</th>
      <th>GDP_PER_CAPITA</th>
      <th>TOTAL_POPULATION</th>
      <th>WGI_SCORE</th>
      <th>GHSI</th>
      <th>INFORM_RISK_SCORE</th>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <th>...</th>
      <th>log_private_health</th>
      <th>DEATHS_PER_100K</th>
      <th>AFFECTED_PER_100K</th>
      <th>log_deaths_per_100k</th>
      <th>log_affected_per_100k</th>
      <th>log_DEATHS_PER_100K</th>
      <th>log_AFFECTED_PER_100K</th>
      <th>Expected Preparedness</th>
      <th>Preparedness Gap</th>
      <th>Classification</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ZAF</td>
      <td>Africa</td>
      <td>South Africa</td>
      <td>6597.714509</td>
      <td>64747319</td>
      <td>46.947812</td>
      <td>45.8</td>
      <td>5.1</td>
      <td>330.611544</td>
      <td>194.939375</td>
      <td>...</td>
      <td>5.272689</td>
      <td>5.513742</td>
      <td>407678.004397</td>
      <td>1.873914</td>
      <td>12.918235</td>
      <td>1.873914</td>
      <td>12.918235</td>
      <td>38.461005</td>
      <td>7.338995</td>
      <td>Positive Deviant</td>
    </tr>
    <tr>
      <th>1</th>
      <td>SLE</td>
      <td>Africa</td>
      <td>Sierra Leone</td>
      <td>846.296172</td>
      <td>8819794</td>
      <td>32.536134</td>
      <td>32.7</td>
      <td>5.1</td>
      <td>5.978927</td>
      <td>19.589493</td>
      <td>...</td>
      <td>2.974993</td>
      <td>3.707570</td>
      <td>324821.452746</td>
      <td>1.549172</td>
      <td>12.691034</td>
      <td>1.549172</td>
      <td>12.691034</td>
      <td>25.545731</td>
      <td>7.154269</td>
      <td>Positive Deviant</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NGA</td>
      <td>Africa</td>
      <td>Nigeria</td>
      <td>1224.254102</td>
      <td>237527782</td>
      <td>31.768344</td>
      <td>38.0</td>
      <td>7.1</td>
      <td>9.581414</td>
      <td>49.702417</td>
      <td>...</td>
      <td>3.906054</td>
      <td>60.708267</td>
      <td>942705.268473</td>
      <td>4.122418</td>
      <td>13.756510</td>
      <td>4.122418</td>
      <td>13.756510</td>
      <td>32.604541</td>
      <td>5.395459</td>
      <td>Positive Deviant</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ETH</td>
      <td>Africa</td>
      <td>Ethiopia</td>
      <td>932.729353</td>
      <td>135472051</td>
      <td>38.779817</td>
      <td>37.8</td>
      <td>7.0</td>
      <td>7.625400</td>
      <td>17.820949</td>
      <td>...</td>
      <td>2.880375</td>
      <td>63.040309</td>
      <td>411080.568198</td>
      <td>4.159513</td>
      <td>12.926547</td>
      <td>4.159513</td>
      <td>12.926547</td>
      <td>32.416653</td>
      <td>5.383347</td>
      <td>Positive Deviant</td>
    </tr>
    <tr>
      <th>4</th>
      <td>COM</td>
      <td>Africa</td>
      <td>Comoros</td>
      <td>2055.759216</td>
      <td>882847</td>
      <td>27.840484</td>
      <td>24.9</td>
      <td>3.3</td>
      <td>13.206058</td>
      <td>61.653875</td>
      <td>...</td>
      <td>4.121536</td>
      <td>1.472509</td>
      <td>432777.593400</td>
      <td>0.905233</td>
      <td>12.977982</td>
      <td>0.905233</td>
      <td>12.977982</td>
      <td>20.439334</td>
      <td>4.460666</td>
      <td>Positive Deviant</td>
    </tr>
    <tr>
      <th>5</th>
      <td>KEN</td>
      <td>Africa</td>
      <td>Kenya</td>
      <td>2362.860912</td>
      <td>57532493</td>
      <td>45.548292</td>
      <td>38.8</td>
      <td>6.2</td>
      <td>38.154097</td>
      <td>29.696648</td>
      <td>...</td>
      <td>3.391034</td>
      <td>27.443622</td>
      <td>769594.295175</td>
      <td>3.347924</td>
      <td>13.553620</td>
      <td>3.347924</td>
      <td>13.553620</td>
      <td>34.426092</td>
      <td>4.373908</td>
      <td>Positive Deviant</td>
    </tr>
    <tr>
      <th>6</th>
      <td>UGA</td>
      <td>Africa</td>
      <td>Uganda</td>
      <td>1206.304508</td>
      <td>51384894</td>
      <td>44.532423</td>
      <td>36.5</td>
      <td>6.5</td>
      <td>9.855858</td>
      <td>15.431913</td>
      <td>...</td>
      <td>2.736438</td>
      <td>32.421980</td>
      <td>199132.227460</td>
      <td>3.509214</td>
      <td>12.201729</td>
      <td>3.509214</td>
      <td>12.201729</td>
      <td>33.126605</td>
      <td>3.373395</td>
      <td>Positive Deviant</td>
    </tr>
    <tr>
      <th>7</th>
      <td>ZWE</td>
      <td>Africa</td>
      <td>Zimbabwe</td>
      <td>3021.430199</td>
      <td>16950795</td>
      <td>32.136983</td>
      <td>32.4</td>
      <td>5.3</td>
      <td>20.359019</td>
      <td>14.262123</td>
      <td>...</td>
      <td>2.657607</td>
      <td>3.816930</td>
      <td>244887.440383</td>
      <td>1.572137</td>
      <td>12.408558</td>
      <td>1.572137</td>
      <td>12.408558</td>
      <td>29.285594</td>
      <td>3.114406</td>
      <td>Positive Deviant</td>
    </tr>
    <tr>
      <th>8</th>
      <td>GMB</td>
      <td>Africa</td>
      <td>Gambia, The</td>
      <td>919.060423</td>
      <td>2822093</td>
      <td>40.921900</td>
      <td>28.7</td>
      <td>4.6</td>
      <td>11.566334</td>
      <td>9.685658</td>
      <td>...</td>
      <td>2.270646</td>
      <td>5.492377</td>
      <td>200875.981054</td>
      <td>1.870629</td>
      <td>12.210448</td>
      <td>1.870629</td>
      <td>12.210448</td>
      <td>25.723489</td>
      <td>2.976511</td>
      <td>Positive Deviant</td>
    </tr>
    <tr>
      <th>9</th>
      <td>SYC</td>
      <td>Africa</td>
      <td>Seychelles</td>
      <td>19449.383635</td>
      <td>122730</td>
      <td>63.527352</td>
      <td>31.8</td>
      <td>1.4</td>
      <td>538.460835</td>
      <td>227.826955</td>
      <td>...</td>
      <td>5.428586</td>
      <td>1.629593</td>
      <td>120887.313615</td>
      <td>0.966829</td>
      <td>11.702622</td>
      <td>0.966829</td>
      <td>11.702622</td>
      <td>28.902189</td>
      <td>2.897811</td>
      <td>Positive Deviant</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 30 columns</p>
</div>




```python
# ============================================================
# NEGATIVE DEVIANTS
# ============================================================

negative_deviants = preparedness_rank.tail(10).copy()

negative_deviants = negative_deviants.sort_values(
    "Preparedness Gap"
)

negative_deviants["Classification"] = "Negative Deviant"

negative_deviants
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
      <th>ISO3</th>
      <th>REGION</th>
      <th>COUNTRY_NAME</th>
      <th>GDP_PER_CAPITA</th>
      <th>TOTAL_POPULATION</th>
      <th>WGI_SCORE</th>
      <th>GHSI</th>
      <th>INFORM_RISK_SCORE</th>
      <th>GOV_HEALTH_EXPENDITURE</th>
      <th>PRIVATE_HEALTH_EXPENDITURE</th>
      <th>...</th>
      <th>log_private_health</th>
      <th>DEATHS_PER_100K</th>
      <th>AFFECTED_PER_100K</th>
      <th>log_deaths_per_100k</th>
      <th>log_affected_per_100k</th>
      <th>log_DEATHS_PER_100K</th>
      <th>log_AFFECTED_PER_100K</th>
      <th>Expected Preparedness</th>
      <th>Preparedness Gap</th>
      <th>Classification</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <td>EGY</td>
      <td>Africa</td>
      <td>Egypt, Arab Rep.</td>
      <td>3085.807120</td>
      <td>118365995</td>
      <td>49.453283</td>
      <td>28.0</td>
      <td>5.0</td>
      <td>45.604674</td>
      <td>90.908912</td>
      <td>...</td>
      <td>4.509858</td>
      <td>13.047666</td>
      <td>4.118710e+05</td>
      <td>2.642456</td>
      <td>12.928468</td>
      <td>2.642456</td>
      <td>12.928468</td>
      <td>36.576018</td>
      <td>-8.576018</td>
      <td>Negative Deviant</td>
    </tr>
    <tr>
      <th>33</th>
      <td>GNQ</td>
      <td>Africa</td>
      <td>Equatorial Guinea</td>
      <td>6615.252452</td>
      <td>1938431</td>
      <td>28.347697</td>
      <td>17.4</td>
      <td>3.5</td>
      <td>75.366352</td>
      <td>155.890360</td>
      <td>...</td>
      <td>5.049153</td>
      <td>7.996158</td>
      <td>1.392330e+05</td>
      <td>2.196798</td>
      <td>11.843911</td>
      <td>2.196798</td>
      <td>11.843911</td>
      <td>24.620100</td>
      <td>-7.220100</td>
      <td>Negative Deviant</td>
    </tr>
    <tr>
      <th>32</th>
      <td>BEN</td>
      <td>Africa</td>
      <td>Benin</td>
      <td>1658.273127</td>
      <td>14814460</td>
      <td>47.735147</td>
      <td>25.4</td>
      <td>5.6</td>
      <td>6.824348</td>
      <td>21.825727</td>
      <td>...</td>
      <td>3.083089</td>
      <td>9.618980</td>
      <td>2.380456e+05</td>
      <td>2.362643</td>
      <td>12.380222</td>
      <td>2.362643</td>
      <td>12.380222</td>
      <td>30.835455</td>
      <td>-5.435455</td>
      <td>Negative Deviant</td>
    </tr>
    <tr>
      <th>31</th>
      <td>ZMB</td>
      <td>Africa</td>
      <td>Zambia</td>
      <td>1317.877716</td>
      <td>21913874</td>
      <td>40.975737</td>
      <td>26.5</td>
      <td>5.5</td>
      <td>32.391680</td>
      <td>9.050693</td>
      <td>...</td>
      <td>2.202841</td>
      <td>1.638232</td>
      <td>1.208534e+05</td>
      <td>0.970109</td>
      <td>11.702342</td>
      <td>0.970109</td>
      <td>11.702342</td>
      <td>31.650620</td>
      <td>-5.150620</td>
      <td>Negative Deviant</td>
    </tr>
    <tr>
      <th>30</th>
      <td>CAF</td>
      <td>Africa</td>
      <td>Central African Republic</td>
      <td>556.131398</td>
      <td>5513282</td>
      <td>20.686009</td>
      <td>18.6</td>
      <td>7.6</td>
      <td>7.728513</td>
      <td>24.621847</td>
      <td>...</td>
      <td>3.203634</td>
      <td>362.959849</td>
      <td>1.589734e+06</td>
      <td>5.897044</td>
      <td>14.279078</td>
      <td>5.897044</td>
      <td>14.279078</td>
      <td>22.137904</td>
      <td>-3.537904</td>
      <td>Negative Deviant</td>
    </tr>
    <tr>
      <th>29</th>
      <td>AGO</td>
      <td>Africa</td>
      <td>Angola</td>
      <td>3129.476623</td>
      <td>39040039</td>
      <td>36.073211</td>
      <td>29.1</td>
      <td>6.0</td>
      <td>33.870996</td>
      <td>36.746709</td>
      <td>...</td>
      <td>3.604049</td>
      <td>369.582110</td>
      <td>3.470379e+05</td>
      <td>5.915075</td>
      <td>12.757192</td>
      <td>5.915075</td>
      <td>12.757192</td>
      <td>32.561479</td>
      <td>-3.461479</td>
      <td>Negative Deviant</td>
    </tr>
    <tr>
      <th>28</th>
      <td>TCD</td>
      <td>Africa</td>
      <td>Chad</td>
      <td>1022.335594</td>
      <td>21003705</td>
      <td>29.942323</td>
      <td>23.9</td>
      <td>7.9</td>
      <td>11.977701</td>
      <td>24.857761</td>
      <td>...</td>
      <td>3.213170</td>
      <td>55.309289</td>
      <td>2.483956e+05</td>
      <td>4.030860</td>
      <td>12.422782</td>
      <td>4.030860</td>
      <td>12.422782</td>
      <td>27.283488</td>
      <td>-3.383488</td>
      <td>Negative Deviant</td>
    </tr>
    <tr>
      <th>27</th>
      <td>BDI</td>
      <td>Africa</td>
      <td>Burundi</td>
      <td>233.823013</td>
      <td>14390003</td>
      <td>31.778105</td>
      <td>22.1</td>
      <td>6.4</td>
      <td>4.147275</td>
      <td>7.834755</td>
      <td>...</td>
      <td>2.058570</td>
      <td>171.473210</td>
      <td>8.926482e+05</td>
      <td>5.150242</td>
      <td>13.701949</td>
      <td>5.150242</td>
      <td>13.701949</td>
      <td>25.255597</td>
      <td>-3.155597</td>
      <td>Negative Deviant</td>
    </tr>
    <tr>
      <th>26</th>
      <td>TZA</td>
      <td>Africa</td>
      <td>Tanzania</td>
      <td>1318.819214</td>
      <td>70545865</td>
      <td>45.654061</td>
      <td>31.3</td>
      <td>5.6</td>
      <td>11.483156</td>
      <td>10.501752</td>
      <td>...</td>
      <td>2.351542</td>
      <td>2.187230</td>
      <td>6.769793e+04</td>
      <td>1.159152</td>
      <td>11.122826</td>
      <td>1.159152</td>
      <td>11.122826</td>
      <td>33.874168</td>
      <td>-2.574168</td>
      <td>Negative Deviant</td>
    </tr>
    <tr>
      <th>25</th>
      <td>CIV</td>
      <td>Africa</td>
      <td>Cote d'Ivoire</td>
      <td>3050.102023</td>
      <td>32711547</td>
      <td>45.201657</td>
      <td>31.2</td>
      <td>5.3</td>
      <td>35.797058</td>
      <td>41.508841</td>
      <td>...</td>
      <td>3.725906</td>
      <td>13.233859</td>
      <td>3.908487e+05</td>
      <td>2.655624</td>
      <td>12.876078</td>
      <td>2.655624</td>
      <td>12.876078</td>
      <td>33.147852</td>
      <td>-1.947852</td>
      <td>Negative Deviant</td>
    </tr>
  </tbody>
</table>
<p>10 rows × 30 columns</p>
</div>




```python
# ============================================================
# POLICY INTERPRETATION
# ============================================================

def interpret_gap(gap):

    if gap >= 5:
        return "Substantially exceeds expected preparedness"

    elif gap >= 2:
        return "Moderately exceeds expectations"

    elif gap <= -5:
        return "Substantially below expected preparedness"

    elif gap <= -2:
        return "Moderately below expectations"

    else:
        return "Preparedness broadly matches expectations"


positive_deviants["Policy Interpretation"] = (
    positive_deviants["Preparedness Gap"]
    .apply(interpret_gap)
)

negative_deviants["Policy Interpretation"] = (
    negative_deviants["Preparedness Gap"]
    .apply(interpret_gap)
)
```


```python
import os

output_folder = r"C:\DISASTER PREPARENEESS CAPACITY BUILDING PROJECT\DATA\AI OUTPUTS"

os.makedirs(output_folder, exist_ok=True)

positive_file = os.path.join(output_folder, "Positive_Deviants.csv")
negative_file = os.path.join(output_folder, "Negative_Deviants.csv")

positive_deviants.to_csv(positive_file, index=False)
negative_deviants.to_csv(negative_file, index=False)

print("Saved:")
print(positive_file)
print(negative_file)
```

    Saved:
    C:\DISASTER PREPARENEESS CAPACITY BUILDING PROJECT\DATA\AI OUTPUTS\Positive_Deviants.csv
    C:\DISASTER PREPARENEESS CAPACITY BUILDING PROJECT\DATA\AI OUTPUTS\Negative_Deviants.csv
    


```python
max_distance = peer_countries["Similarity Distance"].max()

peer_countries["AI Similarity Score"] = (
    (1 - peer_countries["Similarity Distance"] / max_distance) * 100
).round(0)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Cell In[135], line 1
    ----> 1 max_distance = peer_countries["Similarity Distance"].max()
          3 peer_countries["AI Similarity Score"] = (
          4     (1 - peer_countries["Similarity Distance"] / max_distance) * 100
          5 ).round(0)
    

    NameError: name 'peer_countries' is not defined



```python
# ============================================================
# VISUALISE K-MEANS CLUSTERS USING PCA
# ============================================================

from sklearn.decomposition import PCA
import matplotlib.pyplot as plt
import os

# Apply PCA to reduce to 2 dimensions
pca = PCA(n_components=2, random_state=42)
X_pca = pca.fit_transform(X_scaled)

# Create dataframe for plotting
plot_df = knn_df.copy()

plot_df["PC1"] = X_pca[:, 0]
plot_df["PC2"] = X_pca[:, 1]
plot_df["Cluster"] = cluster_labels.astype(str)

# Create figure
plt.figure(figsize=(10,8))

# Plot each cluster
for cluster in sorted(plot_df["Cluster"].unique()):
    cluster_data = plot_df[plot_df["Cluster"] == cluster]

    plt.scatter(
        cluster_data["PC1"],
        cluster_data["PC2"],
        s=60,
        alpha=0.75,
        label=f"Cluster {cluster}"
    )

# Highlight Sierra Leone
sl = plot_df[plot_df["COUNTRY_NAME"] == "Sierra Leone"]

plt.scatter(
    sl["PC1"],
    sl["PC2"],
    s=250,
    marker="*",
    edgecolors="black",
    linewidth=2,
    label="Sierra Leone"
)

plt.annotate(
    "Sierra Leone",
    (sl["PC1"].values[0], sl["PC2"].values[0]),
    xytext=(8,8),
    textcoords="offset points",
    fontsize=11,
    fontweight="bold"
)

plt.title(
    "Global Preparedness Clusters",
    fontsize=16,
    fontweight="bold"
)

plt.xlabel("Principal Component 1")
plt.ylabel("Principal Component 2")

plt.legend()
plt.grid(alpha=0.3)

plt.tight_layout()

# ============================================================
# SAVE FIGURE
# ============================================================

output_folder = r"C:\DISASTER PREPARENEESS CAPACITY BUILDING PROJECT\DATA\AI OUTPUTS"

os.makedirs(output_folder, exist_ok=True)

output_file = os.path.join(
    output_folder,
    "Global_Preparedness_Clusters.png"
)

plt.savefig(output_file, dpi=300, bbox_inches="tight")

print(f"Figure saved to:\n{output_file}")

plt.show()
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    Cell In[136], line 11
          9 # Apply PCA to reduce to 2 dimensions
         10 pca = PCA(n_components=2, random_state=42)
    ---> 11 X_pca = pca.fit_transform(X_scaled)
         13 # Create dataframe for plotting
         14 plot_df = knn_df.copy()
    

    NameError: name 'X_scaled' is not defined



```python
%who DataFrame
```

    X	 X_std	 best	 comparison	 corr	 correlation_matrix	 country_profile	 df	 df_clean	 
    drivers	 gap_table	 gini_df	 missing	 negative_deviants	 plot	 positive_deviants	 prepared	 preparedness_rank	 
    priority_framework	 report_cards	 sl	 sl_profile	 vif	 worst	 
    


```python
print(df.columns.tolist())
```

    ['ISO3', 'REGION', 'COUNTRY_NAME', 'GDP_PER_CAPITA', 'TOTAL_POPULATION', 'WGI_SCORE', 'GHSI', 'INFORM_RISK_SCORE', 'GOV_HEALTH_EXPENDITURE', 'PRIVATE_HEALTH_EXPENDITURE', 'GINI', 'DISASTER COUNT', 'DEATHS FROM DISASTER COUNT', 'TOTAL PEOPLE AFFECTED BY DISASTER']
    


```python
import matplotlib.pyplot as plt
import numpy as np
from scipy.stats import pearsonr
import os

# ======================================
# VARIABLES
# ======================================

x = df["WGI_SCORE"]
y = df["GHSI"]

# ======================================
# CREATE FIGURE
# ======================================

plt.figure(figsize=(8,6))

# Plot all countries
plt.scatter(
    x,
    y,
    color="Black",
    s=40,
    alpha=0.7
)

# ======================================
# REGRESSION LINE
# ======================================

m, b = np.polyfit(x, y, 1)

plt.plot(
    x,
    m*x + b,
    color="Red",
    linewidth=2
)

# ======================================
# CORRELATION
# ======================================

r, p = pearsonr(x, y)

# ======================================
# HIGHLIGHT SIERRA LEONE
# ======================================

sl = df[df["COUNTRY_NAME"] == "Sierra Leone"]

plt.scatter(
    sl["WGI_SCORE"],
    sl["GHSI"],
    color="Green",
    marker="*",
    s=250,
    edgecolor="Green",
    label="Sierra Leone"
)

plt.annotate(
    "Sierra Leone",
    (
        sl["WGI_SCORE"].iloc[0],
        sl["GHSI"].iloc[0]
    ),
    xytext=(8,8),
    textcoords="offset points",
    fontsize=10,
    fontweight="bold"
)

# ======================================
# LABELS
# ======================================

plt.title(
    "Government Effectiveness vs Health Security & Preparedness",
    fontsize=14,
    weight="bold"
)

plt.xlabel("Government Effectiveness")
plt.ylabel("Health Security & Preparedness (GHSI)")

plt.grid(alpha=0.25)

plt.legend(frameon=False)

plt.text(
    0.02,
    0.96,
    f"r = {r:.2f}",
    transform=plt.gca().transAxes,
    fontsize=11,
    bbox=dict(facecolor="white", alpha=0.8)
)

plt.tight_layout()

# ======================================
# SAVE
# ======================================

output_folder = r"C:\DISASTER PREPARENEESS CAPACITY BUILDING PROJECT\DATA\AI OUTPUTS"

os.makedirs(output_folder, exist_ok=True)

output_file = os.path.join(
    output_folder,
    "Government_Effectiveness_vs_Preparedness.png"
)

plt.savefig(
    output_file,
    dpi=600,
    bbox_inches="tight"
)

print(f"Saved to:\n{output_file}")

plt.show()
```

    Saved to:
    C:\DISASTER PREPARENEESS CAPACITY BUILDING PROJECT\DATA\AI OUTPUTS\Government_Effectiveness_vs_Preparedness.png
    


    
![png](output_140_1.png)
    



```python
fig = plt.gcf()

fig.savefig(
    r"C:\DISASTER PREPARENEESS CAPACITY BUILDING PROJECT\DATA\AI OUTPUTS\Government_Effectiveness_vs_Preparedness.png",
    dpi=600,
    bbox_inches="tight",
    facecolor="white"
)
```


    <Figure size 640x480 with 0 Axes>



```python

```
