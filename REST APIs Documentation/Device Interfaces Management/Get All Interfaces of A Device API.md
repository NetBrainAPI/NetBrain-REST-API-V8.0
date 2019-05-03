
# Interface API Design

## ***GET*** /V1/CMDB/Interfaces/{?hostname}
Call this API to get all interfaces of one device which specified by hostname.

## Detail Information

> **Title** : Get All Interfaces of a Device API<br>

> **Version** : 01/29/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Interfaces

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No request body.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|hostname*| string | Device hostname.  |

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
|interfaces| list of string | A list collection of interface names in one device.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
    "interfaces": [
        "Loopback0",
        "Ethernet0/0",
        "Ethernet0/1",
        "Ethernet0/2",
        "Ethernet0/3",
        "Ethernet1/0",
        "Ethernet1/1",
        "Ethernet1/2",
        "Ethernet1/3",
        "Ethernet2/0",
        "Ethernet2/1",
        "Ethernet2/2",
        "Ethernet2/3",
        "Ethernet3/0",
        "Ethernet3/1",
        "Ethernet3/2",
        "Ethernet3/3",
        "Ethernet4/0",
        "Ethernet4/1",
        "Ethernet4/2",
        "Ethernet4/3",
        "Vlan5",
        "Vlan55"
    ],
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
token = "ff389578-87ef-45b6-b42c-7bd98c91d1a9"
nb_url = "http://192.168.28.79"

hostname = "SW5"
#data = {"hostname" : hostname}

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token
full_url= nb_url + "/ServicesAPI/API/V1/CMDB/Interfaces"

try:
    # Do the HTTP request
    response = requests.get(full_url, headers=headers, params = {"hostname" : hostname}, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        result = response.json()
        print (result)
    else:
        print ("Get interfaces failed! - " + str(response.text))

except Exception as e:
    print (str(e)) 
```

    {'interfaces': ['Loopback0', 'Ethernet0/0', 'Ethernet0/1', 'Ethernet0/2', 'Ethernet0/3', 'Ethernet1/0', 'Ethernet1/1', 'Ethernet1/2', 'Ethernet1/3', 'Ethernet2/0', 'Ethernet2/1', 'Ethernet2/2', 'Ethernet2/3', 'Ethernet3/0', 'Ethernet3/1', 'Ethernet3/2', 'Ethernet3/3', 'Ethernet4/0', 'Ethernet4/1', 'Ethernet4/2', 'Ethernet4/3', 'Vlan5', 'Vlan55'], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Interfaces?hostname=SW5' \
  -H 'Postman-Token: 384702d0-8a7d-4fd8-80d0-9fc40dc839ad' \
  -H 'cache-control: no-cache' \
  -H 'token: ff389578-87ef-45b6-b42c-7bd98c91d1a9'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty hostname"""

Input:
    
        hostname = ""

Response:
    
    "Get interfaces failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'hostname' cannot be null."
        }"

###################################################################################################################    

"""Error 2: hostname not exist in current domain"""

Input:
    
        hostname = "blahblahblah"

Response:
    
    "Get interfaces failed! - 
        {
            "statusCode":791006,
            "statusDescription":"device blahblahblah does not exist."
        }"

```
