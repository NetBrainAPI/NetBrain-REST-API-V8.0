
# Interface API Design

## ***PUT*** /V1/CMDB/Interface/Attributes
Call this API to set value for the specific property of one interface.

## Detail Information

> **Title** : Set Interface Attribute API<br>

> **Version** : 01/29/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Interfaces/Attributes

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|attributeName* | string  | The name of the attribute that you want to set a value for. Please note that some interface properties such as Interface Name and IP address cannot be set.|
|attributeValue* | string  | The value for the attribute.  |
|hostname* | string  | The hostname of the device. |
|InterfaceName* | string  | The full name of the interface.  |

>**Note:** Applicable Interface Attributes

|**Property Name**|**Display Name in Device Detail Pane**|**Description**|
|------|------|------|
| bandwidth | Bandwidth  | The bandwidth of an interface. |
| descr | Description  | The description of an interface. |
| duplex | Duplex  | The duplex of an interface. |
| inAclName | Inbound ACL  | The inbound ACL of an interface. |
| intfStatus | Live Status  | The state of an interface. |
| macAddr | MAC Address  | The MAC address of an interface. |
| mibIndex | MIB Index  | The MIB index of an interface. |
| mode | Switchport Mode  | The switchport mode of an interface. |
| mplsVpn | MPLS VPN  | The MPLS VPN configured on an interface. |
| mplsVrf | MPLS VRF  | The name of the VRF configured on an interface. |
| multicastMode | Multicasting Mode  | The multicasting mode of an interface. |
| outAclName | Outbound ACL  | The outbound ACL of an interface. |
| routingProtocol | Routing Protocol.  | The routing protocol configured on an interface. |
| speed | Speed  | The speed of an interface. |
| trunkEncapsulation | Trunk Encapsulation  | The trunk encapsulation protocol of an interface as trunk port. |
| trunkNativeVlan | Native VLAN  | The native VLAN of an interface as switchport. |
| vlan | VLAN  | The VLAN number that an interface belongs to. |

> ***Example***


```python
{
      "attributeName": "loc",
      "attributeValue": "Boston",
      "hostname": "Bos-Core1",
      "interfaceName": "Ethernet0/1"
}
```

## Parameters(****required***)

> No parameters required.

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
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Interfaces/Attributes"

attributeName = "bandwidth"
attributeValue = "10000"
interfaceName = "Ethernet0/3"
hostname = "R1"

body = {
        "hostname": hostname,
        "attributeName": attributeName,
        "attributeValue": attributeValue, 
        "interfaceName":interfaceName
    }

try:
    response = requests.put(full_url, data=json.dumps(body), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Set interface attribute failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X PUT \
  http://192.186.28.79/ServicesAPI/API/V1/CMDB/Interfaces/Attributes \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 1ec5a279-5527-46c2-b869-a3f7eb18b958' \
  -H 'cache-control: no-cache' \
  -H 'token: ff389578-87ef-45b6-b42c-7bd98c91d1a9' \
  -d '{
          "attributeName": "bandwidth",
          "attributeValue": "100000",
          "hostname": "R1",
          "interfaceName": "Ethernet0/3"
    }'
```

# Error Examples :


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
        attributeName = ""# cannot be null
        attributeValue = ""# cannot be null
        interfaceName = ""# cannot be null
        hostname = ""# cannot be null
        
Response:
    
    '''
    # When set all inputs as null
    
        Set interface attribute failed! - 
            {
                "statusCode":791000,
                "statusDescription":"Null parameter: the parameter 'hostname' cannot be null."
            }
    
    
    # When set all inputs as null except 'hostname' 
    
        Set interface attribute failed! - 
            {
                "statusCode":791000,
                "statusDescription":"Null parameter: the parameter 'attributeName' cannot be null."
            }

    
    # When set 'interfaceName' and 'attributeValue' as null
    
        Set interface attribute failed! - 
            {
                "statusCode":791000,
                "statusDescription":"Null parameter: the parameter 'interfaceName' cannot be null."
            }

    
    # When set 'attributeValue' as null """"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
        
            {
                'statusCode': 790200, 
                'statusDescription': 'Success.'
            }
            
    '''
    
###################################################################################################################    

"""Error 2: wrong hostname provided """
Input:
    
        attributeName = "bandwidth"
        attributeValue = ""
        interfaceName = "Ethernet0/3"
        hostname = "RRRRRRRRR1" # it should be "R1"

        
Response:
    
    '''
    Set interface attribute failed! - 
        {
            "statusCode":791006,
            "statusDescription":"device RRRRRRRRR1 does not exist."
        }
            
            
    '''
    
###################################################################################################################    

"""Error 3: wrong attributeName provided """
Input:
    
        attributeName = "Bandwidth" # It should be "bandwidth"
        attributeValue = ""
        interfaceName = "Ethernet0/3"
        hostname = "R1"
        
Response:
    
    '''
    Set interface attribute failed! - 
        {
            "statusCode":791006,
            "statusDescription":"attribute intfs.Bandwidth does not exist."
        }  
            
    '''
    
###################################################################################################################    

"""Error 4: wrong interfaceName provided """
Input:
    
        attributeName = "bandwidth"
        attributeValue = ""
        interfaceName = "Ethernet0/33333"
        hostname = "R1"
        
Response:
    
    '''
    Set interface attribute failed! - 
        {
            "statusCode":791006,
            "statusDescription":"interface Ethernet0/33333 does not exist."
        }
            
    '''
```
