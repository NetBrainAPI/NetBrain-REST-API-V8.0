
# Authentication and Authorization API
OpenAPI version 1.0.0

## ***POST*** /V1/Session

 This method creates an authentication token and starts a session with user's body information and netbrain server url.


## Detail Information

> **Title** : Log in API<br>

> **Version** : 01/23/2019.

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session

> **Authentication** : Not Required.


## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|username* | string  | the username to log into your NetBrain domain.  |
|password* | string  | the password to log into your NetBrain domain.  |
|authentication_id | string  | This body parameter is only required for an external user through LDAP/AD or TACACS and the value must same with the name of external authentication which the user created by admin role during system management under "User Account" section. |

> ***Example*** : 


```python
body = {
          "username": "NetBrain",
          "password": "NetBrain"
       }

```

## Parameters(****required***)

>No parameters required.


## Headers

**Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |


## Response

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|token | string | The returned authentication token.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code. |
 
> ***Example*** :


```python
{
    'token': 'fc6bc6ea-a46a-4e9b-8906-c623f78474b6',
    'statusCode': 790200,
    'statusDescription': 'Success.'
}
```

 ## Full Example : 


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

body = {
    "username" : "Netbrain",      
    "password" : "Netbrain"  
}
    
full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"           

# Set proper headers
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}    

try:
    # Do the HTTP request
    response = requests.post(full_url, headers=headers, data = json.dumps(body), verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Get token failed! - " + str(response.text))
except Exception as e:
    print (str(e))
    
```

    {'token': '9b9715e8-7274-4a28-9692-e00ad315a283', 'statusCode': 790200, 'statusDescription': 'Success.'}
    

> ### Example For External user


```python
body = {
    "username" : "Netbrain",      
    "password" : "Netbrain",
    "authentication_id" : "net-brain" 
}
    
full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"           

# Set proper headers
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}    

try:
    # Do the HTTP request
    response = requests.post(full_url, headers=headers, data = json.dumps(body), verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Get token failed! - " + str(response.text))
except Exception as e:
    print (str(e))
    
```

    {'token': '5e9af6f4-efa8-4a19-9d42-add069c67c99', 'statusCode': 790200, 'statusDescription': 'Success.'}
    

 # cURL Code from Postman


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/Session \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: ba5d854d-80ec-4a63-be98-65dc92c74a7a' \
  -H 'cache-control: no-cache' \
  -d '{
    "username": "Netbrain",
    "password": "Netbrain"
    }'
```

# Error Example : 


```python
###################################################################################################################    

"""Error 1: empty url"""

Input:
    body = {
        "username" : "NetBrain",      
        "password" : "NetBrain"  
    }
    
    full_url = ""  

Response:
    "Invalid URL '': No schema supplied. Perhaps you meant http://?"
    
###################################################################################################################    

"""Error 2: wrong url"""

Input:
    body = {
        "username" : "NetBrain",      
        "password" : "NetBrain"  
    }
    
    full_url = "http://IP address of your NetBrain Web API ServerXXXXXXXXXXX%%%%%%%%/ServicesAPI/API/V1/Session"  

Response:
    """HTTPConnectionPool(host='192.1688.28.79', port=80): 
       Max retries exceeded with url: /ServicesAPI/API/V1/Session (Caused by NewConnectionError(
           '<urllib3.connection.HTTPConnection object at 0x0000022F203C79B0>: 
           Failed to establish a new connection: [Errno 11001] getaddrinfo failed'))"""
    
################################################################################################################### 

"""Error 3: empty body"""

Input:
    body = {
        "username" : "",      
        "password" : ""  
    }
    
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"  

Response:
    "Get token failed! - {"statusCode":795000,"statusDescription":"Invalid username or password."}"
    
################################################################################################################### 

"""Error 4: wrong body information"""

Input:
    body = {
        "username" : "wwwwwww",      
        "password" : "wwwwwww"  
    }
    
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"  

Response:
    "Get token failed! - {"statusCode":795000,"statusDescription":"Invalid username or password."}"
    
################################################################################################################### 

"""Error 4: for external user, empty authentication id"""

Input:
    body = {
        "username" : "Netbrain",      
        "password" : "Netbrain",
        "authentication_id" : ""
    }
    
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"  

Response:
    {
        "statusCode": 795000,
        "statusDescription": "Invalid username or password."
    }
    
################################################################################################################### 

"""Error 4: for external user, wrong authentication id"""

Input:
    body = {
        "username" : "Netbrain",      
        "password" : "Netbrain",
        "authentication_id" : "XXXXXXXXX"
    }
    
    full_url = "http://IP address of your NetBrain Web API Server/ServicesAPI/API/V1/Session"  

Response:
    {
        "statusCode": 795000,
        "statusDescription": "Invalid username or password."
    }
```
