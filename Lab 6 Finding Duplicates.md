<p style="text-align:center">
    <a href="https://skills.network" target="_blank">
    <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/assets/logos/SN_web_lightmode.png" width="200" alt="Skills Network Logo"  />
    </a>
</p>


# **Finding Duplicates Lab**


Estimated time needed: **30** minutes


## Introduction


Data wrangling is a critical step in preparing datasets for analysis, and handling duplicates plays a key role in ensuring data accuracy. In this lab, you will focus on identifying and removing duplicate entries from your dataset. 


## Objectives


In this lab, you will perform the following:


1. Identify duplicate rows in the dataset and analyze their characteristics.
2. Visualize the distribution of duplicates based on key attributes.
3. Remove duplicate values strategically based on specific criteria.
4. Outline the process of verifying and documenting duplicate removal.


## Hands on Lab


Install the needed library



```python
!pip install pandas
!pip install matplotlib
```

    Collecting pandas
      Downloading pandas-2.2.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (89 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m89.9/89.9 kB[0m [31m11.1 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting numpy>=1.23.2 (from pandas)
      Downloading numpy-2.2.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (62 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m62.0/62.0 kB[0m [31m5.5 MB/s[0m eta [36m0:00:00[0m
    [?25hRequirement already satisfied: python-dateutil>=2.8.2 in /opt/conda/lib/python3.11/site-packages (from pandas) (2.9.0)
    Requirement already satisfied: pytz>=2020.1 in /opt/conda/lib/python3.11/site-packages (from pandas) (2024.1)
    Collecting tzdata>=2022.7 (from pandas)
      Downloading tzdata-2024.2-py2.py3-none-any.whl.metadata (1.4 kB)
    Requirement already satisfied: six>=1.5 in /opt/conda/lib/python3.11/site-packages (from python-dateutil>=2.8.2->pandas) (1.16.0)
    Downloading pandas-2.2.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (13.1 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m13.1/13.1 MB[0m [31m108.8 MB/s[0m eta [36m0:00:00[0m00:01[0m00:01[0m
    [?25hDownloading numpy-2.2.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (16.4 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m16.4/16.4 MB[0m [31m97.0 MB/s[0m eta [36m0:00:00[0m:00:01[0m00:01[0m
    [?25hDownloading tzdata-2024.2-py2.py3-none-any.whl (346 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m346.6/346.6 kB[0m [31m34.6 MB/s[0m eta [36m0:00:00[0m
    [?25hInstalling collected packages: tzdata, numpy, pandas
    Successfully installed numpy-2.2.1 pandas-2.2.3 tzdata-2024.2
    Collecting matplotlib
      Downloading matplotlib-3.10.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (11 kB)
    Collecting contourpy>=1.0.1 (from matplotlib)
      Downloading contourpy-1.3.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (5.4 kB)
    Collecting cycler>=0.10 (from matplotlib)
      Downloading cycler-0.12.1-py3-none-any.whl.metadata (3.8 kB)
    Collecting fonttools>=4.22.0 (from matplotlib)
      Downloading fonttools-4.55.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (165 kB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m165.1/165.1 kB[0m [31m20.4 MB/s[0m eta [36m0:00:00[0m
    [?25hCollecting kiwisolver>=1.3.1 (from matplotlib)
      Downloading kiwisolver-1.4.8-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (6.2 kB)
    Requirement already satisfied: numpy>=1.23 in /opt/conda/lib/python3.11/site-packages (from matplotlib) (2.2.1)
    Requirement already satisfied: packaging>=20.0 in /opt/conda/lib/python3.11/site-packages (from matplotlib) (24.0)
    Collecting pillow>=8 (from matplotlib)
      Downloading pillow-11.1.0-cp311-cp311-manylinux_2_28_x86_64.whl.metadata (9.1 kB)
    Collecting pyparsing>=2.3.1 (from matplotlib)
      Downloading pyparsing-3.2.1-py3-none-any.whl.metadata (5.0 kB)
    Requirement already satisfied: python-dateutil>=2.7 in /opt/conda/lib/python3.11/site-packages (from matplotlib) (2.9.0)
    Requirement already satisfied: six>=1.5 in /opt/conda/lib/python3.11/site-packages (from python-dateutil>=2.7->matplotlib) (1.16.0)
    Downloading matplotlib-3.10.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (8.6 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m8.6/8.6 MB[0m [31m129.4 MB/s[0m eta [36m0:00:00[0ma [36m0:00:01[0m
    [?25hDownloading contourpy-1.3.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (326 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m326.2/326.2 kB[0m [31m37.4 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading cycler-0.12.1-py3-none-any.whl (8.3 kB)
    Downloading fonttools-4.55.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (4.9 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m4.9/4.9 MB[0m [31m117.1 MB/s[0m eta [36m0:00:00[0m00:01[0m
    [?25hDownloading kiwisolver-1.4.8-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.4 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m1.4/1.4 MB[0m [31m81.9 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading pillow-11.1.0-cp311-cp311-manylinux_2_28_x86_64.whl (4.5 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m4.5/4.5 MB[0m [31m115.5 MB/s[0m eta [36m0:00:00[0m00:01[0m
    [?25hDownloading pyparsing-3.2.1-py3-none-any.whl (107 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m107.7/107.7 kB[0m [31m13.8 MB/s[0m eta [36m0:00:00[0m
    [?25hInstalling collected packages: pyparsing, pillow, kiwisolver, fonttools, cycler, contourpy, matplotlib
    Successfully installed contourpy-1.3.1 cycler-0.12.1 fonttools-4.55.3 kiwisolver-1.4.8 matplotlib-3.10.0 pillow-11.1.0 pyparsing-3.2.1


Import pandas module



```python
import pandas as pd

```

Import matplotlib



```python
import matplotlib.pyplot as plt

```

## **Load the dataset into a dataframe**


<h2>Read Data</h2>
<p>
We utilize the <code>pandas.read_csv()</code> function for reading CSV files. However, in this version of the lab, which operates on JupyterLite, the dataset needs to be downloaded to the interface using the provided code below.
</p>



```python
# Load the dataset directly from the URL
file_path = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/UDKAZw-kz18Yj8P6icf_qw/survey-data-duplicates.csv"
df = pd.read_csv(file_path)

# Display the first few rows
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



Load the data into a pandas dataframe:



Note: If you are working on a local Jupyter environment, you can use the URL directly in the pandas.read_csv() function as shown below:




```python
# df = pd.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/n01PQ9pSmiRX6520flujwQ/survey-data.csv")

```

## Identify and Analyze Duplicates


### Task 1: Identify Duplicate Rows
1. Count the number of duplicate rows in the dataset.
3. Display the first few duplicate rows to understand their structure.



```python
#Count the number of duplicate rows in the dataset.
duplicate_count = df.duplicated().sum()
print('Number of duplicated rows: ', duplicate_count)

#Display the first few duplicate rows to understand their structure.
duplicate_rows = df[df.duplicated()]
duplicate_count, duplicate_rows.head()
```

    Number of duplicated rows:  10





    (np.int64(10),
            ResponseId                      MainBranch                 Age  \
     65437           1  I am a developer by profession  Under 18 years old   
     65438           2  I am a developer by profession     35-44 years old   
     65439           3  I am a developer by profession     45-54 years old   
     65440           4           I am learning to code     18-24 years old   
     65441           5  I am a developer by profession     18-24 years old   
     
                     Employment RemoteWork   Check  \
     65437  Employed, full-time     Remote  Apples   
     65438  Employed, full-time     Remote  Apples   
     65439  Employed, full-time     Remote  Apples   
     65440   Student, full-time        NaN  Apples   
     65441   Student, full-time        NaN  Apples   
     
                                             CodingActivities  \
     65437                                              Hobby   
     65438  Hobby;Contribute to open-source projects;Other...   
     65439  Hobby;Contribute to open-source projects;Other...   
     65440                                                NaN   
     65441                                                NaN   
     
                                                      EdLevel  \
     65437                          Primary/elementary school   
     65438       Bachelor’s degree (B.A., B.S., B.Eng., etc.)   
     65439    Master’s degree (M.A., M.S., M.Eng., MBA, etc.)   
     65440  Some college/university study without earning ...   
     65441  Secondary school (e.g. American high school, G...   
     
                                                    LearnCode  \
     65437                             Books / Physical media   
     65438  Books / Physical media;Colleague;On the job tr...   
     65439  Books / Physical media;Colleague;On the job tr...   
     65440  Other online resources (e.g., videos, blogs, f...   
     65441  Other online resources (e.g., videos, blogs, f...   
     
                                              LearnCodeOnline  ... JobSatPoints_6  \
     65437                                                NaN  ...            NaN   
     65438  Technical documentation;Blogs;Books;Written Tu...  ...            0.0   
     65439  Technical documentation;Blogs;Books;Written Tu...  ...            NaN   
     65440  Stack Overflow;How-to videos;Interactive tutorial  ...            NaN   
     65441  Technical documentation;Blogs;Written Tutorial...  ...            NaN   
     
           JobSatPoints_7 JobSatPoints_8 JobSatPoints_9 JobSatPoints_10  \
     65437            NaN            NaN            NaN             NaN   
     65438            0.0            0.0            0.0             0.0   
     65439            NaN            NaN            NaN             NaN   
     65440            NaN            NaN            NaN             NaN   
     65441            NaN            NaN            NaN             NaN   
     
           JobSatPoints_11           SurveyLength SurveyEase ConvertedCompYearly  \
     65437             NaN                    NaN        NaN                 NaN   
     65438             0.0                    NaN        NaN                 NaN   
     65439             NaN  Appropriate in length       Easy                 NaN   
     65440             NaN               Too long       Easy                 NaN   
     65441             NaN              Too short       Easy                 NaN   
     
           JobSat  
     65437    NaN  
     65438    NaN  
     65439    NaN  
     65440    NaN  
     65441    NaN  
     
     [5 rows x 114 columns])



### Task 2: Analyze Characteristics of Duplicates
1. Identify which columns have the same values in duplicate rows.
2. Analyze the distribution of duplicates across different columns such as Country, Employment, and DevType.



```python
#Identify which columns have the same values in duplicate rows.
duplicate_rows = df[df.duplicated(keep=False)]  # Include all duplicates for analysis
same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
print(same_values)

#Analyze the distribution of duplicates across different columns such as Country, Employment, and DevType.
columns_to_analyze = ['Country', 'Employment', 'DevType']

for column in columns_to_analyze:
    distribution = duplicate_rows[column].value_counts()
    print(f"Distribution of duplicates in {column}:\n", distribution)
```

    Empty DataFrame
    Columns: [ResponseId, MainBranch, Age, Employment, RemoteWork, Check, CodingActivities, EdLevel, LearnCode, LearnCodeOnline, TechDoc, YearsCode, YearsCodePro, DevType, OrgSize, PurchaseInfluence, BuyNewTool, BuildvsBuy, TechEndorse, Country, Currency, CompTotal, LanguageHaveWorkedWith, LanguageWantToWorkWith, LanguageAdmired, DatabaseHaveWorkedWith, DatabaseWantToWorkWith, DatabaseAdmired, PlatformHaveWorkedWith, PlatformWantToWorkWith, PlatformAdmired, WebframeHaveWorkedWith, WebframeWantToWorkWith, WebframeAdmired, EmbeddedHaveWorkedWith, EmbeddedWantToWorkWith, EmbeddedAdmired, MiscTechHaveWorkedWith, MiscTechWantToWorkWith, MiscTechAdmired, ToolsTechHaveWorkedWith, ToolsTechWantToWorkWith, ToolsTechAdmired, NEWCollabToolsHaveWorkedWith, NEWCollabToolsWantToWorkWith, NEWCollabToolsAdmired, OpSysPersonal use, OpSysProfessional use, OfficeStackAsyncHaveWorkedWith, OfficeStackAsyncWantToWorkWith, OfficeStackAsyncAdmired, OfficeStackSyncHaveWorkedWith, OfficeStackSyncWantToWorkWith, OfficeStackSyncAdmired, AISearchDevHaveWorkedWith, AISearchDevWantToWorkWith, AISearchDevAdmired, NEWSOSites, SOVisitFreq, SOAccount, SOPartFreq, SOHow, SOComm, AISelect, AISent, AIBen, AIAcc, AIComplex, AIToolCurrently Using, AIToolInterested in Using, AIToolNot interested in Using, AINextMuch more integrated, AINextNo change, AINextMore integrated, AINextLess integrated, AINextMuch less integrated, AIThreat, AIEthics, AIChallenges, TBranch, ICorPM, WorkExp, Knowledge_1, Knowledge_2, Knowledge_3, Knowledge_4, Knowledge_5, Knowledge_6, Knowledge_7, Knowledge_8, Knowledge_9, Frequency_1, Frequency_2, Frequency_3, TimeSearching, TimeAnswering, Frustration, ProfessionalTech, ProfessionalCloud, ProfessionalQuestion, ...]
    Index: []
    
    [0 rows x 115 columns]
    Distribution of duplicates in Country:
     Country
    United States of America                                6
    United Kingdom of Great Britain and Northern Ireland    6
    Canada                                                  2
    Norway                                                  2
    Uzbekistan                                              2
    Serbia                                                  2
    Name: count, dtype: int64
    Distribution of duplicates in Employment:
     Employment
    Employed, full-time                                      10
    Student, full-time                                        6
    Student, full-time;Not employed, but looking for work     2
    Independent contractor, freelancer, or self-employed      2
    Name: count, dtype: int64
    Distribution of duplicates in DevType:
     DevType
    Developer, full-stack    8
    Student                  4
    Academic researcher      4
    Developer Experience     2
    Name: count, dtype: int64


    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')
    /tmp/ipykernel_84/886262335.py:3: PerformanceWarning: DataFrame is highly fragmented.  This is usually the result of calling `frame.insert` many times, which has poor performance.  Consider joining all columns at once using pd.concat(axis=1) instead. To get a de-fragmented frame, use `newframe = frame.copy()`
      same_values = duplicate_rows.groupby(list(df.columns)).size().reset_index(name='Count')


### Task 3: Visualize Duplicates Distribution
1. Create visualizations to show the distribution of duplicates across different categories.
2. Use bar charts or pie charts to represent the distribution of duplicates by Country and Employment.



```python
import matplotlib.pyplot as plt

# Analyze the distribution of duplicates by 'Country' and 'Employment'
columns_to_visualize = ['Country', 'Employment']

for column in columns_to_visualize:
    distribution = duplicate_rows[column].value_counts()

# Create pie charts
plt.figure(figsize=(8, 8))
distribution.plot(kind='pie', autopct='%1.1f%%', startangle=140, colors=plt.cm.Paired.colors)
plt.title(f"Percentage of Duplicates by {column}", fontsize=14)
plt.ylabel('')  # Eliminar etiqueta del eje Y
plt.tight_layout()
plt.show()

```


    
![png](output_27_0.png)
    


### Task 4: Strategic Removal of Duplicates
1. Decide which columns are critical for defining uniqueness in the dataset.
2. Remove duplicates based on a subset of columns if complete row duplication is not a good criterion.



```python
columns_names = df.columns.values
columns_names
```




    array(['ResponseId', 'MainBranch', 'Age', 'Employment', 'RemoteWork',
           'Check', 'CodingActivities', 'EdLevel', 'LearnCode',
           'LearnCodeOnline', 'TechDoc', 'YearsCode', 'YearsCodePro',
           'DevType', 'OrgSize', 'PurchaseInfluence', 'BuyNewTool',
           'BuildvsBuy', 'TechEndorse', 'Country', 'Currency', 'CompTotal',
           'LanguageHaveWorkedWith', 'LanguageWantToWorkWith',
           'LanguageAdmired', 'DatabaseHaveWorkedWith',
           'DatabaseWantToWorkWith', 'DatabaseAdmired',
           'PlatformHaveWorkedWith', 'PlatformWantToWorkWith',
           'PlatformAdmired', 'WebframeHaveWorkedWith',
           'WebframeWantToWorkWith', 'WebframeAdmired',
           'EmbeddedHaveWorkedWith', 'EmbeddedWantToWorkWith',
           'EmbeddedAdmired', 'MiscTechHaveWorkedWith',
           'MiscTechWantToWorkWith', 'MiscTechAdmired',
           'ToolsTechHaveWorkedWith', 'ToolsTechWantToWorkWith',
           'ToolsTechAdmired', 'NEWCollabToolsHaveWorkedWith',
           'NEWCollabToolsWantToWorkWith', 'NEWCollabToolsAdmired',
           'OpSysPersonal use', 'OpSysProfessional use',
           'OfficeStackAsyncHaveWorkedWith', 'OfficeStackAsyncWantToWorkWith',
           'OfficeStackAsyncAdmired', 'OfficeStackSyncHaveWorkedWith',
           'OfficeStackSyncWantToWorkWith', 'OfficeStackSyncAdmired',
           'AISearchDevHaveWorkedWith', 'AISearchDevWantToWorkWith',
           'AISearchDevAdmired', 'NEWSOSites', 'SOVisitFreq', 'SOAccount',
           'SOPartFreq', 'SOHow', 'SOComm', 'AISelect', 'AISent', 'AIBen',
           'AIAcc', 'AIComplex', 'AIToolCurrently Using',
           'AIToolInterested in Using', 'AIToolNot interested in Using',
           'AINextMuch more integrated', 'AINextNo change',
           'AINextMore integrated', 'AINextLess integrated',
           'AINextMuch less integrated', 'AIThreat', 'AIEthics',
           'AIChallenges', 'TBranch', 'ICorPM', 'WorkExp', 'Knowledge_1',
           'Knowledge_2', 'Knowledge_3', 'Knowledge_4', 'Knowledge_5',
           'Knowledge_6', 'Knowledge_7', 'Knowledge_8', 'Knowledge_9',
           'Frequency_1', 'Frequency_2', 'Frequency_3', 'TimeSearching',
           'TimeAnswering', 'Frustration', 'ProfessionalTech',
           'ProfessionalCloud', 'ProfessionalQuestion', 'Industry',
           'JobSatPoints_1', 'JobSatPoints_4', 'JobSatPoints_5',
           'JobSatPoints_6', 'JobSatPoints_7', 'JobSatPoints_8',
           'JobSatPoints_9', 'JobSatPoints_10', 'JobSatPoints_11',
           'SurveyLength', 'SurveyEase', 'ConvertedCompYearly', 'JobSat'],
          dtype=object)




```python
critical_columns = ['Country', 'Employment', 'DevType','CompTotal','ConvertedCompYearly']

# Eliminar duplicados basados en las columnas críticas
df_unique = df.drop_duplicates(subset=critical_columns, keep='first')

# Mostrar el número de filas antes y después de eliminar duplicados
original_row_count = df.shape[0]
unique_row_count = df_unique.shape[0]

original_row_count, unique_row_count
```




    (65447, 32795)



## Verify and Document Duplicate Removal Process


### Task 5: Documentation



#### **Step 1: Understand the Dataset**
- **Objective**: Familiarize yourself with the dataset's structure, columns, and types of data it contains.
- **Actions**:
  1. Load the dataset into a data analysis tool like Python (pandas library).
  2. Use functions like `.head()` to inspect the columns and preview data.

#### **Step 2: Identify Duplicate Rows**
- **Objective**: Determine if the dataset contains duplicate rows and assess their nature.
- **Actions**:
  1. Check for complete duplicate rows:
  2. Display a sample of duplicate rows to understand their structure:
  3. Analyze duplicate patterns to identify frequent issues.

#### **Step 3: Define Uniqueness Criteria**
- **Objective**: Determine which columns are critical for identifying unique records.
- **Considerations**:
  - Are all columns necessary to define a unique row?
  - Would certain columns (e.g., `ID`, `Country`, `Employment`) be sufficient for defining uniqueness?

#### **Step 4: Strategic Removal of Duplicates**
- **Objective**: Remove duplicates while preserving meaningful data.
- **Actions**:
  1. Drop duplicates based on all columns if complete duplication is the criterion
  2. Drop duplicates based on a subset of columns if partial duplication is acceptable
  3. Verify the results

#### **Step 5: Analyze Distribution of Duplicates**
- **Objective**: Gain insights into which categories contribute to duplicates.
- **Actions**:
  1. Use value counts on columns with duplicates
  2. Visualize duplicates using bar charts or pie charts

#### **Best Practices for Managing Duplicates**
1. **Understand the Data Context**:
   - Ensure you know the meaning of each column to avoid unintentional data loss.
2. **Preserve the Original Dataset**:
   - Always work on a copy of the dataset when cleaning.
3. **Validate Results**:
   - Double-check for remaining duplicates using `.duplicated()` after removal.




2. Explain the reasoning behind selecting specific columns for identifying and removing duplicates.


## Reasoning Behind Column Selection

### **Purpose of Analysis:**

The project focuses on understanding key trends, such as programming languages in demand, database technologies sought after, and popular Integrated Development Environments (IDEs).

Employment distribution and compensation analysis are also critical for the study.

### **Critical Columns for Uniqueness:**

**Country:** Geographical context is important for analyzing regional trends.

**Employment:** Helps understand the employment distribution among respondents.

**DevType:** Identifies the types of developers and their roles.

**CompTotal and ConvertedCompYearly:** Essential for comparing and normalizing compensation data

### Summary and Next Steps
**In this lab, you focused on identifying and analyzing duplicate rows within the dataset.**

- You employed various techniques to explore the nature of duplicates and applied strategic methods for their removal.
- For additional analysis, consider investigating the impact of duplicates on specific analyses and how their removal affects the results.
- This version of the lab is more focused on duplicate analysis and handling, providing a structured approach to deal with duplicates in a dataset effectively.


<!--
## Change Log
|Date (YYYY-MM-DD)|Version|Changed By|Change Description|
|-|-|-|-|
|2024-11- 05|1.3|Madhusudhan Moole|Updated lab|
|2024-10-28|1.2|Madhusudhan Moole|Updated lab|
|2024-09-24|1.1|Madhusudhan Moole|Updated lab|
|2024-09-23|1.0|Raghul Ramesh|Created lab|
--!>


Copyright © IBM Corporation. All rights reserved.

