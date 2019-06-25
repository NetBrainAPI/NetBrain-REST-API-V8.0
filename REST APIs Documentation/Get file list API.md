
# Get file list API

## ***POST*** /V1/CMDB/Files/
This function returns a list of files contained in a specified folder.

## Detail Information

> **Title** : Get file list API<br>

> **Version** : 03/11/2019.

> **API Server URL** : http(s)://< IP address of NetBrain Web API Server >/ServicesAPI/API/V1/CMDB/Files/

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)
|**Name**|**Type**|**Description**|
|------|------|------|
| folderId | string  | The ID of the folder from which you want to get the files. Root folder (public folder) will be returned if folderId is null. |
| fileTypes* | array  | The file types you want to retrieve. There are three file types:<br> ▪ 0: Folder<br> ▪ 11: Map<br> ▪ 21: Dashboard |

> ***Example:***


```python
{ 
    "folderId": "", 
    "fileTypes": [ 0, 11 ,21] 
}
```


## Path Parameters(****required***)

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
|items| array | A list of folders and files.  |
|id| string | The ID of a folder in the file tree. |
|name| string | The name of a file. |
|originalId| string | The ID of a specific dashboard or file. (Used for Map or Dashboard type only.) |
|type | integer | The file types you want to retrieve. There are three file types:<br> ▪ 0: Folder<br> ▪ 11: Map<br> ▪ 21: Dashboard |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***


```python
{ 
    "items": 
    [ 
        { 
            "id":"ad09aa07-b31d-4f42-a0aa-319697825b09", 
            "name":"Public/Site Maps", 
            "type":0 
        }, 
        { 
            "originalId":"75ff3cdf-dff4-48c6-a736-7a86e4374a29", 
            "id":"2a19165f-a4a5-4488-ac5d-acdf9e287ed6", 
            "name":"Public/New Folder/New Folder/New Map",
            "type":11 
        }, 
        { 
            "originalId":"d2650deb-5276-44cb-be21-43e2b129380a", 
            "id":"a84cdca3-3710-47b1-b037-665e38fd6d08", 
            "name":"Public/New Folder(1)/New Map", 
            "type":11 
        } 
    ], 
    "statusCode":790200, 
    "statusDescription":"Success." 
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
token = "cdba9af6-1f4d-45d0-8933-7ee38c3223b1"
nb_url = "http://192.168.28.79"

full_url= nb_url + "/ServicesAPI/API/V1/CMDB/Files/"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

body = { 
    "folderId": "", 
    "fileTypes": [ 0, 11 ,21] 
}

try:
    response = requests.post(full_url, headers = headers, data = json.dumps(body), verify = False)
        #response = requests.delete(full_url, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get Files List Failed! - " + str(response.text))
except Exception as e:
    print (str(e))
```

    {'items': [{'originalId': 'cf23253b-963e-a6e9-be56-d1bc75ea862c', 'id': 'bbf22db2-6a35-48c8-b3b0-537aac641295', 'name': 'Desktop/Map1', 'type': 11}, {'originalId': '3379471c-1c06-19af-b030-8dc82245a7f1', 'id': '7a4cece9-7565-44e9-986e-d4f285a14cb3', 'name': 'Desktop/Map2', 'type': 11}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Files/ \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: f8137d46-98cd-42aa-9aa1-f660b68d872e' \
  -H 'cache-control: no-cache' \
  -H 'token: cdba9af6-1f4d-45d0-8933-7ee38c3223b1' \
  -d '{ 
        "folderId": "", 
        "fileTypes": [ 0, 11 ,21] 
    }'
```
