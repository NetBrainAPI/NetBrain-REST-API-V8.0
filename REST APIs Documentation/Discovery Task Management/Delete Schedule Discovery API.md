
# Discovery API Design

## ***DELETE*** /V1/CMDB/Discovery/Tasks/{task}
This API call is used to delete an existing scheduled Discovery task.

Note that {task} means task name or task ID.

## Detail Information

> **Title** : Delete Scheduled Discovery Task API<br>

> **Version** : 06/26/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Discovery/Tasks	

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)
> No request input body.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|taskName* | string  | The name of the task.  |
|taskID* | string  | The unique ID of the task.  |

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
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***



```python
{
  "statusCode": 790200,  
  "statusDescription": "Success"   
}
```

# Full Example:


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

nb_url = "https://ie80.netbraintech.com"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'} 
token = "ea58b13f-b26d-49e1-8e07-eaa657f48f72"
headers["Token"] = token

deleteDiscoveryTask_URL = nb_url + "/ServicesAPI/API/V1/CMDB/Discovery/Tasks/" + "API_Test"

data = {
    "taskName" : "API_Test"
}

def deleteDiscoveryTask(deleteDiscoveryTask_URL, data, headers):
    try:
        # Do the HTTP request
        #response = requests.delete(deleteDiscoveryTask_URL, headers=headers, params = json.dumps(data), verify=False)
        response = requests.delete(deleteDiscoveryTask_URL, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            js = response.json()
            return (js)
        else:
            return ("Delete discovery task failed! - " + str(response.text))
    except Exception as e:
        return (str(e))
    
result = deleteDiscoveryTask(deleteDiscoveryTask_URL, data, headers)
print(result) # print out 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

    D:\Anaconda\lib\site-packages\urllib3\connectionpool.py:847: InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
      InsecureRequestWarning)
    

# cURL Code from Postman


```python
curl -X DELETE \
  https://ie80.netbraintech.com/ServicesAPI/API/V1/CMDB/Discovery/Tasks/API_Test \
  -H 'Accept: */*' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Host: ie80.netbraintech.com' \
  -H 'Postman-Token: 94fafbbf-7d10-4106-9f18-4a7d22582da9,a42105b1-b075-4dd1-985f-b76c859754b9' \
  -H 'User-Agent: PostmanRuntime/7.13.0' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -H 'content-length: ' \
  -H 'token: ea58b13f-b26d-49e1-8e07-eaa657f48f72'
```
