
# Interface API Design

## ***DELETE*** /V1/CMDB/Interfaces/Attributes/{?InterfaceType}/{?attributeName}
Call this API to delete an interface attribute (property).

## Detail Information

> **Title** : Delete Interface Attribute API<br>

> **Version** : 01/30/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Interfaces/Attributes/{?InterfaceType}/{?attributeName}	

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
|InterfaceType* | string  |The interface type that the attribute belongs to, such as intfs (Layer 2 topology type interfaces), ipIntfs (IPv4 L3 topology type interfaces), and ip6Intfs (IPv6 L3 topology type interfaces).<br>The following list introduces the interface types in the system.<br>▪ intfs - Interface, displayed as Physical Interface in GDR. Contains all interfaces parsed from configuration files directly. For example, e0/0, f0/0, vlan10, and so on.<br>▪ ipIntfs - IPv4 Interface,  displayed as ipIntfs in GDR. Contains all interfaces created by NetBrain for IPv4 L3 topology calculation and display. For example, vlan 10 172.24.101.32/24.<br>▪ ip6Intfs - IPv6 Interface, displayed as ip6Intfs in GDR. Contains all interfaces created by NetBrain for IPv6 L3 topology calculation and display. For example, atm4/0 2002:9:9:34::4/64.<br>▪ vpnIntfs - VPN Interface, displayed as vpnIntfs in GDR. Contains all interfaces created by NetBrain for IPsec VPN topology calculation and display. For example, outside peer-ip 172.27.129.114.|
|attributeName* | string  | The name of the attribute that you want to delete. Please note that the attribute name here is case sensitive.  |

> **Note:** Applicable Interface Attributes
> <br>The following table lists the interface properties/attributes (case-sensitive) that you can retrieve by using the GetInterfaceAttribute API.

|**Property Name**|**Display Name in Device Detail Pane**|**Description**|
|------|------|------|
| bandwidth | Bandwidth  | The bandwidth of an interface. |
| descr | Description  | The description of an interface. |
| duplex | Duplex  | The duplex of an interface. |
| inAclName | Inbound ACL  | The inbound ACL of an interface. |
| intfStatus | Live Status  | The state of an interface. |
| ips | IPv4 Address  | The IPv4 address of an interface. |
| ipUnnumberedIp | Trunk Unnumbered IP  | The unnumbered IP address of an interface as an unnumbered interface. |
| ipv6LinkLocalAddress | IPv6 Link Local Address  | The IPv6 local address of an interface. |
| ipv6s | IPv6 Address  | The IPv6 address of an interface. |
| macAddr | MAC Address  | The MAC address of an interface. |
| mibIndex | MIB Index  | The MIB index of an interface. |
| mode | Switchport Mode  | The switchport mode of an interface. |
| moduleSlot | Slot#  | The module slot number of an interface. |
| moduleType | Module Type  | The module type of an interface. |
| mplsVpn | MPLS VPN  | The MPLS VPN configured on an interface.|
| mplsVrf | MPLS VRF  | The name of the VRF configured on an interface. |
| multicastMode | Multicasting Mode  | The multicasting mode of an interface. |
| name | Interface Name  | The name of an interface. |
| outAclName | Outbound ACL  | The outbound ACL of an interface. |
| routingProtocol | Routing Protocol.  | The routing protocol configured on an interface. |
| speed | Speed  | The speed of an interface. |
| trunkEncapsulation | Trunk Encapsulation  | The trunk encapsulation protocol of an interface as trunk port. |
| trunkNativeVlan | Native VLAN  | The native VLAN of an interface as switchport. |
| vlan | VLAN  | The VLAN number that an interface belongs to. |


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
token = "c284e3cc-a8ef-4a33-9582-2772a5295874"
nb_url = "http://192.168.28.79"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Interfaces/Attributes"

interfaceType = "ipIntfs"
attributeName = "newAtt"

data = {
        "interfaceType":interfaceType, 
        "attributeName":attributeName
    }

try:
    response = requests.delete(full_url, params=data, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Delete interface attribute failed! - " + str(response.text))
    
except Exception as e:
    print (str(e))  

```

    Delete interface attribute failed! - {"statusCode":791006,"statusDescription":"attribute newAtt does not exist."}
    

# cURL Code from Postman


```python
curl -X DELETE \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Interfaces/Attributes?interfaceType=ipIntfs&attributeName=newAtt1' \
  -H 'Postman-Token: 6b8d393f-3b60-4c9d-98b0-a2046694d0a3' \
  -H 'cache-control: no-cache' \
  -H 'token: b56ed962-8ccd-4b2d-a7c1-7d97fff51321'v
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
        interfaceType = ""
        attributeName = ""
        
Response:
    
    # Set both params as emprty.
    "Delete interface attribute failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'attributeName' cannot be null."
        }"
        
    # Set only "attributeName" as emprty.
    "Delete interface attribute failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'attributeName' cannot be null."
        }"
        
    # Set only "interfaceType" as emprty.
    "Delete interface attribute failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'interfaceType' cannot be null."
        }"
     
###################################################################################################################    

"""Error 1: wrong inputs"""

Input:
    
        interfaceType = "ipIntfs"
        attributeName = "newAtt11hahahahah" # There is no attribute name as "newAtt11hahahahah"
        
Response:
    
    "Delete interface attribute failed! - 
        {
            "statusCode":791006,
            "statusDescription":"attribute newAtt11hahahahah does not exist."
        }"
        
Input:""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
    
        interfaceType = "ipIntfshahahah" # There is no interface type called "ipIntfshahahah"
        attributeName = "newAtt1111"
 
        
Response:
    
    "Delete interface attribute failed! - 
        {
            "statusCode":791006,
            "statusDescription":"attribute newAtt1111 does not exist."
        }"
        
###################################################################################################################    

"""Error : duplicate deletion"""

Input:
    
        interfaceType = "ipIntfs"
        attributeName = "newAtt" # "newAtt" has been deleted.

Response:
    
    "Delete interface attribute failed! - 
        {
            "statusCode":791006,
            "statusDescription":"attribute newAtt does not exist."
        }"
        
```
