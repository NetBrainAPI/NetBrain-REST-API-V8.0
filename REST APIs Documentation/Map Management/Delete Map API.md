
# Map API Design

## ***DELETE*** /V1/CMDB/Maps/{?url}
Call this API to delete a map from system.

## Detail Information

> **Title** : Delete Map API<br>

> **Version** : 01/30/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Maps

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
|url* | string  | The url of the map that you will export.  |

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
token = "b56ed962-8ccd-4b2d-a7c1-7d97fff51321"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Maps"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

map_url = "http://192.168.28.79/map.html?t=fb24f3f0-81a7-1929-4b8f-99106c23fa5b&d=0201adc4-ae96-46f0-ae3d-01cdba9e41d6&id=7f713c38-10fa-295a-3721-35d1b6d9fa5b"
data = {
        "url": map_url
    }

try:
    response = requests.delete(full_url, params=data, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        #map_data = result["fileData"]
        print (result)
    elif response.status_code != 200:
        print ("Export map failed! - " + str(response.text))
    
except Exception as e:
    print (str(e))
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL from Postman:


```python
curl -X DELETE \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Maps?url=http://192.168.28.79/map.html?t=fb24f3f0-81a7-1929-4b8f-99106c23fa5b&d=0201adc4-ae96-46f0-ae3d-01cdba9e41d6&id=7f713c38-10fa-295a-3721-35d1b6d9fa5b' \
  -H 'Postman-Token: 9bf74a3f-96df-4b5c-a399-238f3a2b8c3c' \
  -H 'cache-control: no-cache' \
  -H 'token: b56ed962-8ccd-4b2d-a7c1-7d97fff51321'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
        map_url = ""
        
Response:
    
    "Export map failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'url' cannot be null."
        }"
""  
###################################################################################################################    

"""Error 2: wrong inputs"""

Input:
    
        map_url = "http://192.168.28.79/map.html?t=fb24f3f0-81a7-1929-4b8f-99106c23fa5b"
        # it shoudld be "http://192.168.28.79/map.html?t=fb24f3f0-81a7-1929-4b8f-99106c23fa5b&d=0201adc4-ae96-46f0-ae3d-01cdba9e41d6&id=7f713c38-10fa-295a-3721-35d1b6d9fa5b"

        
Response:
    
    "Export map failed! - 
        {
            "statusCode":793001,
            "statusDescription":"Inner exception. please check system log(default location: log/NgThirdAPI.log)"
        }"
""  
###################################################################################################################    

"""Error 3: double delete one map"""

Input:
    
        map_url = "http://192.168.28.79/map.html?t=fb24f3f0-81a7-1929-4b8f-99106c23fa5b&d=0201adc4-ae96-46f0-ae3d-01cdba9e41d6&id=7f713c38-10fa-295a-3721-35d1b6d9fa5b"
        # the map with this usrl has been deleted already.
Response:
    
    "Export map failed! - 
        {
            "statusCode":792010,
            "statusDescription":"The map file does not exist in the tenant and domain."
        }"
  
```
