
# Site API Design

## ***POST*** /V1/CMDB/Sites/Parent
Calling this API to create a container si/Parentte. If one parent site doesn't exist in current system, create it before create its child site. 

Note: this API call needs to be invoked in a site transaction.

## Detail Information

> **Title** : Create A Parent Sites API<br>

> **Version** : 02/04/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Sites/Parent

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|sitePath* | string  | Full path name of a site. For example, 'My Network/Site1/Boston'.  |

> ***Example***


```python
{
    "sitePath":"My Network/Site1/Boston"
}         
```

## Parameters(****required***)

>No parameters required.

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
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
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
token = "1c52cd65-3247-44ad-91e6-cd73fc6c64a6"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Parent"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

sitePath = "My Network/Site1"

body = {
            "sitePath" : sitePath       
        }

try:
    response = requests.post(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Parent Site Created Failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman: 


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Sites/Parent \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: b95c68fb-b3b2-450c-84a8-fec659e2366f' \
  -H 'cache-control: no-cache' \
  -H 'token: 1c52cd65-3247-44ad-91e6-cd73fc6c64a6' \
  -d '{
        "sitePath":"My Network/Site1/Boston"
    }'
```

# Error Example:


```python
###################################################################################################################    

"""Error 1: empty input"""

Input:
        
        sitePath = ""

Response:
    
       "Parent Site Created Failed! - 
            {
                "statusCode":791000,
                "statusDescription":"Null parameter: the parameter 'sitePath' cannot be null."
            }"

###################################################################################################################    

"""Error 2: wrong input"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
        
        sitePath = "Boston/haha/babab"

Response:
    
       "{
            'statusCode': 790200,
            'statusDescription': 'Success.'          
        }"

###################################################################################################################    

"""Error 3: wrong input value"""

Input:
    
        sitePath = 111/222/333

Response:
    
       "Parent Site Created Failed! - 
            {
                "statusCode":791001,
                "statusDescription":"Invalid parameter: the parameter 'sitePath' is invalid."
            }"
            
###################################################################################################################    

"""Error 3: Same Path multi-created""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    
        sitePath = "My Network/Site1"

Response:
    
       "{
            "statusCode": 790200,
            "statusDescription": "Success."
        }"
```
