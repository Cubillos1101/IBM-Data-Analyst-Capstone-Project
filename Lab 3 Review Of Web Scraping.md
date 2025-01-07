<p style="text-align:center">
    <a href="https://skills.network/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDA0321ENSkillsNetwork928-2022-01-01" target="_blank">
    <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/assets/logos/SN_web_lightmode.png" width="200" alt="Skills Network Logo"  />
    </a>
</p>


# **Web Scraping Lab**


Estimated time needed: **30** minutes


## Objectives


After completing this lab, you will be able to:


* Download a webpage using requests module
* Scrape all links from a webpage
* Scrape all image URLs from a web page
* Scrape data from html tables


## Scrape www.ibm.com


Import the required modules and functions



```python
from bs4 import BeautifulSoup # this module helps in web scrapping.
import requests  # this module helps us to download a webpage
```

Download the contents of the webpage



```python
url = "http://www.ibm.com"
```


```python
# get the contents of the webpage in text format and store in a variable called data
data  = requests.get(url).text 
```

Create a soup object using the class BeautifulSoup



```python
soup = BeautifulSoup(data,"html.parser")  # create a soup object using the variable 'data'
```

Scrape all links



```python
for link in soup.find_all('a'):  # in html anchor/link is represented by the tag <a>
    print(link.get('href'))
```

    https://www.ibm.com/granite?lnk=dev
    https://developer.ibm.com/technologies/artificial-intelligence?lnk=dev
    https://www.ibm.com/products/watsonx-code-assistant?lnk=dev
    https://www.ibm.com/watsonx/developer/?lnk=dev
    https://www.ibm.com/thought-leadership/institute-business-value/report/ceo-generative-ai?lnk=bus
    https://www.ibm.com/think/reports/ai-in-action?lnk=bus
    https://www.ibm.com/impact/ai-ethics?lnk=bus
    https://www.ibm.com/account/reg/signup?formid=news-urx-52954&lnk=bus
    https://www.ibm.com/artificial-intelligence?lnk=ProdC
    https://www.ibm.com/hybrid-cloud?lnk=ProdC
    https://www.ibm.com/consulting?lnk=ProdC


Scrape  all images



```python
for link in soup.find_all('img'):# in html image is represented by the tag <img>
    print(link.get('src'))
```

## Scrape data from html tables



```python
#The below URL contains a html table with data about colors and color codes.
URL = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DA0321EN-SkillsNetwork/labs/datasets/HTMLColorCodes.html"

```

Before proceeding to scrape a website, you need to examine the contents, and the way data is organized on the website. Open the above URL in your browser and check how many rows and columns are there in the color table.



```python
# get the contents of the webpage in text format and store in a variable called data
data  = requests.get(URL).text
```


```python
soup = BeautifulSoup(data,"html.parser")
```


```python
#find a html table in the web page
table = soup.find('table') # in html table is represented by the tag <table>
```

# Get all rows from the table



```python
for row in table.find_all('tr'): # in html table row is represented by the tag <tr>
    # Get all columns in each row.
    cols = row.find_all('td') # in html a column is represented by the tag <td>
    color_name = cols[2].getText() # store the value in column 3 as color_name
    color_code = cols[3].getText() # store the value in column 4 as color_code
    print("{}--->{}".format(color_name,color_code))
```

    Color Name--->Hex Code#RRGGBB
    lightsalmon--->#FFA07A
    salmon--->#FA8072
    darksalmon--->#E9967A
    lightcoral--->#F08080
    coral--->#FF7F50
    tomato--->#FF6347
    orangered--->#FF4500
    gold--->#FFD700
    orange--->#FFA500
    darkorange--->#FF8C00
    lightyellow--->#FFFFE0
    lemonchiffon--->#FFFACD
    papayawhip--->#FFEFD5
    moccasin--->#FFE4B5
    peachpuff--->#FFDAB9
    palegoldenrod--->#EEE8AA
    khaki--->#F0E68C
    darkkhaki--->#BDB76B
    yellow--->#FFFF00
    lawngreen--->#7CFC00
    chartreuse--->#7FFF00
    limegreen--->#32CD32
    lime--->#00FF00
    forestgreen--->#228B22
    green--->#008000
    powderblue--->#B0E0E6
    lightblue--->#ADD8E6
    lightskyblue--->#87CEFA
    skyblue--->#87CEEB
    deepskyblue--->#00BFFF
    lightsteelblue--->#B0C4DE
    dodgerblue--->#1E90FF


## Authors


Ramesh Sannareddy


### Other Contributors


Rav Ahuja


<!--
## Change Log
|  Date (YYYY-MM-DD) |  Version | Changed By  |  Change Description |
|---|---|---|---|
| 2024-10-29  | 0.2  | Madhusudhan Moole |  Updated lab |
| 2020-10-17  | 0.1  | Ramesh Sannareddy  |  Created initial version of the lab |
--!>


Copyright © IBM Corporation. All rights reserved.

