
# Module API Design

## ***DELETE*** /V1/CMDB/Modules/Attributes
Call this API to delete an module attribute (property) from device schema.

## Detail Information

> **Title** : Delete Module Attribute API<br>

> **Version** : 01/30/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Modules/Attributes
    
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
|attributeName* | string  | The name of the attribute that you want to delete. Please note that the attribute name here is case sensitive.  |

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
    'statusCode': 790200, 
    'statusDescription': 'Success.'
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
token = "9c717c9a-4302-45b5-a068-2a3e9c4ea1a3"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Modules/Attributes"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

attributeName = "Module_newAtt"

body={"attributeName": attributeName} # Why set "attributeName" as a body parameter?

try:
    response = requests.delete(full_url, data=json.dumps(body),headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print ("Delete module attribute :"+str(result))
    else:
        print ("Delete module attribute failed! - " + str(response.text))

except Exception as e:
    print (str(e))       
```

    Delete module attribute :{'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X DELETE \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Modules/Attributes \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 61103f33-dd90-4ad6-8745-d524a8696dd3' \
  -H 'cache-control: no-cache' \
  -H 'token: 9c717c9a-4302-45b5-a068-2a3e9c4ea1a3' \
  -d '{
        "attributeName" : "newAttribute"
    }'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
        attributeName = ""
        
Response:
    
    "Delete module attribute failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'attributeName' cannot be null."
        }"
        
###################################################################################################################    

"""Error 2: wrong attribute name provided"""

Input:
    
        attributeName = "hahahahahahahaha" # No attribute named as "hahahahahahahaha".
        
Response:
    
    "Delete module attribute failed! - 
        {
            "statusCode":791006,
            "statusDescription":"attribute hahahahahahahaha does not exist."
        }" 
        
###################################################################################################################    

"""Error 3: duplicate deletion"""

Input:
    
        attributeName = "hahahahahahahaha" # No attribute named as "hahahahahahahaha".
        
Response:
    
    "Delete module attribute failed! - 
        {
            "statusCode":791006,
            "statusDescription":"attribute Module_newAtt1 does not exist."
        }" 


```
