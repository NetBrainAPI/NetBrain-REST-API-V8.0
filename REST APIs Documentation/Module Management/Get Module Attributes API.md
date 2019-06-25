
# Module API Design

## ***GET*** /V1/CMDB/Modules/Attributes{?hostname}&{?attributeName}&{?moduleName}
Call this API to get the value for a specified attribute of a device interface, get all attributes if the attribute name is not specifed.

## Detail Information

> **Title** : Get Module Attributes API<br>

> **Version** : 01/30/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Modules/Attributes
    
> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No request body.

## Query arameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|hostname* | string  | The hostname of the target device.  |
|moduleName* | string  | Input the full name of the module. |
|attributeName | string  | Optional. The name of the attribute that you want to get its value,, get all attributes if the attribute name is not specifed.<br>Please note that the attribute name here is case sensitive and not the name displayed in the Device Details pane of NetBrain IE system. See Applicable Module Attributes for system built-in module attributes.  |

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
|moduleAttributeValue| string | The returned attribute value.  |
|hostname| string | The hostname of the returned device. |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***


```python
{
      "statusCode": 790200,
      "statusDescription": "success",
      "hostname": "Bos-Core-1",
      "attributeName": "newAttribute",
      "moduleAttributeValue": "newAttribute"
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
token = "9c717c9a-4302-45b5-a068-2a3e9c4ea1a3"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Modules/Attributes"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

attributeName = "Module_newAtt"
moduleName = "newModule"
hostname = "R1"

data = {
        "hostname":hostname,
        "moduleName":moduleName, 
        "attributeName":attributeName
    }

try:
    response = requests.get(full_url, params=data, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print ("Module Attributes :"+str(result))
    else:
        print ("Get module attributes failed! - " + str(response.text))
    
except Exception as e:
    print (str(e))  
```

    Get module attributes failed! - {"statusCode":793001,"statusDescription":"Inner exception. please check system log(default location: log/NgThirdAPI.log)"}
    

# cURL Code from Postman


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Modules/Attributes?attributeName=Module_newAtt&moduleName=newModule&hostname=R1' \
  -H 'Postman-Token: e4e03449-689b-4732-af30-0e4e2ef6360e' \
  -H 'cache-control: no-cache' \
  -H 'token: 9c717c9a-4302-45b5-a068-2a3e9c4ea1a3'
```
