
# Interface API Design

## ***POST*** /V1/CMDB/Interface/Attributes
Call this API to to create a customized interface attribute for certain device types. You can use the "SetInterfaceAttribute" API to set a value for the created attribute.

## Detail Information

> **Title** : Create Interface Attribute API<br>

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
|InterfaceType* | string  | The interface type that the created attribute belongs to, such as intfs (Layer 2 topology type interfaces), ipIntfs (IPv4 L3 topology type interfaces), and ip6Intfs (IPv6 L3 topology type interfaces). See Interface Type for more details.  |
|attributeName* | string  | The name of the attribute.  |
|attributeDisplayName | string  | The display name of the attribute in NetBrain IE system. If set to null, it will use the attribute name.  |
|deviceTypeNames* | string[]  | The device types that the created attribute applies to. If set to null, it will apply to all device types. See Device Type Name and ID for available device types.  |
|dataType* | string  | The supported data types of the attribute.  |
|subDataType | string  | Only available for list or table type property.<br>▪ The data type of each value in a list type property.<br>▪ If the data type is table, specify the sub properties of table type property as follows:<br>name - the sub property name (displayed as a column header).<br>displayName - the display name (alias) of the sub property (can be null).<br>dataType - the data type of the sub property.<br>isKey (bool) - control whether to use the sub property as the key when comparing the table type property. The default value is false.  |
|isFullSearch* | bool  | Set whether to use the property as an index in full scope search, including extended search and default search.  |

> ***Example***


```python
{
  "InterfaceType": "ipIntfs ",
  "attributeName": "newAttribute",
  "attributeDisplayName": "New Attribute",
  "deviceTypeNames": "null",
  "dataType": "string",
  "subDataType": "null",
  "isFullSearch": true
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

# Full Example :


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

interfaceType = "ipIntfs "
attributeName = "newAttribute22"
attributeDisplayName = "New_Attribute"
deviceTypeNames = "null"
dataType = "string"
isFullSearch = True

full_url= nb_url + "/ServicesAPI/API/V1/CMDB/Interfaces/Attributes"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

body={
        "interfaceType":interfaceType,
        "attributeName": attributeName,
        "attributeDisplayName": attributeDisplayName,
        "deviceTypeNames": deviceTypeNames, 
        "dataType": dataType,
        "isFullSearch": isFullSearch
    }

try:
    response = requests.post(full_url, data=json.dumps(body), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Create interface attribute failed! - " + str(response.text))

except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Interfaces/Attributes \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: dbd3f763-7295-4ae9-ba49-fd0e79fbeaf2' \
  -H 'cache-control: no-cache' \
  -H 'token: ff389578-87ef-45b6-b42c-7bd98c91d1a9' \
  -d '{
       "InterfaceType": "ipIntfs ",
       "attributeName": "newAttribute222",
       "attributeDisplayName": "New_Attribute",
       "deviceTypeNames": "null",
       "dataType": "string",
       "subDataType": "null",
       "isFullSearch": true
     }'
```

# Error Examples : 


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
        interfaceType = "" # cannot be null
        attributeName = "" # cannot be null
        attributeDisplayName = ""
        deviceTypeNames = ""
        dataType = "" # cannot be null
        isFullSearch = True
        
Response:
    
    '''
    Create interface attribute failed! - 
           {
               "statusCode":791000,
               "statusDescription":"Null parameter: the parameter 'attributeName' cannot be null."
            }
       
    Create interface attribute failed! - 
           {
               "statusCode":791000,
               "statusDescription":"Null parameter: the parameter 'dataType' cannot be null."
            }
       
    Create interface attribute failed! - 
           {
               "statusCode":791000,
               "statusDescription":"Null parameter: the parameter 'interfaceType' cannot be null."
            }
            
            
    '''

###################################################################################################################    

"""Error 2: duplicate "attributeName" """""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    
        interfaceType = "ipIntfs"
        attributeName = "newAtt1" # "newAtt1" already exist in current device.
        attributeDisplayName = ""
        deviceTypeNames = ""
        dataType = "string"
        isFullSearch = True
        
Response:
    
    '''
    Create interface attribute failed! - 
        {
            "statusCode":793001,
            "statusDescription":"Inner exception. Insert: Device Schema already exists, ID = ipIntfs.newAtt1"
        }
            
            
    '''

###################################################################################################################    

"""Error 3: wrong input of "interfaceType" """""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    
        interfaceType = "blahblah" # Should be one kind of "intfs", "ipIntfs", "ip6Intfs", "ipsecVpnIntfs", "greVpnIntfs"
        attributeName = "newAtt11"
        attributeDisplayName = ""
        deviceTypeNames = ""
        dataType = "string"
        isFullSearch = True
        
Response:
    
    '''
    {
        'statusCode': 790200, 
        'statusDescription': 'Success.'
    }     
    
    '''
    
###################################################################################################################    

"""Error 4: wrong input of "dataType" """

Input:
    
        interfaceType = "blahblah" # Should be one kind of "intfs", "ipIntfs", "ip6Intfs", "ipsecVpnIntfs", "greVpnIntfs"
        attributeName = "newAtt11"
        attributeDisplayName = ""
        deviceTypeNames = ""
        dataType = "string"
        isFullSearch = True
        
Response:
    
    '''
    Create interface attribute failed! - 
        {
            "statusCode":791002,
            "statusDescription":"Invalid value the dataType value is invalid."
        }    
    
    '''
    
###################################################################################################################    

"""Error 5: set "isFullSearch" as null"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    
        interfaceType = "ipIntfs"
        attributeName = "newAtt111"
        attributeDisplayName = ""
        deviceTypeNames = ""
        dataType = "string"
        isFullSearch = "" # shouldn't be null

Response:
    
    '''
    {
        'statusCode': 790200, 
        'statusDescription': 'Success.'
    }   
    
    '''
    
###################################################################################################################    

"""Error 6: set "isFullSearch" with wrong value"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    
        interfaceType = "ipIntfs"
        attributeName = "newAtt1111"
        attributeDisplayName = ""
        deviceTypeNames = ""
        dataType = "string"
        isFullSearch = "hahahaha" # only can be "True" or "False"


Response:
    
    '''
    {
        'statusCode': 790200, 
        'statusDescription': 'Success.'
    }   
    
    '''
```
