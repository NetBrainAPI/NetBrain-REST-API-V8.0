
# Role API Design

## ***PUT*** /V1/CMDB/Path/Calculation
Call this API to update the information and privileges of the specified role by current "roleName".

## Detail Information

> **Title** : Update Role API<br>

> **Version** : 02/01/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Roles

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|roleName* | string  | The current name of the role. Identification key to search the existing role.  |
|newRoleName | string  | A new name of the role. This field is optional. No change will be made if newRoleName, description, privileges fields are all null or empty.  |
|description | string  | The description of the role. This field is optional.  |
|privileges | list of integer  | The privileges that the role owns. See roles and privileges for more details. Detail information please check Roles and Privileges Table in [Add Role API](https://github.com/NetBrainAPI/NetBrain-REST-API-V8.0/blob/master/REST%20APIs%20Documentation/User%20Roles%20Management/Add%20Role%20API.md) |

> ***Example***


```python
{
      "newRoleName": "newRoleName",
      "roleName": "RoleName",
      "description": "string",
      "privileges": [2, 5, ...]
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
token = "005fd6cc-cf08-4742-985b-902503dad2a4"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Roles"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

roleName = "testRole11"
newRoleName = ""
description = ""
privileges = [24]

body = {
        "newRoleName": newRoleName, 
        "roleName": roleName, 
        "description": description, 
        "privileges": privileges
    }

try:
    response = requests.put(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Add Role failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

# cURL Code from Postman


```python
curl -X PUT \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Roles \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: afc35fb3-b899-43c6-a421-6925ffcf15ec' \
  -H 'cache-control: no-cache' \
  -H 'token: 005fd6cc-cf08-4742-985b-902503dad2a4' \
  -d '{
          "newRoleName": "newRoleName",
          "roleName": "testRole11",
          "description": "string",
          "privileges": [2, 5]
    }'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
        
        roleName = "" # Can not be null.
        newRoleName = ""
        description = ""
        privileges = []

Response:
    
    "Add Role failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'roleName' cannot be null."
        }"

###################################################################################################################    

"""Error 2: wrong inputs"""

Input:
        
        roleName = "hahahah" # Role named as"hahahah" not exist.
        newRoleName = ""
        description = ""
        privileges = []

Response:
    
    "Add Role failed! - 
        {
            "statusCode":791006,
            "statusDescription":"roleName does not exist."
        }"
 
###################################################################################################################    

"""Error 3: wrong inputs type value"""

# same with "Add Role API"

###################################################################################################################    

"""Error 4: duplicate "roleName" """""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
        
        roleName = 2 
        newRoleName = 2 # "roleName" and "newRoleName" can not be same in one API calling ? 
        description = 2
        privileges = []


Response:
    
    "Add Role failed! - 
    {
        "statusCode":791007,
        "statusDescription":"newRoleName already exists."
    }"
    
###################################################################################################################    

"""Error 5: wrong "privileges" number provided"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
        
        roleName = 2 
        newRoleName = ""
        description = 2
        privileges = [7]


Response:
    
    "Add Role failed! - 
        {
            "statusCode":791001,
            "statusDescription":"Invalid parameter: the parameter 'privilege 7' is invalid."
        }"
```
