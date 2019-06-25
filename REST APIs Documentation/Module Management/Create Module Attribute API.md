
# Module API Design

## ***POST*** /V1/CMDB/Modules/Attributes
Call this API to create a customized module attribute for certain device types. You can use the SetModuleAttribute API to set a value for the created attribute.

## Detail Information

> **Title** : Create Module Attribute API<br>

> **Version** : 01/30/2019.

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
|attributeName* | string  | The name of the attribute.  |
|deviceTypeNames* | string  | The device types that the created attribute applies to. If set to null, it will apply to all device types. See Device Type Name and ID for available device types. |
|subDataType | string  | Only available for list or table type property.<br>▪ The data type of each value in a list type property.<br>▪ If the data type is table, specify the sub properties of table type property as follows:<br>"name" - the sub property name (displayed as a column header).<br>"displayName" - the display name (alias) of the sub property (can be null).<br>"dataType" - the data type of the sub property.<br>"isKey" (bool) - control whether to use the sub property as the key when comparing the table type property. The default value is false.  |
|dataType* | string  | The data type of the attribute.  |
|attributeDisplayName | string  | The display name of the attribute in Device Details pane of NetBrain IE system. |
|isFullSearch*|bool|Set whether to use the property as an index in full scope search, including extended search and default search.|

> ***Example***


```python
{
      "attributeName": "newAttribute",
      "attributeDisplayName": "New Attribute",
      "deviceTypeNames": "null",
      "dataType": "string",
      "subDataType": "null",
      "isFullSearch": true
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
attributeDisplayName = "Module newAtt"
deviceTypeNames = "null"
dataType = "string"
subDataType = "null"
isFullSearch = True

body = {
      "attributeName": attributeName,
      "attributeDisplayName": attributeDisplayName,
      "deviceTypeNames": deviceTypeNames,
      "dataType": dataType,
      "subDataType": subDataType,
      "isFullSearch": isFullSearch
}

try:
    response = requests.post(full_url, data=json.dumps(body), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Create module attribute failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Modules/Attributes \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 1c0f98f1-7a9b-4877-a19b-96b5a6cb2c84' \
  -H 'cache-control: no-cache' \
  -H 'token: b56ed962-8ccd-4b2d-a7c1-7d97fff51321' \
  -d '{
        "attributeName" : "Module_newAtt1",
        "attributeDisplayName" : "Module newAtt1",
        "deviceTypeNames" : "null",
        "dataType" : "string",
        "subDataType" : "null",
        "isFullSearch" : "True"
    }'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
        attributeName = "" # cannot be null
        attributeDisplayName = ""
        deviceTypeNames = ""
        dataType = "" # cannot be null
        subDataType = ""
        isFullSearch = ""
        
Response:
    
    "Create module attribute failed! - 
    {
        "statusCode":791000,
        "statusDescription":"Null parameter: the parameter 'attributeName' cannot be null."
    }

    Create module attribute failed! - 
    {
        "statusCode":791000,
        "statusDescription":"Null parameter: the parameter 'dataType' cannot be null."
    }"
    
###################################################################################################################    

"""Error 1: duplicate "attributeName" """

Input:
    
        attributeName = "Module_newAtt" # atrribute with "attributeName" already exist in domain.
        attributeDisplayName = ""
        deviceTypeNames = ""
        dataType = "string" # cannot be null
        subDataType = ""
        isFullSearch = ""
        
Response:
    
    "Create module attribute failed! - 
        {
            "statusCode":793001,
            "statusDescription":"Inner exception. Insert: Device Schema already exists, ID = modules.Module_newAtt"
        }"
        
###################################################################################################################    

"""Error 1: duplicate "attributeName" """

Input:
    
        attributeName = "Module_newAtt22" 
        attributeDisplayName = ""
        deviceTypeNames = ""
        dataType = "hahaha" # wrong input of data type
        subDataType = ""
        isFullSearch = ""
        
Response:
    
    "Create module attribute failed! - 
        {
            "statusCode":791002,
            "statusDescription":"Invalid value the dataType value is invalid."
        }"
```
