
# User API Design

## ***POST*** /V1/CMDB/Users
Call this API to create a new user account in Netbrain system.

## Detail Information

> **Title** : Add User API<br>

> **Version** : 02/01/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Users

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|username* | string  | The user name. This parameter is required.  |
|email* | string  | The email address of the user. This parameter is required. |
|firstName* | string  | The first name of the user. This parameter is required.  |
|lastName* | string  | The last name of the user. This parameter is required. |
|password* | string  | The login password. The allowed length is 6-128 characters by default. This parameter is required.  |
|authenticationType | integer |The authentication type for the user account.<br>▪ 1 - Local<br>▪ 2 - External|
|phoneNumber | string |The phone number of the user.|
|department | string |The department that the user belongs to.|
|description | string |Any description about the account.|
|allowChangePassword | bool |Specify whether to allow the user to change password independently. This parameter is required.|
|deactivatedTime | string |Specify the time when the account is expired.|
|isSystemAdmin | bool |Decide whether to allocate system administrator role to the user. This parameter is required.|
|tenants | list of tenant object |Specify a list of tenants for the user.<br>Only required if the parameter isSystemAdmin is false.<br>▪ tenantName (string) - the tenant that the user can access.<br>▪ isTenantAdmin (bool) - decide whether to allocate the tenant administrator role to the user. If false, you need to specify a domain for the user to access.<br>▪ allowCreateDomain (bool) - decide whether to allow the user to create domains.<br>▪ domains - required only if the parameter isTenantAdmin is false.<br>---domainName - the domain name.<br>---domainRoles - the role of the domain user.|

> ***Example***


```python
{
      "username": "NetBrain",
      "email": "NetBrain@netbrain.com",
      "firstName": "NetBrain",
      "lastName": "NetBrain",
      "password": "NetBrain",
      "authenticationType": 1 or 2,
      "phoneNumber": "string",
      "department": "string",
      "description": "string",
      "allowChangePassword": True or False,
      "deactivatedTime": "string",
      "isSystemAdmin": True or False,
      "tenants": [tenants objects ...]
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
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Users"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

username = "NetBrain2"
email = "NetBrain2@netbrain.com"
firstName = "NetBrain"
lastName = "NetBrain"
password = "NetBrain"
authenticationType = 1 
phoneNumber = "string"
department = "string"
description = "string"
allowChangePassword = True
deactivatedTime = "string"
isSystemAdmin = False
tenants = []

body = {
        "username": username,
        "email": email,
        "firstName": firstName,
        "lastName": lastName,
        "password": password,
        "authenticationType" : authenticationType,
        "phoneNumber" : phoneNumber,
        "department" : department,
        "description" : description,
        "allowChangePassword": allowChangePassword,
        "deactivatedTime" : deactivatedTime,
        "isSystemAdmin":isSystemAdmin,
        "tenants" : tenants
       }

try:
    response = requests.post(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Add New User failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Users \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 9da895b2-18a4-46c0-a18f-cb5e7877fefb' \
  -H 'cache-control: no-cache' \
  -H 'token: 005fd6cc-cf08-4742-985b-902503dad2a4' \
  -d '{
        "username": "NetBrain1",
        "email": "NetBrain1@netbrain.com",
        "firstName": "NetBrain",
        "lastName": "NetBrain",
        "password": "NetBrain",
        "authenticationType" : 1,
        "phoneNumber" : "string",
        "department" : "string",
        "description" : "string",
        "allowChangePassword": "True",
        "deactivatedTime" : "string",
        "isSystemAdmin":"True",
        "tenants" : []
    }'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
        
        username = "" # Can not be null.
        email = "" # Can not be null.
        firstName = "" # Can not be null.
        lastName = "" # Can not be null.
        password = "" # Can not be null.
        authenticationType = None 
        phoneNumber = ""
        department = ""
        description = ""
        allowChangePassword = None # Can not be null.
        deactivatedTime = ""
        isSystemAdmin = None # Can not be null.
        tenants = []


Response:
    
    # Null parameters checking sequence.
    
    "Add New User failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'username' cannot be null."}

    Add New User failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'email' cannot be null."}

    Add New User failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'firstName' cannot be null."}

    Add New User failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'lastName' cannot be null."}

    Add New User failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'allowChangePassword' cannot be null."}

    Add New User failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'isSystemAdmin' cannot be null."}

    Add New User failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'password' cannot be null."}"
    
###################################################################################################################    

"""Error 2: wrong inputs""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    
        username = 1 # should be string
        email = "1@netbraintech.com"
        firstName = "Net"
        lastName = "brain"
        password = "Netbrain"
        authenticationType = None 
        phoneNumber = ""
        department = ""
        description = ""
        allowChangePassword = True
        deactivatedTime = ""
        isSystemAdmin = True
        tenants = []

Response:
    
        "{
            'statusCode': 790200, 
            'statusDescription': 'Success.'
        }"
        
###################################################################################################################    

"""Error 3: wrong format email input""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    
        email = "@netbraintech.com"
        

Response:
    
        "Add New User failed! - 
        {
            "statusCode":791001,
            "statusDescription":"Invalid parameter: the parameter 'email' is invalid."
        }"
        
###################################################################################################################    

"""Error 4: duplicate email input""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    
        email = "Netbrain4@netbraintech.com" # Another existing user already use this email. Which means users can not 
                                             # register two user acounts with same email address.
        

Response:
    
        "Add New User failed! - 
            {
                "statusCode":791007,
                "statusDescription":"email NetBrain4@netbraintech.com already exists."
            }"
            
###################################################################################################################    

"""Error 5: duplicate username input""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    
        username = "Netbrain" # Another existing user already use this name.
        

Response:
    
        "Add New User failed! - 
            {
                "statusCode":791007,
                "statusDescription":"user Netbrain already exists."
            }"
```
