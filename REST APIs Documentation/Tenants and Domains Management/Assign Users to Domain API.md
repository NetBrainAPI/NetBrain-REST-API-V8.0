
# Tenant Domain Management API Design

## ***POST*** /V1/CMDB/Domains/Users
Calling this API to assign users and roles to a specified domain. Make sure that the user has at least access privilege to the tenant that this domain belongs to.

## Detail Information

> **Title** : Assign Users to Domain API<br>

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
|users.userName* | string | The names of the users that you want to assign.|
|users.roles | list of int | Roles of user. Could be more than one role, seperated by comma(","). The following roles are valid for domain users.br>---domainAdmin = 2<br>---domainUser = 5<br>---powerUser = 6<br>---engineer = 7<br>---guest = 8<br>---networkChangeCreator = 9<br>---networkChangeExecutor =10|

> ***Example***


```python
{
  "domainId": "3234bc43-d992-de31-3163-d388ed14d45c",
  "users": [
    {
      "userName": "user1",
      "roles": [2,6,9]
    },
    {
      "userName": "user2",
      "roles": [8,9,10]
    }
  ]
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
token = "220d6462-ba64-4058-83cb-affb2d55de78"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Domains/Users"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

domainId = "bcae6de1-55b3-45c2-9dbd-ced9bf14b1e0"
userName = "gdluserTest"
roles = []

body = {
  "domainId": domainId,
  "users": [
        {
          "userName": userName,
          "roles": roles
        }
    ]
}

try:
    response = requests.post(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Assign users to domain failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Domains/Users \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 2e001036-43c5-43b7-b94f-24c6517732a4' \
  -H 'cache-control: no-cache' \
  -H 'token: 0930eb5a-133d-46cf-93cf-2ab15fd858dd' \
  -d '{
      "domainId": "bcae6de1-55b3-45c2-9dbd-ced9bf14b1e0",
      "users": [
            {
              "userName": "gongdaiAdmin",
              "roles": [2]
            }
        ]
    }'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
        
        domainId = ""
        userName = "" # Can not be null
        roles = [] 
        
Response:
    
    "Assign users to domain failed! - 
    {
        "statusCode":791000,
        "statusDescription":"Null parameter: the parameter 'UserName' cannot be null."
    }"
    
#-----------------------------------------------------------------------------------------------------------------

Input:""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
        
        domainId = ""
        userName = "gongdaiAdmin" # Can not be null
        roles = [] 
        
Response:
    
        "{
            'statusCode': 790200, 
            'statusDescription': 'Success.'
        }"
        
###################################################################################################################    

"""Error 2: wrong domain ID inputs"""

Input:
        
        domainId = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" # no domain has a Id like this
        userName = "gongdaiAdmin"
        roles = []
        
Response:
    
    "Assign users to domain failed! - 
        {
            "statusCode":791006,
            "statusDescription":"domain with id XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX does not exist."
        }"
        
###################################################################################################################    

"""Error 3: random user name inputs"""

Input:
        
        domainId = "bcae6de1-55b3-45c2-9dbd-ced9bf14b1e0"
        userName = "brainnet" # There is no user called "brainnet".
        roles = []
        
Response:
    
    "Assign users to domain failed! - 
        {
            "statusCode":794011,
            "statusDescription":"Operation failed. Reason: User 'brainnet' does not exist or not a domain user"
        }"
        
###################################################################################################################    

"""Error 4: random role list inputs"""

Input:
        
        domainId = "bcae6de1-55b3-45c2-9dbd-ced9bf14b1e0"
        userName = "gongdaiAdmin"
        roles = [2,100,1000,10000] # the role numbers are 2, 5, 6, 7, 8, 9 and 10.
        
Response:
    
    "Assign users to domain failed! - 
        {
            "statusCode":794011,
            "statusDescription":"Operation failed. Reason: User 'brainnet' does not exist or not a domain user"
        }"
```
