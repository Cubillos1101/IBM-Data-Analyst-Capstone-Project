<p style="text-align:center">
    <a href="https://skills.network" target="_blank">
    <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/assets/logos/SN_web_lightmode.png" width="200" alt="Skills Network Logo"  />
    </a>
</p>


# **Finding Missing Values**


Estimated time needed: **30** minutes


Data wrangling is the process of cleaning, transforming, and organizing data to make it suitable for analysis. Finding and handling missing values is a crucial step in this process to ensure data accuracy and completeness. In this lab, you will focus exclusively on identifying and handling missing values in the dataset.


## Objectives


After completing this lab, you will be able to:


-   Identify missing values in the dataset.

- Quantify missing values for specific columns.

- Impute missing values using various strategies.


## Hands on Lab


##### Setup: Install Required Libraries



```python
!pip install pandas
!pip install matplotlib
!pip install seaborn
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
    Requirement already satisfied: seaborn in /opt/conda/lib/python3.11/site-packages (0.13.2)
    Requirement already satisfied: numpy!=1.24.0,>=1.20 in /opt/conda/lib/python3.11/site-packages (from seaborn) (2.2.1)
    Requirement already satisfied: pandas>=1.2 in /opt/conda/lib/python3.11/site-packages (from seaborn) (2.2.3)
    Requirement already satisfied: matplotlib!=3.6.1,>=3.4 in /opt/conda/lib/python3.11/site-packages (from seaborn) (3.10.0)
    Requirement already satisfied: contourpy>=1.0.1 in /opt/conda/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (1.3.1)
    Requirement already satisfied: cycler>=0.10 in /opt/conda/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (0.12.1)
    Requirement already satisfied: fonttools>=4.22.0 in /opt/conda/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (4.55.3)
    Requirement already satisfied: kiwisolver>=1.3.1 in /opt/conda/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (1.4.8)
    Requirement already satisfied: packaging>=20.0 in /opt/conda/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (24.0)
    Requirement already satisfied: pillow>=8 in /opt/conda/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (11.1.0)
    Requirement already satisfied: pyparsing>=2.3.1 in /opt/conda/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (3.2.1)
    Requirement already satisfied: python-dateutil>=2.7 in /opt/conda/lib/python3.11/site-packages (from matplotlib!=3.6.1,>=3.4->seaborn) (2.9.0)
    Requirement already satisfied: pytz>=2020.1 in /opt/conda/lib/python3.11/site-packages (from pandas>=1.2->seaborn) (2024.1)
    Requirement already satisfied: tzdata>=2022.7 in /opt/conda/lib/python3.11/site-packages (from pandas>=1.2->seaborn) (2024.2)
    Requirement already satisfied: six>=1.5 in /opt/conda/lib/python3.11/site-packages (from python-dateutil>=2.7->matplotlib!=3.6.1,>=3.4->seaborn) (1.16.0)


##### Import Necessary Modules:



```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

## Tasks


<h2>1. Load the Dataset</h2>
<p>
We use the <code>pandas.read_csv()</code> function for reading CSV files. However, in this version of the lab, which operates on JupyterLite, the dataset needs to be downloaded to the interface using the provided code below.
</p>


The functions below will download the dataset into your browser:




