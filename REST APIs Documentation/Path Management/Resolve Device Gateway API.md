
# Path API Design

## ***GET*** /V1/CMDB/Path/Gateways
Call this API to retrieve the gateways for a device used as path source.

## Detail Information

> **Title** : Resolve Device Gateway API<br>

> **Version** : 01/30/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Path/Gateways

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

> No request body.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| ipOrHost* | string  | Device mgmIp or hostname, used as the source in path calculation. |

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
|gatewayList| List of Object | Gateway list.  |
|gatewayList.devName| string | Device name of gateway.  |
|gatewayList.intfName| string | Interface name of gate way.  |
|gatewayList.ip| string | Ip address of gate way.  |


> ***Example***


```python
{
    "gatewayList": [
        {
            "ip": "123.10.1.2",
            "devName": "R1",
            "intfName": "Ethernet0/2"
        }
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
token = "34713abf-71d6-41c6-8fd4-d3b8f3066959"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Path/Gateways"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

ipOrHost = "R3"

data = {"ipOrHost":ipOrHost}

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Create module attribute failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'gatewayList': [{'ip': '10.120.15.5', 'devName': 'R3', 'intfName': 'Ethernet0/3.15'}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

 # cURL Code form Postman:


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Path/Gateways?ipOrHost=10.1.13.2' \
  -H 'Postman-Token: 67f40439-549a-4689-929e-26d9651597ad' \
  -H 'cache-control: no-cache' \
  -H 'token: c4edcb21-8d27-42a3-be0c-7e3b53b608c7'
```

 # Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
    ipOrHost = ""
    
Response:
    
    "Create module attribute failed! - 
        {
            "statusCode":791000,
            statusDescription":"Null parameter: the parameter 'ipOrHost' cannot be null."
        }"

###################################################################################################################    

"""Error 2: wrong inputs"""

Input:
    
    ipOrHost = "123456789"
    
Response:
    
    "Create module attribute failed! - 
        {
            "statusCode":792040,
            "statusDescription":"Gateway was not found."
        }"
        
```
