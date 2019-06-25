
# Module API Design

## ***GET*** /V1/CMDB/Modules{?hostname}
Call this API to get all modules of device which specified by hostname.

## Detail Information

> **Title** : Get All Modules of a Device API<br>

> **Version** : 01/30/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Modules

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

> No request body.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|hostname* | string  | The hostname of the target device.  |

## Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| token | string  | Authentication token, get from login API. |

## Response

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|moudules| List of string | A collection of module names. |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
    "modules": [],
    "statusCode": 790200,
    "statusDescription": "Success."
}
```

# Full Example:


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "0a187aa9-34e3-407d-9e41-e0eb3824623e"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Modules"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

hostname = "R1"
body={"hostname":hostname}

try:
    response = requests.get(full_url, params=body, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get modules failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'modules': [], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Modules?hostname=Client2' \
  -H 'Postman-Token: 1c6d2533-3044-480e-b08f-af247bcd1d45' \
  -H 'cache-control: no-cache' \
  -H 'token: 0a187aa9-34e3-407d-9e41-e0eb3824623e'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
        hostname = ""
        
Response:
    
    "Get modules failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'hostname' cannot be null."
        }"
        
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
        hostname = "ahahaha" # No device called as "ahahaha"
        
Response:
    
    "Get modules failed! - 
        {
            "statusCode":791006,
            "statusDescription":"device ahahahaha does not exist."
        }"
```
