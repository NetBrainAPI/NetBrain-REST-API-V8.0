
# Tenant Domain Management API Design

## ***DELETE*** /V1/CMDB/Tenants/{tenantId}
Calling this API to delete a tenant specified by tenant id (how to get ID? see Get_All_Tenants).

## Detail Information

> **Title** : Delete Tenant API<br>

> **Version** : 02/05/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Tenants/{tenantId}

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No request body.

## Path Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|tenantId* | string  | The ID of the tenant that you want to delete.  |

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
|tenantId| string | The tenant ID.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
    "statusCode": 790200,
    "statusDescription": "success"
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
token = "855b2da0-306b-4c29-b37f-be09e33e2d02"
nb_url = "http://192.168.28.79"

tenantId = "2492adc4-6014-4c1f-b2d3-be4dadc4dd3e"

full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Tenants/" + str(tenantId)
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

try:
    response = requests.delete(full_url, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Delete tenant failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X GET \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Tenants/2492adc4-6014-4c1f-b2d3-be4dadc4dd3e \
  -H 'Postman-Token: 834d6596-2b8f-45d7-bb1c-a318576e7a78' \
  -H 'cache-control: no-cache' \
  -H 'token: 855b2da0-306b-4c29-b37f-be09e33e2d02'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
        
           tenantId = ""
        
Response:
    
        "Delete tenant failed! - 
            {
                "statusCode" : 793405,
                "statusDescription" : "Method is not supported"
            }"

###################################################################################################################    

"""Error 1: wrong inputs"""

Input:
        
           tenantId = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" # Not tenant called "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX".
        
Response:
    
        "Delete tenant failed! - 
            {
                "statusCode":791006,
                "statusDescription":"tenant with id XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX does not exist."
            }"

```
