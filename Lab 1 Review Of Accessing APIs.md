<p style="text-align:center">
    <a href="https://skills.network/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDA0321ENSkillsNetwork928-2022-01-01" target="_blank">
    <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/assets/logos/SN_web_lightmode.png" width="300" alt="Skills Network Logo"  />
    </a>
</p>


<h1> Review Of Accessing APIs</h1>

Estimated time needed: **30** minutes

## Objectives

After completing this lab, you will be able to:

*   Understand HTTP
*   Analyze HTTP Requests


<h2>Table of Contents</h2>

<div class="alert alert-block alert-info" style="margin-top: 20px">
    <ul>
        <li>
            <a href="#Overview-of-HTTP">Overview of HTTP </a>
            <ul>
                <li><a href="#Uniform-Resource-Locator-(URL)">Uniform Resource Locator: URL</a></li>
                 <li><a href="#Request">Request</a></li>
                <li><a href="#Response">Response</a></li>
            </ul>
        </li>
        <li>
            <a href="#Requests-in-Python">Requests in Python  </a>
            <ul>
                <li><a href="#Get-Request-with-URL-Parameters">Get Request with URL Parameters</a></li>
                <li><a href="#Post-Requests">Post Requests </a></li>

</ul>

</div>

<hr>


## Overview of HTTP


When you, the **client**, access a web page, your browser sends an **HTTP** request to the **server** where the page is hosted. The server tries to locate the desired **resource** by default "<code>index.html</code>". If your request is successful, the server will send the object to the client in an **HTTP response**, which includes information like the type of the **resource**, the length of the **resource**, and other information.

<p>
The figure below represents the process. The circle on the left represents the client, the circle on the right represents the web server. The table under the web server represents a list of resources stored in the web server. In  this case an <code>HTML</code> file, <code>png</code> image, and <code>txt</code> file .
</p>
<p>
The <b>HTTP</b> protocol enables you to send and receive information through the web including webpages, images, and other web resources. In this lab, you will explore the Requests library for interacting with the <code>HTTP</code> protocol. 
</p


<div class="alert alert-block alert-info" style="margin-top: 20px">
         <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0101EN-SkillsNetwork/labs/Module%205/images/reqest_basics.png" width="750" align="center">

</div>


## Uniform Resource Locator (URL)


Uniform resource locator (URL) is the common way to find resources on the web.  A URL can be broken down into three main parts:

<ul>
    <li><b>Scheme</b>: The protocol used, which in this lab will always be<code>http://</code>  </li>
    <li><b> Internet address or  Base URL</b>: Used to locate the resource. Examples include <code>www.ibm.com</code> and  <code> www.gitlab.com </code> </li>
    <li><b>Route</b>: Location on the web server for example: <code>/images/IDSNlogo.png</code> </li>
</ul>


You may also hear the term Uniform Resource Identifier (URI). URL are actually a subset of URIs. Another popular term is endpoint, which refers to the URL of an operation provided by a web server.


## Request


The process can be broken into the <b>Request</b> and <b>Response </b> process.  The request using the get method is partially illustrated below. In the start line we have the <code>GET</code> method, this is an <code>HTTP</code> method. Also the location of the resource  <code>/index.html</code> and the <code>HTTP</code> version. The Request header passes additional information with an <code>HTTP</code> request:


<div class="alert alert-block alert-info" style="margin-top: 20px">
         <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0101EN-SkillsNetwork/labs/Module%205/images/reqest_messege.png" width="400" align="center">
</div>


When an <code>HTTP</code> request is made, an <code>HTTP</code> method is sent, this tells the server what action to perform.  A list of several <code>HTTP</code> methods is shown below. We will review more examples later.


<div class="alert alert-block alert-info" style="margin-top: 20px">
         <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/SC6-zFofaLr-ILglB1xFGA/HTTP%20image.png" width="900" align="center">
</div>


## Response


The figure below represents the response; the response start line contains the version number <code>HTTP/1.0</code>, a status code (200) meaning success, followed by a descriptive phrase (OK). The response header contains useful information. Finally, we have the response body containing the requested file, an <code> HTML </code> document.  It should be noted that some requests have headers.


<div class="alert alert-block alert-info" style="margin-top: 20px">
         <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/kGeR47UyGjoLBL8R9yVH5Q/Response%20image.png" width="600" align="center">
</div> 


Some status code examples are shown in the table below, the prefix indicates the class. These are shown in yellow, with actual status codes shown in  white. Check out the following <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Status?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0101ENSkillsNetwork19487395-2021-01-01">link </a> for more descriptions.


