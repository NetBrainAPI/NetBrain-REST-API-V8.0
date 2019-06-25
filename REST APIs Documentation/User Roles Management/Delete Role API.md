
# Role API Design

## ***DELETE*** /V1/CMDB/Roles/{roleName}
Call this API to add a new role into Netbrain system and grant privileges to this role.

## Detail Information

> **Title** : Delete Role API<br>

> **Version** : 02/01/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Roles/{roleName}

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

> No request body.

## Path Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|roleName* | string  | The current name of the role. Key filed to search the existing role.  |

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
token = "005fd6cc-cf08-4742-985b-902503dad2a4"
nb_url = "http://192.168.28.79"

roleName = "testRole1"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Roles/" + str(roleName)
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

try:
    response = requests.delete(full_url, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Delete Role failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X DELETE \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Roles/testRole1 \
  -H 'Postman-Token: c8d5df24-a484-4a37-b833-fbd2b0d4fe5c' \
  -H 'cache-control: no-cache' \
  -H 'token: 005fd6cc-cf08-4742-985b-902503dad2a4'
```

# Error Examples:


```python
# Same with "Add Role API" mostly.

###################################################################################################################    

"""Error 2: wrong inputs"""

Input:
        
        roleName = "111111111111111111" # Role named as "111111111111111111" not exist.
        

Response:
    
    "Delete Role failed! - 
        {
            "statusCode": 791006,
            "statusDescription": "roleName 111111111111111111 does not exist."
        }"

```
