
# Tenant Domain Management API Design

## ***POST*** /V1/CMDB/Tenants/Users
Calling this API to assign users to a specified tenant.

## Detail Information

> **Title** : Assign Users to Tenant API<br>

> **Version** : 02/05/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Tenants/Users	

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|tenantId* | string  | The Id of the tenant. This field is optional.<br>▪ If this parameter presents, its value will be used;<br>▪ If not, this parameter uses the value of tenantId in the SetCurrentDomain;<br>▪ If this parameter cannot get a value by either of the above ways, an error will prompt.  |
|users* | list of object  | List of user info.  |
|users.userName* | string  | The names of the users that you want to assign.  |
|users.isTenantAdmin | bool  | Determine whether the assigned user is an admin. This field is optional.  |

> ***Example***


```python
{
    "tenantId": "4e75247a-309c-4231-96a5-823b6cb1e78d",
    "users": {
        "username": "NetBrain",
        "isTenantAdmin": "NetBrain"
    }
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
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Tenants/Users"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

tenantId = "6f4e381a-1752-4b84-8a59-6ed6391614cf"
username = "gongdaiAdmin"
isTenantAdmin = True

body = {
        "tenantId": tenantId, 
        "users": [{
                "username": username,
                "isTenantAdmin": isTenantAdmin
            }]
    }

try:
    response = requests.post(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Assign users to tenant failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Tenants/Users \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: fced702d-d5ef-4516-b811-8882b167650c' \
  -H 'cache-control: no-cache' \
  -H 'token: 855b2da0-306b-4c29-b37f-be09e33e2d02' \
  -d '{
        "tenantId": "6f4e381a-1752-4b84-8a59-6ed6391614cf", 
        "users": [{
                "username": "gongdaiAdmin",
                "isTenantAdmin": "True"
            }]
    }'
```

 # Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
        
        tenantId = "6f4e381a-1752-4b84-8a59-6ed6391614cf"  # Can not be null
        username = "gongdaiAdmin"  # Can not be null
        isTenantAdmin = None  # Can be null
        
Response:
    
        "Assign users to tenant failed! - 
        {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'userName' cannot be null."}

        
        """"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
        Assign users to tenant failed! - 
        {"statusCode":791004,"statusDescription":"Invalid tenant id."}"

###################################################################################################################    

"""Error 1: wrong inputs"""

Input:
        
        tenantId = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"  # No tenant have a ID like this
        
Response:
    
        "Assign users to tenant failed! - 
            {
                "statusCode":791006,
                "statusDescription":"tenant with id XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX does not exist."
            }"

 #-----------------------------------------------------------------------------------------------------------------
            
Input:""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
        
        username = "blahblah"  # No user called "blahblah"
        
Response:
    
            "{
                'statusCode': 790200, 
                'statusDescription': 'Success.'
            }
       
 #-----------------------------------------------------------------------------------------------------------------
            
Input:""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
        
        isTenantAdmin = "hahahahahahah"  # It should be "True" or "False"
        
Response:
    
            "{
                'statusCode': 790200, 
                'statusDescription': 'Success.'
            }"
            
            

```
