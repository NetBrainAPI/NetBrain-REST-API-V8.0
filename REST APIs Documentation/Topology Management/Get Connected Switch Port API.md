
# Topology API Design

## ***GET*** /V1/CMDB/Topology/Devices/{ip}/ConnectedSwitchPort
Call this API to get the connected layer 2 switch port whihc specified by management.

## Detail Information

> **Title** : Get Connected Switch Port API<br>

> **Version** : 02/01/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Topology/Devices/{ip}/ConnectedSwitchPort

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No request body.

## Path Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|ip* | string  | The IP address of a end system. |

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
|hostname| string | Hostname of the device that the endsystem connected to.  |
|interface| string | Interface of the device that the endsystem connected to. |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***


```python
{
      "statusCode": 790200,
      "statusDescription": "success",
      "hostname": "Bos-SW",
      "interface": "Ethernet 0/1"
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
token = "3d0f475d-dbae-4c44-9080-7b08ded7d35b"
nb_url = "http://192.168.28.79"
 
ip = "10.8.1.25" 

full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Topology/Devices/"+str(ip)+"/ConnectedSwitchPort"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

try:
    response = requests.get(full_url, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get Connected Switch Port failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'hostname': 'US-BOS-SW3', 'interface': 'Ethernet0', 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X GET \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Topology/Devices/192.168.28.204/ConnectedSwitchPort \
  -H 'Postman-Token: dc0db983-5679-48fc-9748-dede6fb4644a' \
  -H 'cache-control: no-cache' \
  -H 'token: 3d0f475d-dbae-4c44-9080-7b08ded7d35b'
```


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
        
        ip = "" # Can not be null.

Response:
    
        "Get Connected Switch Port failed! - 
            { 
                "statusCode":793404,
                "statusDescription":"No resource"
            }"
            
###################################################################################################################    

"""Error 1: wrong inputs"""

Input:
        
        ip = "192.168.28.204" # Not a useful interface ip.

Response:
    
        "Get Connected Switch Port failed! -
            {
                "statusCode": 792011,
                "statusDescription": "Cannot find the connected switches by the input IP address."
            }"
```