<div class="alert alert-block alert-info" style="margin-top: 20px">
         <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0101EN-SkillsNetwork/labs/Module%205/images/status_code.png" width="300" align="center">
</div>


### Install the required libraries



```python
!pip install requests
!pip install pillow
```

    Requirement already satisfied: requests in /opt/conda/lib/python3.11/site-packages (2.31.0)
    Requirement already satisfied: charset-normalizer<4,>=2 in /opt/conda/lib/python3.11/site-packages (from requests) (3.3.2)
    Requirement already satisfied: idna<4,>=2.5 in /opt/conda/lib/python3.11/site-packages (from requests) (3.7)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /opt/conda/lib/python3.11/site-packages (from requests) (2.2.1)
    Requirement already satisfied: certifi>=2017.4.17 in /opt/conda/lib/python3.11/site-packages (from requests) (2024.12.14)
    Collecting pillow
      Downloading pillow-11.1.0-cp311-cp311-manylinux_2_28_x86_64.whl.metadata (9.1 kB)
    Downloading pillow-11.1.0-cp311-cp311-manylinux_2_28_x86_64.whl (4.5 MB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m4.5/4.5 MB[0m [31m92.9 MB/s[0m eta [36m0:00:00[0m:00:01[0m
    [?25hInstalling collected packages: pillow
    Successfully installed pillow-11.1.0


## Requests in Python


Requests is a Python Library that allows you to send <code>HTTP/1.1</code> requests easily. We can import the library as follows:



```python
import requests
```

We will also use the following libraries:



```python
import os 
from PIL import Image
from IPython.display import IFrame
```

You can make a <code>GET</code> request via the method <code>get</code> to [www.ibm.com](http://www.ibm.com/?utm_source=Exinfluencer&utm_content=000026UJ&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0101ENSkillsNetwork19487395-2021-01-01&utm_medium=Exinfluencer&utm_term=10006555):



```python

url='https://www.ibm.com/'
r=requests.get(url)

```

We have the response object <code>r</code>, this has information about the request, like the status of the request. You can view the status code using the attribute <code>status_code</code>.



```python
r.status_code
```




    200



You can view the request headers:



```python
print(r.request.headers)
```

    {'User-Agent': 'python-requests/2.31.0', 'Accept-Encoding': 'gzip, deflate, br, zstd', 'Accept': '*/*', 'Connection': 'keep-alive', 'Cookie': '_abck=56774E1196BE05291D8D57EC7CA8232F~-1~YAAQl2vcF2rrYrmTAQAAw5iRQA0Py9UfBRn4Ra0DmNgBYhWjXOAyYeq5h1+LGIXXyhNaPz6DWDYE85iIepC6rxWzhay1Roy5+eZu3rAg0+htairXz7+mGWL5D0NXvDVb5j2WxfwIlK5ULAkBHEntTnlt/g/fT7fIVCLAu9fzrdU5z+cSpLLd0l5Ini21Hx4PVO2wgJ4lMHF7jACSb2iIsH6RWP6jVoML0dybry2WRsA8CW8fxUkHkURojiV1+GV6JOuqTWrue7OLmUsGVWkYSCEvDqFIrIvYBsY2HqaDV8pmtp3XFMV1WukXcIcytImE8Pq2ZzM9MsX716jvpoQB6gFoartA3ODs0zNHMlnxCSthvYMrYILuNfRk2tbnOj2UsR0K7rrdTvAQKJ17K5RSIsyE3IAd2Z8=~-1~-1~-1; bm_sz=08265AA80A0C6219EDAEE7E8ED19CE9F~YAAQl2vcF2vrYrmTAQAAw5iRQBrSYIG1TpEEfnNB1hwFRiAU/gIosdHoB8MOHIfS8ff1RTtdq/XXKzhzx5sg2pJJcm/BV5BGFynT74zcUlkmaD/AeIkZrYD25BrxyL4wqVExjeXsele1PwnicSG/fCo0RlX+GsXm04YsqQ9RF7kpfSIawYcLe2x8nIPiYpv/UZH/YJys4jqO7CO7x3c1qghqc531HpUAIW7UcEPA1erKn+Tafs5X0GU42iQRVkZAkrb+y/jNaNoxYVhPMW1pOG55AbzkfdiGSezJS/oY/iTugfmdprtmsMhng/uEjCjCGCkte1sbfYQVpqyBgfO1DO6agIjlHu/2q/0=~3425328~3225923'}


You can view the request body, in the following line, as there is nobody for a get request we get <code>None</code>:



```python
print("request body:", r.request.body)
```

    request body: None


You can view the <code>HTTP</code> response header using the attribute <code>headers</code>. This returns a python dictionary of <code>HTTP</code> response headers.



```python
header=r.headers
print(r.headers)
```

    {'Content-Security-Policy': 'upgrade-insecure-requests', 'x-frame-options': 'SAMEORIGIN', 'Last-Modified': 'Tue, 07 Jan 2025 11:15:23 GMT', 'ETag': '"275b8-62b1bdc7b0cca-gzip"', 'Accept-Ranges': 'bytes', 'Content-Type': 'text/html;charset=utf-8', 'X-Content-Type-Options': 'nosniff', 'Cache-Control': 'max-age=600', 'Expires': 'Tue, 07 Jan 2025 11:51:11 GMT', 'X-Akamai-Transformed': '9 27138 0 pmb=mTOE,2', 'Content-Encoding': 'gzip', 'Date': 'Tue, 07 Jan 2025 11:41:11 GMT', 'Content-Length': '27346', 'Connection': 'keep-alive', 'Vary': 'Accept-Encoding', 'Strict-Transport-Security': 'max-age=31536000'}


You can obtain the date the request was sent using the key <code>Date</code>.



```python
header['date']
```




    'Tue, 07 Jan 2025 11:41:11 GMT'



<code>Content-Type</code> indicates the type of data:



```python
header['Content-Type']
```




    'text/html;charset=utf-8'



You can also check the <code>encoding</code>:



```python
 r.encoding
```




    'utf-8'



As the <code>Content-Type</code> is <code>text/html</code> you can use the attribute <code>text</code> to display the <code>HTML</code> in the body. You can review the first 100 characters:



```python
r.text[0:100]
```




    '\n<!DOCTYPE HTML>\n<html lang="en">\n<head>\r\n    \r\n    \r\n    \r\n    \r\n    \r\n    \r\n    \r\n      \r\n    \r\n  '



You can load other types of data for non-text requests, like images. Consider the URL of the following image:



```python
# Use single quotation marks for defining string
url='https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0101EN-SkillsNetwork/IDSNlogo.png'
```

You can make a get request:



```python
r=requests.get(url)
```

You can look at the response header:



```python
print(r.headers)
```

    {'Date': 'Tue, 07 Jan 2025 11:41:17 GMT', 'X-Clv-Request-Id': 'd30dd62f-20e8-4519-952b-1b33f9d03c39', 'Server': 'Cleversafe', 'X-Clv-S3-Version': '2.5', 'Accept-Ranges': 'bytes', 'x-amz-request-id': 'd30dd62f-20e8-4519-952b-1b33f9d03c39', 'ETag': '"8bb44578fff8fdcc3d2972be9ece0164"', 'Content-Type': 'image/png', 'Last-Modified': 'Wed, 16 Nov 2022 03:32:41 GMT', 'Content-Length': '78776'}


You can see the <code>'Content-Type'</code>



```python
r.headers['Content-Type']
```




    'image/png'



An image is a response object that contains the image as a <a href="https://docs.python.org/3/glossary.html?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0101ENSkillsNetwork19487395-2021-01-01#term-bytes-like-object">bytes-like object</a>. As a result, we must save it using a file object. First, you specify the <u>file path and
name</u>



```python
path=os.path.join(os.getcwd(),'image.png')
```

You save the file, in order to access the body of the response we use the attribute <code>content</code> then save it using the <code>open</code> function and write <code>method</code>:



```python
with open(path,'wb') as f:
    f.write(r.content)
```

You can view the image:



```python
Image.open(path)
```




    
![png](output_57_0.png)
    



<h3>Question: Download a file </h3>


Consider the following URL:


<code>URL = <https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0101EN-SkillsNetwork/labs/Module%205/data/Example1.txt</code>


Write the commands to download the txt file in the given link.



```python
url='https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0101EN-SkillsNetwork/labs/Module%205/data/Example1.txt'
path=os.path.join(os.getcwd(),'example1.txt')
r=requests.get(url)
with open (path,'wb') as f:
    f.write(r.content)
```

<details><summary>Click here for the solution</summary>

```python
url='https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0101EN-SkillsNetwork/labs/Module%205/data/Example1.txt'
path=os.path.join(os.getcwd(),'example1.txt')
r=requests.get(url)
with open(path,'wb') as f:
    f.write(r.content)

```

</details>


## Get Request with URL Parameters


You can use the <b>GET</b> method to modify the results of your query, for example, retrieving data from an API. We send a <b>GET</b> request to the  server. As before, we have the <b>Base URL</b>, in the <b>Route</b> we append <code>/get</code>, this indicates we would like to preform a <code>GET</code> request. This is demonstrated in the following table:


<div class="alert alert-block alert-info" style="margin-top: 20px">
         <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/HvW_UIwnCUtHR7SKIJHqtQ/base-image-file.jpg" width="400" align="center">
</div>


The Base URL is for <code>[http://httpbin.org/](http://httpbin.org/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0101ENSkillsNetwork19487395-2021-01-01)</code>. It is a simple HTTP Request and Response service. The <code>URL</code> in Python is given by:



```python
url_get='http://httpbin.org/get'
```

A <a href="https://en.wikipedia.org/wiki/Query_string?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0101ENSkillsNetwork19487395-2021-01-01">query string</a> is a part of a uniform resource locator (URL), it sends other information to the web server. The start of the query is a <code>?</code>, followed by a series of parameter and value pairs, as shown in the table below. The first parameter name is <code>name</code> and the value is <code>Joseph</code>. The second parameter name is <code>ID</code> and the Value is <code>123</code>. Each pair, parameter, and value is separated by an equals sign, <code>=</code>.
The series of pairs is separated by the ampersand <code>&</code>.


<div class="alert alert-block alert-info" style="margin-top: 20px">
         <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0101EN-SkillsNetwork/labs/Module%205/images/query_string.png" width="500" align="center">
</div>


To create a Query string, add a dictionary. The keys are the parameter names and the values are the value of the Query string.



```python
payload={"name":"Joseph","ID":"123"}
```

Then passing the dictionary <code>payload</code> to the <code>params</code> parameter of the <code> get()</code> function:



```python
r=requests.get(url_get,params=payload)
```

You can print out the <code>URL</code> and see the name and values.



```python
r.url
```




    'http://httpbin.org/get?name=Joseph&ID=123'



There is no request body.



```python
print("request body:", r.request.body)
```

    request body: None


You can print out the status code.



```python
print(r.status_code)
```

    200


You can view the response as text:



```python
print(r.text)
```

    {
      "args": {
        "ID": "123", 
        "name": "Joseph"
      }, 
      "headers": {
        "Accept": "*/*", 
        "Accept-Encoding": "gzip, deflate, br, zstd", 
        "Host": "httpbin.org", 
        "User-Agent": "python-requests/2.31.0", 
        "X-Amzn-Trace-Id": "Root=1-677d12e5-1cffb6d77ccb51e71c59c60a"
      }, 
      "origin": "52.117.120.36", 
      "url": "http://httpbin.org/get?name=Joseph&ID=123"
    }
    


You can look at the <code>'Content-Type'</code>.



```python
r.headers['Content-Type']
```




    'application/json'



As the content <code>'Content-Type'</code> is in the <code>JSON</code> format, you can use the method <code>json()</code>, it returns a Python <code>dict</code>:



```python
r.json()
```




    {'args': {'ID': '123', 'name': 'Joseph'},
     'headers': {'Accept': '*/*',
      'Accept-Encoding': 'gzip, deflate, br, zstd',
      'Host': 'httpbin.org',
      'User-Agent': 'python-requests/2.31.0',
      'X-Amzn-Trace-Id': 'Root=1-677d12e5-1cffb6d77ccb51e71c59c60a'},
     'origin': '52.117.120.36',
     'url': 'http://httpbin.org/get?name=Joseph&ID=123'}



The key <code>args</code> has the name and values:



```python
r.json()['args']
```




    {'ID': '123', 'name': 'Joseph'}



<hr>

<p>Congratulations, you have completed your hands-on lab on Review Of Accessing APIs.
<hr>


## Authors

<p><a href="https://www.linkedin.com/in/joseph-s-50398b136/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0101ENSkillsNetwork19487395-2021-01-01" target="_blank">Joseph Santarcangelo</a> 


### Other Contributors

<a href="https://www.linkedin.com/in/jiahui-mavis-zhou-a4537814a?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0101ENSkillsNetwork19487395-2021-01-01">Mavis Zhou</a>


<!--
## Change Log

| Date (YYYY-MM-DD) | Version | Changed By | Change Description           |
| ----------------- | ------- | ---------- | ---------------------------- |
| 2024-10-04        | 2.5     | Madhusudan | ID reviewed                  |
| 2024-05-23        | 2.5     | Madhusudan | ID reviewed                  |
| 2023-11-02        | 2.4     | Abhishek Gagneja | Updated instructions   |
| 2023-06-07        | 2.3     |Akansha Yadav| Spell Check                 |
| 2021-12-20        | 2.1     | Malika     | Updated the links            |
| 2020-09-02        | 2.0     | Simran     | Template updates to the file |
|                   |         |            |                              |
|                   |         |            |                              |

## <h3 align="center"> © IBM Corporation. All rights reserved. <h3/>
--!>


Copyright © IBM Corporation. All rights reserved.

