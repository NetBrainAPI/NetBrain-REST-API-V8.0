
# Tenant Domain Management API Design

## ***PUT*** /V1/CMDB/Domains/Users
Calling this API to assign users and roles to a specified Domain.

## Detail Information

> **Title** : Remove Users from Domain API<br>

> **Version** : 02/05/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Domains/Users	

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|domainId* | string | The Id of the domain. This field is optional.<br>▪ If this parameter presents, its value will be used;<br>▪ If not, this parameter uses the value of domainId in the SetCurrentDomain;<br>▪ If this parameter cannot get a value by either of the above ways, an error will prompt.|
|users* | list of object | List of user info.|

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
    "statusDescription": "string",
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
token = "220d6462-ba64-4058-83cb-affb2d55de78"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Domains/Users"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

domainId = "bcae6de1-55b3-45c2-9dbd-ced9bf14b1e0" 
users = [ "gdluserTest"] 

body = {
    "domainId": domainId, 
    "users": users
}

try:
    response = requests.put(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Delete users from domain failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X PUT \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Domains/Users \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 0ee44c87-143a-4eaf-8c96-e5a79e6a14a2' \
  -H 'cache-control: no-cache' \
  -H 'token: 4b78f855-6515-4c1f-b4ae-408bfdedcfad' \
  -d '{
        "domainId": "bcae6de1-55b3-45c2-9dbd-ced9bf14b1e0", 
        "users": [ "gongdaiAdmin"]
    }'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
        domainId = ""  # Can not be null
        users = [""]  # Can not be null
        
Response:
    
    "Delete users from domain failed! - 
    {
        "statusCode":791000,
        "statusDescription":"Null parameter: the parameter 'users' cannot be null."
    }"
    
###################################################################################################################    

"""Error 1: wrong privilege user role"""

Input:
    
        domainId = "9f43080f-d502-4ddb-9c76-006c3ef665ad" 
        users = ["gdluserTest"]  # user as a tenant admin of current tenant or system admin. 
        
Response:
    
    "Delete users from domain failed! - 
    {
        "statusCode":794011,
        "statusDescription":"Operation failed. Reason: 
                            "A user with system or tenant admin permissions is contained in the user list."
    }"
```
