
# Site API Design

## ***PUT*** /V1/CMDB/Sites/Transactions
This API  is used to commit all site change operations since creating a site transaction. Commit operation will automatically trigger site rebuild process.

## Detail Information

> **Title** : Commit Site Transaction API<br>

> **Version** : 02/01/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Sites/Transactions

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|rebuildSite | bool  | Wheather to rebuild site. Optional, false by default.  |

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




    {'statusCode': 790200, 'statusDescription': 'Success.'}



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
token = "3d0f475d-dbae-4c44-9080-7b08ded7d35b"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Transactions"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

rebuildSite = True

body = {"rebuildSite" : rebuildSite}

try:
    response = requests.put(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Site commit Failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman: 


```python
curl -X PUT \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Sites/Transactions \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: f9f36a8f-f88b-4094-9046-ed4d03733849' \
  -H 'cache-control: no-cache' \
  -H 'token: 3d0f475d-dbae-4c44-9080-7b08ded7d35b' \
  -d '{
        "rebuildSite" : "True"
    }'
```

# Error Example:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
        
        rebuildSite = None # Can be none.

Response:
    
       "{
            "statusCode": 790200,
            "statusDescription": "Success."
        }"
        
###################################################################################################################    

"""Error 1: wrong inputs"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
        
        rebuildSite = 111111111 # Or "111111111" 

Response:
    
       "{
            "statusCode": 790200,
            "statusDescription": "Success."
        }"
```
