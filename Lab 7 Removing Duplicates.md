<p style="text-align:center">
    <a href="https://skills.network" target="_blank">
    <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/assets/logos/SN_web_lightmode.png" width="200" alt="Skills Network Logo"  />
    </a>
</p>


# **Removing Duplicates**


Estimated time needed: **30** minutes


## Introduction


In this lab, you will focus on data wrangling, an important step in preparing data for analysis. Data wrangling involves cleaning and organizing data to make it suitable for analysis. One key task in this process is removing duplicate entries, which are repeated entries that can distort analysis and lead to inaccurate conclusions.  


## Objectives


In this lab you will perform the following:


1. Identify duplicate rows  in the dataset.
2. Use suitable techniques to remove duplicate rows and verify the removal.
3. Summarize how to handle missing values appropriately.
4. Use ConvertedCompYearly to normalize compensation data.
   


### Install the Required Libraries



```python
!pip install pandas
!pip install matplotlib
!pip install scikit-learn
```

    Requirement already satisfied: pandas in /opt/conda/lib/python3.11/site-packages (2.2.3)
    Requirement already satisfied: numpy>=1.23.2 in /opt/conda/lib/python3.11/site-packages (from pandas) (2.2.1)
    Requirement already satisfied: python-dateutil>=2.8.2 in /opt/conda/lib/python3.11/site-packages (from pandas) (2.9.0)
    Requirement already satisfied: pytz>=2020.1 in /opt/conda/lib/python3.11/site-packages (from pandas) (2024.1)
    Requirement already satisfied: tzdata>=2022.7 in /opt/conda/lib/python3.11/site-packages (from pandas) (2024.2)
    Requirement already satisfied: six>=1.5 in /opt/conda/lib/python3.11/site-packages (from python-dateutil>=2.8.2->pandas) (1.16.0)
    Requirement already satisfied: matplotlib in /opt/conda/lib/python3.11/site-packages (3.10.0)
    Requirement already satisfied: contourpy>=1.0.1 in /opt/conda/lib/python3.11/site-packages (from matplotlib) (1.3.1)
    Requirement already satisfied: cycler>=0.10 in /opt/conda/lib/python3.11/site-packages (from matplotlib) (0.12.1)
    Requirement already satisfied: fonttools>=4.22.0 in /opt/conda/lib/python3.11/site-packages (from matplotlib) (4.55.3)
    Requirement already satisfied: kiwisolver>=1.3.1 in /opt/conda/lib/python3.11/site-packages (from matplotlib) (1.4.8)
    Requirement already satisfied: numpy>=1.23 in /opt/conda/lib/python3.11/site-packages (from matplotlib) (2.2.1)
    Requirement already satisfied: packaging>=20.0 in /opt/conda/lib/python3.11/site-packages (from matplotlib) (24.0)
    Requirement already satisfied: pillow>=8 in /opt/conda/lib/python3.11/site-packages (from matplotlib) (11.1.0)
    Requirement already satisfied: pyparsing>=2.3.1 in /opt/conda/lib/python3.11/site-packages (from matplotlib) (3.2.1)
    Requirement already satisfied: python-dateutil>=2.7 in /opt/conda/lib/python3.11/site-packages (from matplotlib) (2.9.0)
    Requirement already satisfied: six>=1.5 in /opt/conda/lib/python3.11/site-packages (from python-dateutil>=2.7->matplotlib) (1.16.0)
    Collecting scikit-learn
      Downloading scikit_learn-1.6.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (18 kB)
    Requirement already satisfied: numpy>=1.19.5 in /opt/conda/lib/python3.11/site-packages (from scikit-learn) (2.2.1)
    Collecting scipy>=1.6.0 (from scikit-learn)
      Downloading scipy-1.15.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (61 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m62.0/62.0 kB[0m [31m8.0 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting joblib>=1.2.0 (from scikit-learn)
      Downloading joblib-1.4.2-py3-none-any.whl.metadata (5.4 kB)
    Collecting threadpoolctl>=3.1.0 (from scikit-learn)
      Downloading threadpoolctl-3.5.0-py3-none-any.whl.metadata (13 kB)
    Downloading scikit_learn-1.6.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (13.5 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m13.5/13.5 MB[0m [31m107.7 MB/s[0m eta [36m0:00:00[0m00:01[0m0:01[0m
    [?25hDownloading joblib-1.4.2-py3-none-any.whl (301 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m301.8/301.8 kB[0m [31m34.6 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading scipy-1.15.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (40.6 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m40.6/40.6 MB[0m [31m60.9 MB/s[0m eta [36m0:00:00[0m:00:01[0m00:01[0m
    [?25hDownloading threadpoolctl-3.5.0-py3-none-any.whl (18 kB)
    Installing collected packages: threadpoolctl, scipy, joblib, scikit-learn
    Successfully installed joblib-1.4.2 scikit-learn-1.6.0 scipy-1.15.0 threadpoolctl-3.5.0


### Step 1: Import Required Libraries



```python
import pandas as pd
import sklearn
print(sklearn.__version__)

```

    1.6.0


### Step 2: Load the Dataset into a DataFrame



load the dataset using pd.read_csv()



```python
# Load the dataset directly from the URL
file_path = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/UDKAZw-kz18Yj8P6icf_qw/survey-data-duplicates.csv"
df = pd.read_csv(file_path)

# Display the first few rows
df.head()

# Display the first few rows to ensure it loaded correctly
#df.head()

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
      <th>ResponseId</th>
      <th>MainBranch</th>
      <th>Age</th>
      <th>Employment</th>
      <th>RemoteWork</th>
      <th>Check</th>
      <th>CodingActivities</th>
      <th>EdLevel</th>
      <th>LearnCode</th>
      <th>LearnCodeOnline</th>
      <th>...</th>
      <th>JobSatPoints_6</th>
      <th>JobSatPoints_7</th>
      <th>JobSatPoints_8</th>
      <th>JobSatPoints_9</th>
      <th>JobSatPoints_10</th>
      <th>JobSatPoints_11</th>
      <th>SurveyLength</th>
      <th>SurveyEase</th>
      <th>ConvertedCompYearly</th>
      <th>JobSat</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>I am a developer by profession</td>
      <td>Under 18 years old</td>
      <td>Employed, full-time</td>
      <td>Remote</td>
      <td>Apples</td>
      <td>Hobby</td>
      <td>Primary/elementary school</td>
      <td>Books / Physical media</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>I am a developer by profession</td>
      <td>35-44 years old</td>
      <td>Employed, full-time</td>
      <td>Remote</td>
      <td>Apples</td>
      <td>Hobby;Contribute to open-source projects;Other...</td>
      <td>Bachelor’s degree (B.A., B.S., B.Eng., etc.)</td>
      <td>Books / Physical media;Colleague;On the job tr...</td>
      <td>Technical documentation;Blogs;Books;Written Tu...</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>I am a developer by profession</td>
      <td>45-54 years old</td>
      <td>Employed, full-time</td>
      <td>Remote</td>
      <td>Apples</td>
      <td>Hobby;Contribute to open-source projects;Other...</td>
      <td>Master’s degree (M.A., M.S., M.Eng., MBA, etc.)</td>
      <td>Books / Physical media;Colleague;On the job tr...</td>
      <td>Technical documentation;Blogs;Books;Written Tu...</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Appropriate in length</td>
      <td>Easy</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>I am learning to code</td>
      <td>18-24 years old</td>
      <td>Student, full-time</td>
      <td>NaN</td>
      <td>Apples</td>
      <td>NaN</td>
      <td>Some college/university study without earning ...</td>
      <td>Other online resources (e.g., videos, blogs, f...</td>
      <td>Stack Overflow;How-to videos;Interactive tutorial</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Too long</td>
      <td>Easy</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>I am a developer by profession</td>
      <td>18-24 years old</td>
      <td>Student, full-time</td>
      <td>NaN</td>
      <td>Apples</td>
      <td>NaN</td>
      <td>Secondary school (e.g. American high school, G...</td>
      <td>Other online resources (e.g., videos, blogs, f...</td>
      <td>Technical documentation;Blogs;Written Tutorial...</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Too short</td>
      <td>Easy</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 114 columns</p>
</div>



**Note: If you are working on a local Jupyter environment, you can use the URL directly in the <code>pandas.read_csv()</code>  function as shown below:**



df = pd.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/n01PQ9pSmiRX6520flujwQ/survey-data.csv")
df.head()

### Step 3: Identifying Duplicate Rows


**Task 1: Identify Duplicate Rows**
  1. Count the number of duplicate rows in the dataset.
  2. Display the first few duplicate rows to understand their structure.



```python
df.shape
```




    (65447, 114)




```python
#Count the number of duplicate rows in the dataset.
duplicate_count = df.duplicated().sum()
print('Number of duplicated rows: ', duplicate_count)

#Display the first few duplicate rows to understand their structure.
duplicate_rows = df[df.duplicated()]
duplicate_count, duplicate_rows.head(1)
```

    Number of duplicated rows:  10





    (np.int64(10),
            ResponseId                      MainBranch                 Age  \
     65437           1  I am a developer by profession  Under 18 years old   
     
                     Employment RemoteWork   Check CodingActivities  \
     65437  Employed, full-time     Remote  Apples            Hobby   
     
                              EdLevel               LearnCode LearnCodeOnline  ...  \
     65437  Primary/elementary school  Books / Physical media             NaN  ...   
     
           JobSatPoints_6 JobSatPoints_7 JobSatPoints_8 JobSatPoints_9  \
     65437            NaN            NaN            NaN            NaN   
     
           JobSatPoints_10 JobSatPoints_11 SurveyLength SurveyEase  \
     65437             NaN             NaN          NaN        NaN   
     
           ConvertedCompYearly JobSat  
     65437                 NaN    NaN  
     
     [1 rows x 114 columns])



### Step 4: Removing Duplicate Rows


**Task 2: Remove Duplicates**
   1. Remove duplicate rows from the dataset using the drop_duplicates() function.
2. Verify the removal by counting the number of duplicate rows after removal .



```python
df.drop_duplicates(inplace=True)
duplicate_count = df.duplicated().sum()
print('Check duplicated rows: ', duplicate_count)
```

    Check duplicated rows:  0


### Step 5: Handling Missing Values


**Task 3: Identify and Handle Missing Values**
   1. Identify missing values for all columns in the dataset.
   2. Choose a column with significant missing values (e.g., EdLevel) and impute with the most frequent value.



```python
missing_values = df.isnull().sum()
missing_percentage = (missing_values / len(df)) * 100

# Crear una tabla
missing_table = pd.DataFrame({
    'Column': missing_values.index,
    'Missing Values': missing_values.values,
    'Percentage (%)': missing_percentage.values
})

# Ordenar la tabla por valores faltantes de forma descendente
missing_table = missing_table.sort_values(by='Missing Values', ascending=False).reset_index(drop=True)

# Mostrar la tabla
missing_table.head(5)
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
      <th>Column</th>
      <th>Missing Values</th>
      <th>Percentage (%)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AINextMuch less integrated</td>
      <td>64289</td>
      <td>98.245641</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AINextLess integrated</td>
      <td>63082</td>
      <td>96.401119</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AINextNo change</td>
      <td>52939</td>
      <td>80.900714</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AINextMuch more integrated</td>
      <td>51999</td>
      <td>79.464217</td>
    </tr>
    <tr>
      <th>4</th>
      <td>EmbeddedAdmired</td>
      <td>48704</td>
      <td>74.428840</td>
    </tr>
  </tbody>
</table>
</div>




```python
significant_missing = missing_table[missing_table['Percentage (%)'] > 10]
significant_missing = significant_missing.sort_values(by='Missing Values', ascending=False).reset_index(drop=True)

column_to_impute = significant_missing.loc[0, 'Column']

most_frequent_value = df[column_to_impute].mode()[0]
df[column_to_impute] = df[column_to_impute].fillna(most_frequent_value)

print('Number of values null is: ', df[column_to_impute].isnull().sum())
```

    Number of values null is:  0


### Step 6: Normalizing Compensation Data


**Task 4: Normalize Compensation Data Using ConvertedCompYearly**
   1. Use the ConvertedCompYearly column for compensation analysis as the normalized annual compensation is already provided.
   2. Check for missing values in ConvertedCompYearly and handle them if necessary.



```python
missing_values = df['ConvertedCompYearly'].isnull().sum()
print('Missing values in ConvertedCompYearly: ', missing_values)
```

    Missing values in ConvertedCompYearly:  42002



```python
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()
df['ConvertedCompYearly_Normalized'] = scaler.fit_transform(df[['ConvertedCompYearly']])
print("Values after normalization:")
print(df['ConvertedCompYearly_Normalized'].describe())
```

    Values after normalization:
    count    23435.000000
    mean         0.005300
    std          0.011488
    min          0.000000
    25%          0.002012
    50%          0.003998
    75%          0.006642
    max          1.000000
    Name: ConvertedCompYearly_Normalized, dtype: float64



```python
print('Missing values in ConvertedCompYearly: ', missing_values)
```

    Missing values in ConvertedCompYearly:  0


### Step 7: Summary and Next Steps


**In this lab, you focused on identifying and removing duplicate rows.**

- You handled missing values by imputing the most frequent value in a chosen column.

- You used ConvertedCompYearly for compensation normalization and handled missing values.

- For further analysis, consider exploring other columns or visualizing the cleaned dataset.



```python

```

<!--
## Change Log

|Date (YYYY-MM-DD)|Version|Changed By|Change Description|
|-|-|-|-|
|2024-11-05|1.2|Madhusudhan Moole|Updated lab|
|2024-09-24|1.1|Madhusudhan Moole|Updated lab|
|2024-09-23|1.0|Raghul Ramesh|Created lab|

--!>


Copyright © IBM Corporation. All rights reserved.

