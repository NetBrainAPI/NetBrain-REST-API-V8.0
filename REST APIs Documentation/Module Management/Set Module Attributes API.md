
# Module API Design

## ***PUT*** /V1/CMDB/Modules/Attributes
Call this API to set a value for the specific property of a device module.

## Detail Information

> **Title** : Set Module Attribute API<br>

> **Version** : 01/31/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Modules/Attributes
    
> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|attributeName* | string  | The name of the attribute. Please note that the slot name cannot be set. |
|attributeValue* | string/int/double  | The value for the attribute.  |
|hostname* | string  | The hostname of the device. |
|moduleName* | string  | The full name of the module.  |

> **Note:** Applicable Module Attribute

|**Property Name**|**Display Name in Device Detail Pane**|**Description**|
|------|------|------|
| type | Module Type  | The port counts of a module. |
| ports | Ports  | The module type of a module. |
| sn | Module Serial Number  | The serial number of a module. |
| hwrev | HW Rev  | The hardware revision of a module. |
| fwrev | FW Rev  | The firmware revision of a module. |
| swrev | SW Rev  | The software revision of a module. |
| descr | Description  | The description of a module. |

> ***Example***


```python
{
      "attributeName": "newAtt11",
      "attributeValue": "Boston",
      "hostname": "Bos-Core1",
      "moduleName": "slot1"
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

# Full Examples:


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "9c717c9a-4302-45b5-a068-2a3e9c4ea1a3"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Modules/Attributes"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

attributeName = "Module_newAtt"
attributeValue = "10"
hostname = "R1"
moduleName = ""

body = {
        "hostname": hostname,
        "attributeName": attributeName,
        "attributeValue": attributeValue, 
        "moduleName":moduleName
    }

try:
    response = requests.put(full_url, data=json.dumps(body), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Set module attribute failed! - " + str(response.text))

except Exception as e:
    print (str(e))     
```

    Set module attribute failed! - {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'moduleName' cannot be null."}
    

# cURL Code From Postman


```python
curl -X PUT \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Modules/Attributes \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: ec5e4492-8129-4157-ab4a-1e3cc2d10371' \
  -H 'cache-control: no-cache' \
  -H 'token: 9c717c9a-4302-45b5-a068-2a3e9c4ea1a3' \
  -d '{
        "hostname": "Module_newAtt",
        "attributeName": "10",
        "attributeValue": "R1", 
        "moduleName":""

    }'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
        attributeName = ""
        attributeValue = ""
        hostname = ""
        moduleName = ""
        
Response:
    
    # Set all Attributes as null
    "Set module attribute failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'hostname' cannot be null."
        }

    # Only set hostname not null
    Set module attribute failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'attributeName' cannot be null."
        }
    
    # Set hostname and attributeName not null
    Set module attribute failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'moduleName' cannot be null."
        }
        
    # Only set attribute value as null
    Set module attribute failed! - """"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
        {
            "statusCode":791006,
            "statusDescription":"attribute modules.New does not exist."
        }"
        'Interestingly, the module i provided is not exist, the response should shows the module not exist.'
    
###################################################################################################################    

"""Error 2: Set hostname wrong"""

Input:
    
        attributeName = ""
        attributeValue = ""
        hostname = "RRRRRRRRRRRRRRRRRRRRRRR1" # No device name as "RRRRRRRRRRRRRRRRRRRRRRR1"
        moduleName = ""
        
Response:
    
    "Set module attribute failed! - 
        {
            "statusCode":791006,
            "statusDescription":"device RRRRRRRRRRRRRRRRRr1 does not exist."
        }"
```