```python
# Define the URL of the dataset
file_path = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/n01PQ9pSmiRX6520flujwQ/survey-data.csv"

# Load the dataset into a DataFrame
df = pd.read_csv(file_path)

# Display the first few rows to ensure it loaded correctly
print(df.head())

```

       ResponseId                      MainBranch                 Age  \
    0           1  I am a developer by profession  Under 18 years old   
    1           2  I am a developer by profession     35-44 years old   
    2           3  I am a developer by profession     45-54 years old   
    3           4           I am learning to code     18-24 years old   
    4           5  I am a developer by profession     18-24 years old   
    
                Employment RemoteWork   Check  \
    0  Employed, full-time     Remote  Apples   
    1  Employed, full-time     Remote  Apples   
    2  Employed, full-time     Remote  Apples   
    3   Student, full-time        NaN  Apples   
    4   Student, full-time        NaN  Apples   
    
                                        CodingActivities  \
    0                                              Hobby   
    1  Hobby;Contribute to open-source projects;Other...   
    2  Hobby;Contribute to open-source projects;Other...   
    3                                                NaN   
    4                                                NaN   
    
                                                 EdLevel  \
    0                          Primary/elementary school   
    1       Bachelor’s degree (B.A., B.S., B.Eng., etc.)   
    2    Master’s degree (M.A., M.S., M.Eng., MBA, etc.)   
    3  Some college/university study without earning ...   
    4  Secondary school (e.g. American high school, G...   
    
                                               LearnCode  \
    0                             Books / Physical media   
    1  Books / Physical media;Colleague;On the job tr...   
    2  Books / Physical media;Colleague;On the job tr...   
    3  Other online resources (e.g., videos, blogs, f...   
    4  Other online resources (e.g., videos, blogs, f...   
    
                                         LearnCodeOnline  ... JobSatPoints_6  \
    0                                                NaN  ...            NaN   
    1  Technical documentation;Blogs;Books;Written Tu...  ...            0.0   
    2  Technical documentation;Blogs;Books;Written Tu...  ...            NaN   
    3  Stack Overflow;How-to videos;Interactive tutorial  ...            NaN   
    4  Technical documentation;Blogs;Written Tutorial...  ...            NaN   
    
      JobSatPoints_7 JobSatPoints_8 JobSatPoints_9 JobSatPoints_10  \
    0            NaN            NaN            NaN             NaN   
    1            0.0            0.0            0.0             0.0   
    2            NaN            NaN            NaN             NaN   
    3            NaN            NaN            NaN             NaN   
    4            NaN            NaN            NaN             NaN   
    
      JobSatPoints_11           SurveyLength SurveyEase ConvertedCompYearly JobSat  
    0             NaN                    NaN        NaN                 NaN    NaN  
    1             0.0                    NaN        NaN                 NaN    NaN  
    2             NaN  Appropriate in length       Easy                 NaN    NaN  
    3             NaN               Too long       Easy                 NaN    NaN  
    4             NaN              Too short       Easy                 NaN    NaN  
    
    [5 rows x 114 columns]


### 2. Explore the Dataset
##### Task 1: Display basic information and summary statistics of the dataset.



```python
df.describe(include='all')
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
      <th>count</th>
      <td>65437.000000</td>
      <td>65437</td>
      <td>65437</td>
      <td>65437</td>
      <td>54806</td>
      <td>65437</td>
      <td>54466</td>
      <td>60784</td>
      <td>60488</td>
      <td>49237</td>
      <td>...</td>
      <td>29450.000000</td>
      <td>29448.00000</td>
      <td>29456.000000</td>
      <td>29456.000000</td>
      <td>29450.000000</td>
      <td>29445.000000</td>
      <td>56182</td>
      <td>56238</td>
      <td>2.343500e+04</td>
      <td>29126.000000</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>NaN</td>
      <td>5</td>
      <td>8</td>
      <td>110</td>
      <td>3</td>
      <td>1</td>
      <td>118</td>
      <td>8</td>
      <td>418</td>
      <td>10853</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3</td>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>top</th>
      <td>NaN</td>
      <td>I am a developer by profession</td>
      <td>25-34 years old</td>
      <td>Employed, full-time</td>
      <td>Hybrid (some remote, some in-person)</td>
      <td>Apples</td>
      <td>Hobby</td>
      <td>Bachelor’s degree (B.A., B.S., B.Eng., etc.)</td>
      <td>Other online resources (e.g., videos, blogs, f...</td>
      <td>Technical documentation;Blogs;Written Tutorial...</td>
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
      <th>freq</th>
      <td>NaN</td>
      <td>50207</td>
      <td>23911</td>
      <td>39041</td>
      <td>23015</td>
      <td>65437</td>
      <td>9993</td>
      <td>24942</td>
      <td>3674</td>
      <td>603</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>38767</td>
      <td>30071</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>32719.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>24.343232</td>
      <td>22.96522</td>
      <td>20.278165</td>
      <td>16.169432</td>
      <td>10.955713</td>
      <td>9.953948</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.615529e+04</td>
      <td>6.935041</td>
    </tr>
    <tr>
      <th>std</th>
      <td>18890.179119</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>27.089360</td>
      <td>27.01774</td>
      <td>26.108110</td>
      <td>24.845032</td>
      <td>22.906263</td>
      <td>21.775652</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.867570e+05</td>
      <td>2.088259</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.000000e+00</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>16360.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.271200e+04</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>32719.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>20.000000</td>
      <td>15.00000</td>
      <td>10.000000</td>
      <td>5.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.500000e+04</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>49078.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>30.000000</td>
      <td>30.00000</td>
      <td>25.000000</td>
      <td>20.000000</td>
      <td>10.000000</td>
      <td>10.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.079715e+05</td>
      <td>8.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>65437.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>100.000000</td>
      <td>100.00000</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.625660e+07</td>
      <td>10.000000</td>
    </tr>
  </tbody>
</table>
<p>11 rows × 114 columns</p>
</div>



### 3. Finding Missing Values
##### Task 2: Identify missing values for all columns.



```python
missing_values = df.isnull().sum()
missing_percentage = (missing_values / len(df)) * 100

missing_table = pd.DataFrame({
    'Column': missing_values.index,
    'Missing Values': missing_values.values,
    'Percentage (%)': missing_percentage.values
})

missing_table = missing_table.sort_values(by='Missing Values', ascending=False).reset_index(drop=True)

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



##### Task 3: Visualize missing values using a heatmap (Using seaborn library).




```python
plt.figure(figsize=(12, 8))
sns.heatmap(df.isnull(), cbar=False, cmap="coolwarm", yticklabels=False)

plt.title("Heatmap of Missing Values")

plt.show()
```


    
![png](output_21_0.png)
    


##### Task 4: Count the number of missing rows for a specific column (e.g., `Employment`).



```python
columns_to_check = ['Age', 'Employment', 'DevType','Country']
missing_values_in_columns = df[columns_to_check].isnull().sum()

print("Missing values in specified columns:")
print(missing_values_in_columns)
```

    Missing values in specified columns:
    Age              0
    Employment       0
    DevType       5992
    Country       6507
    dtype: int64


### 4. Imputing Missing Values
##### Task 5: Identify the most frequent (majority) value in a specific column (e.g., `Employment`).



```python
most_frequent_values = df[columns_to_check].mode().iloc[0]

print("Most frequent values in specified columns:")
print(most_frequent_values)
```

    Most frequent values in specified columns:
    Age                    25-34 years old
    Employment         Employed, full-time
    DevType          Developer, full-stack
    Country       United States of America
    Name: 0, dtype: object


##### Task 6: Impute missing values in the `Employment` column with the most frequent value.




```python
df['Employment'] = df['Employment'].fillna(most_frequent_values)

# Verify if the missing values were imputed
print(f"Missing values in 'Employment' column after imputation: {df['Employment'].isnull().sum()}")
```

    Missing values in 'Employment' column after imputation: 0


##### Impute missing values in the `Country` column with the most frequent value.


```python
df['Country'] = df['Country'].fillna(most_frequent_values)

# Verify if the missing values were imputed
print(f"Missing values in 'Employment' column after imputation: {df['Country'].isnull().sum()}")
```

    Missing values in 'Employment' column after imputation: 6507


##### Impute missing values in the `DevType` column with the most frequent value.


```python
df['DevType'] = df['DevType'].fillna(most_frequent_values)

# Verify if the missing values were imputed
print(f"Missing values in 'Employment' column after imputation: {df['DevType'].isnull().sum()}")
```

    Missing values in 'Employment' column after imputation: 5992


### 5. Visualizing Imputed Data
##### Task 7: Visualize the distribution of a column after imputation (e.g., `Employment`).



```python
top_10_employment = df['Employment'].value_counts().head(10)
top_10_devtype = df['DevType'].value_counts().head(10)
top_10_country = df['Country'].value_counts().head(10)

plt.figure(figsize=(10, 6))
sns.countplot(x='Employment', data=df[df['Employment'].isin(top_10_employment.index)], palette='viridis',hue='Employment',legend=False)
plt.title('Top 10 Most Frequent Employment Categories')
plt.xlabel('Employment')
plt.ylabel('Frequency')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()

plt.figure(figsize=(10, 6))
sns.countplot(x='DevType', data=df[df['DevType'].isin(top_10_devtype.index)], palette='Set2',hue='DevType',legend=False)
plt.title('Top 10 Most Frequent DevTypes')
plt.xlabel('DevType')
plt.ylabel('Frequency')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()

plt.figure(figsize=(10, 6))
sns.countplot(x='Country', data=df[df['Country'].isin(top_10_country.index)], palette='muted',hue='Country',legend=False)
plt.title('Top 10 Most Frequent Countries')
plt.xlabel('Country')
plt.ylabel('Frequency')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()
```


    
![png](output_33_0.png)
    



    
![png](output_33_1.png)
    



    
![png](output_33_2.png)
    


### Summary


In this lab, you:
- Loaded the dataset into a pandas DataFrame.
- Identified missing values across all columns.
- Quantified missing values in specific columns.
- Imputed missing values in a categorical column using the most frequent value.
- Visualized the imputed data for better understanding.
  


Copyright © IBM Corporation. All rights reserved.

