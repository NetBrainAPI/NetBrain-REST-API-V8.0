
# Tenant Domain Management API Design

## ***DELETE*** /V1/CMDB/Domains/{domainId}
Calling this API to delete a domain specified by domain id (how to get ID? see Get All Domains of a Tenant).

## Detail Information

> **Title** : Delete Domain API<br>

> **Version** : 02/05/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Domains/{domainId}	

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
|domainId* | string | The ID of the domain that you want to delete.|

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
    "statusDescription": "string"
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

domainId = "668489d7-54d9-41a9-a04e-0283f46e9135"
# Set the request inputs
token = "0930eb5a-133d-46cf-93cf-2ab15fd858dd"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Domains/" + str(domainId)
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

data = {
    "domainId": domainId
}

try:
    response = requests.delete(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Delete domain failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X DELETE \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Domains/668489d7-54d9-41a9-a04e-0283f46e9135 \
  -H 'Postman-Token: 4ff80b5e-f352-43a6-aa55-e4db3a5df769' \
  -H 'cache-control: no-cache' \
  -H 'token: 0930eb5a-133d-46cf-93cf-2ab15fd858dd'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
        
        domainId = "" # Can not be null
        
Response:
    
    "Delete domain failed! - 
        {
            "statusCode":793405,
            "statusDescription":"Method is not supported"
        }"

###################################################################################################################    

"""Error 2: wrong inputs"""

Input:
        
        domainId = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" # no domain has a Id like this
        ##OR##
        # domainId = "668489d7-54d9-41a9-a04e-0283f46e9135" # domain with domainId has been deleted.
        
        
        
Response:
    
    "Delete domain failed! - 
        {
            "statusCode":791006,
            "statusDescription":"domain with id XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX does not exist."
        }"
        
```
