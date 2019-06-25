
# Authentication and Authorization API

## ***DELETE*** /V1/Session
Use this API to log out from the current session.

## Detail Information

> **Title** : Log out API<br>

> **Version** : 01/23/2019.

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server /ServicesAPI/API/V1/Session

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No request body.

## Parameters(****required***)

> No parameters required.

## Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| token | string  | Authentication token, get from login API. |

## Response

|**Name**|**Type**|**Description**|
|------|------|------|
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code. |

> ***Example*** :


```python
{
    "statusCode": 790200,
    "statusDescription": "Success."
}
```

## Full Example : 


```python
# import python modules 
import requests
import time
import urllib3
import pprint
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

# Set the request inputs
token = "a63c6610-1a44-4907-bb57-784179d30ba3"
full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"
    
# Set proper headers
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["token"] = token

try:
    # Do the HTTP request
    response = requests.delete(full_url, headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Session logout failed! - " + str(response.text))

except Exception as e:
    print (str(e))
    

```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X DELETE \
  http://192.168.28.79/ServicesAPI/API/V1/Session \
  -H 'Postman-Token: d6de8cb3-ca3b-4bde-b9c7-be800e902d2c' \
  -H 'cache-control: no-cache' \
  -H 'token: 7480e46f-6a25-470e-9c61-351f6b7d86fa'
```

## Error Exampes


```python
###################################################################################################################    

"""Error 1: empty url"""

Input:
    token = "a63c6610-1a44-4907-bb57-784179d30ba3"
    
    full_url = ""  

Response:
    "Invalid URL '': No schema supplied. Perhaps you meant http://?"
    
###################################################################################################################    

"""Error 2: wrong url"""

Input:
    token = "a63c6610-1a44-4907-bb57-784179d30ba3"
    
    full_url = "http://IP address of your NetBrain Web API Serveraaaaaaaaaaaaaa/ServicesAPI/API/V1/Session"  

Response:
    """HTTPConnectionPool(host='192.1688.28.79', port=80): 
       Max retries exceeded with url: /ServicesAPI/API/V1/Session (Caused by NewConnectionError(
           '<urllib3.connection.HTTPConnection object at 0x0000022F203C79B0>: 
           Failed to establish a new connection: [Errno 11001] getaddrinfo failed'))"""
    
################################################################################################################### 

"""Error 3: empty token"""

Input:
    token = "" 
    
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"  

Response:
    { "statusCode": 795005, "statusDescription": "Invalid token. }
     
###################################################################################################################    

"""Error 4: wrong token"""

Input:
    token = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa" 
    
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"  

Response:
    { "statusCode": 795005, "statusDescription": "Invalid token. }
```
