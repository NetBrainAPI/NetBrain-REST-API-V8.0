
# Topology API Design

## ***GET*** /V1/CMDB/Topology/Tasks/{taskId}/Status
Call this API to query the running status of a topology build task status in a domain.

## Detail Information

> **Title** : Get Topology Build Task Status API<br>

> **Version** : 02/01/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Topology/Tasks/{taskId}/Status

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
|taskId* | string  | task id of topology build task  |

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
|status| string | task status. Options:<br>---1, running<br>---2, finished |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***


```python
{
      "status": "finished",
      "statusCode": 790200,
      "statusDescription": "success"
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

taskId = "710e42d5-cc90-49bc-9f92-7e743251ae01"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Topology/Tasks/"+ str(taskId) + "/Status"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

try:
    response = requests.get(full_url, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get status of topology build task failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    Get status of topology build task failed! - {"statusCode":791006,"statusDescription":" Topo build task with Id: 710e42d5-cc90-49bc-9f92-7e743251ae01 does not exist.  or this task is not started yet!"}
    

 # cURL Code from Postman:


```python
curl -X GET \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Topology/Tasks/710e42d5-cc90-49bc-9f92-7e743251ae01/Status \
  -H 'Postman-Token: 20f215c8-e073-4fe2-a78d-2b72316f340f' \
  -H 'cache-control: no-cache' \
  -H 'token: 3d0f475d-dbae-4c44-9080-7b08ded7d35b'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
        
        taskId = "" # Cannot be null.

Response:
    
        "Get status of topology build task failed! - 
            {
                "statusCode":793404,
                "statusDescription":"No resource"
            }"

###################################################################################################################    

"""Error 1: wrong inputs"""

Input:
        
        taskId = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" # There is no "taskId" would be like that.

Response:
    
        "Get status of topology build task failed! - 
            {
                "statusCode":791006,
                "statusDescription":" Topo build task with Id: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX does not exist.  
                                    " or this task is not started yet!"
            }"
            
```
