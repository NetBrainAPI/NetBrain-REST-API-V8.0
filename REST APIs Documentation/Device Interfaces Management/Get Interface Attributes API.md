
# Interface API Design

## ***GET*** /V1/CMDB/Interface/Attributes/{?hostname}/{?interfaceName}/{?attributeName}
Call this API to get the value for a specified attribute of a device interface, get all attributes if the attribute name is not specifed.

## Detail Information

> **Title** : Get Interface Attribute(s) API<br>

> **Version** : 01/29/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Interfaces/Attributes/{?hostname}/{?interfaceName}/{?attributeName}

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
|interfaceName* | string  | Input the full name of the interface. |
|attributeName | string  | Optional. The name of the attribute that you want to get its value,, get all attributes if the attribute name is not specifed.<br>Please note that the attribute name here is case sensitive and not the name displayed in the Device Details pane of NetBrain IE system. See Applicable Interface Attributes for system built-in interface attributes.  |

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
|attributeName | string | The name of the attribute.  |
|interfaceAttributeValue| string | The returned attribute value. |
|hostname| string | The hostname of the returned device. |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
    "hostname": "R1",
    "attributes": {
        "Ethernet0/3": {
            "name": "Ethernet0/3",
            "ips": "",
            "ipv6s": "",
            "ipv6LinkLocalAddress": "",
            "mibIndex": 4,
            "bandwidth": 10000,
            "speed": "",
            "duplex": "halfDuplex",
            "intfStatus": "up/up",
            "macAddr": "aabb.cc00.0530",
            "moduleSlot": "",
            "moduleType": "",
            "descr": "",
            "routingProtocol": "",
            "multicastMode": "",
            "mplsVrf": "",
            "inAclName": "",
            "outAclName": "",
            "mode": "",
            "vlan": "",
            "trunkNativeVlan": "",
            "trunkEncapsulation": "",
            "ipUnnumberedIp": "",
            "ss": ""
        }
    },
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
token = "b56ed962-8ccd-4b2d-a7c1-7d97fff51321"
nb_url = "http://192.168.28.79"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Interfaces/Attributes"

hostname = "R1"
interfaceName = ""
attributeName = ""

data = {
        "hostname":hostname,
        "interfaceName":interfaceName, 
        "attributeName":attributeName
    }

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get interface attributes failed! - " + str(response.text))

except Exception as e:
    print (str(e))  
```

    {'hostname': 'R1', 'attributes': {'Ethernet0/0': {'name': 'Ethernet0/0', 'ips': '', 'ipv6s': '', 'ipv6LinkLocalAddress': '', 'mibIndex': 1, 'bandwidth': 10000, 'speed': '', 'duplex': 'halfDuplex', 'intfStatus': 'up/up', 'macAddr': 'aabb.cc00.0500', 'moduleSlot': '', 'moduleType': '', 'descr': '', 'routingProtocol': '', 'multicastMode': '', 'mplsVrf': '', 'inAclName': '', 'outAclName': '', 'mode': '', 'vlan': '', 'trunkNativeVlan': '', 'trunkEncapsulation': '', 'ipUnnumberedIp': '', 'ss': ''}, 'Ethernet0/1': {'name': 'Ethernet0/1', 'ips': [{'ip': 2064253189, 'ipLoc': '123.10.1.5/30', 'maskLen': 30}], 'ipv6s': '', 'ipv6LinkLocalAddress': '', 'mibIndex': 8, 'bandwidth': 10000, 'speed': '', 'duplex': 'halfDuplex', 'intfStatus': 'up/up', 'macAddr': 'aabb.cc00.0510', 'moduleSlot': '', 'moduleType': '', 'descr': '', 'routingProtocol': 'OSPF 12345 AREA 0', 'multicastMode': '', 'mplsVrf': '', 'inAclName': '', 'outAclName': '', 'mode': '', 'vlan': '', 'trunkNativeVlan': '', 'trunkEncapsulation': '', 'ipUnnumberedIp': '', 'ss': ''}, 'Ethernet0/2': {'name': 'Ethernet0/2', 'ips': [{'ip': 2064253186, 'ipLoc': '123.10.1.2/30', 'maskLen': 30}], 'ipv6s': '', 'ipv6LinkLocalAddress': '', 'mibIndex': 9, 'bandwidth': 10000, 'speed': '', 'duplex': 'halfDuplex', 'intfStatus': 'up/up', 'macAddr': 'aabb.cc00.0520', 'moduleSlot': '', 'moduleType': '', 'descr': '', 'routingProtocol': 'OSPF 12345 AREA 0', 'multicastMode': '', 'mplsVrf': '', 'inAclName': '', 'outAclName': '', 'mode': '', 'vlan': '', 'trunkNativeVlan': '', 'trunkEncapsulation': '', 'ipUnnumberedIp': '', 'ss': ''}, 'Ethernet0/3': {'name': 'Ethernet0/3', 'ips': '', 'ipv6s': '', 'ipv6LinkLocalAddress': '', 'mibIndex': 4, 'bandwidth': 10000, 'speed': '', 'duplex': 'halfDuplex', 'intfStatus': 'up/up', 'macAddr': 'aabb.cc00.0530', 'moduleSlot': '', 'moduleType': '', 'descr': '', 'routingProtocol': '', 'multicastMode': '', 'mplsVrf': '', 'inAclName': '', 'outAclName': '', 'mode': '', 'vlan': '', 'trunkNativeVlan': '', 'trunkEncapsulation': '', 'ipUnnumberedIp': '', 'ss': ''}, 'Loopback0': {'name': 'Loopback0', 'ips': [{'ip': 2063663361, 'ipLoc': '123.1.1.1/32', 'maskLen': 32}], 'ipv6s': '', 'ipv6LinkLocalAddress': '', 'mibIndex': 7, 'bandwidth': 8000000, 'speed': '', 'duplex': '', 'intfStatus': 'up/up', 'macAddr': '', 'moduleSlot': '', 'moduleType': '', 'descr': '', 'routingProtocol': 'OSPF 12345 AREA 0', 'multicastMode': '', 'mplsVrf': '', 'inAclName': '', 'outAclName': '', 'mode': '', 'vlan': '', 'trunkNativeVlan': '', 'trunkEncapsulation': '', 'ipUnnumberedIp': '', 'ss': ''}}, 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Interfaces/Attributes?hostname=R1&interfaceName=Ethernet0/3' \
  -H 'Postman-Token: f049a497-1d47-40d9-b76b-8b03f2607b21' \
  -H 'cache-control: no-cache' \
  -H 'token: ff389578-87ef-45b6-b42c-7bd98c91d1a9'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty hostname"""

Input:
    
        hostname = ""
        interfaceName = ""
        attributeName = ""

Response:
    
    "Get interfaces failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'hostname' cannot be null."
        }"
            
###################################################################################################################    

"""Error 2: wrong hostname"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    
        hostname = "RRRRRRRRRR1" # Should be "R1"
        interfaceName = ""
        attributeName = ""

Response:
    
                    {
                        "statusCode": 790200,
                        "statusDescription": "Success."
                    }
```
