
# Device API Design

## ***GET*** /V1/CMDB/Devices/Attributes/{hostname}/FrontServer
Call this API to set a value for the specified attriute of a device.

* ## Detail Information

> **Title** : Get Front Server of a device API<br>

> **Version** : 01/28/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Devices/{hostname}/FrontServer

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

* ## Request body(****required***)

>No request body.

* ## Path Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|hostname* | string  | The hostname of the target device. |

* ## Headers

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

* ## Response

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|alias | string | the alias of the front server  |
|ipOrHostname| string | the ip or hostname of the front server  |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***


```python
{
    "statusCode": 793404,
    "statusDescription": "No resource"
}
```

* # Full Example:


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
hostname = "R10"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token
full_url= nb_url + "/ServicesAPI/API/V1/CMDB/Devices/" + str(hostname) + "/FrontServer"
try:
    response = requests.get(full_url, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get front server failed! - " + str(response.text))

except Exception as e:
    print (str(e))   
```

    Get front server failed! - {"statusCode":793404,"statusDescription":"No resource"}
    

* # cURL Code from Postman


```python
curl -X GET \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Devices/AS20002/FrontServer \
  -H 'Postman-Token: c873eb5b-7924-49ed-8609-ea5e215b4e52' \
  -H 'cache-control: no-cache' \
  -H 'token: 13c7ed6e-781d-4b22-83e7-b1722de4e31d'
```
