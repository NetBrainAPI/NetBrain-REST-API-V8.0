
# Stop A Path API


## ***POST*** /V1/CMDB/Path/Calculation/Stop{?taskId}
This function is used to force stop a path calculation process.

## Detail Information

> **Title** : Stop A Path API<br>

> **Version** : 03/11/2019.

> **API Server URL** : http(s)://< IP address of NetBrain Web API Server >/ServicesAPI/API/V1/CMDB/Path/Calculation/Stop

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

> No request body.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
| taskId* | string  | The task ID retrieved from Calculate a Path. |


## Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| token | string  | Authentication token, get from login API. |


## Response

|**Name**|**Type**|**Description**|
|------|------|------|
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |
|result| bool | The execution (force stop) result. |

> ***Example***



```python
{ 
    "result": true, 
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
token = "cdba9af6-1f4d-45d0-8933-7ee38c3223b1"
nb_url = "http://192.168.28.79"
taskId = "24804fe2-0d4d-4057-a7cd-3966e3d5b7c4"

data = {"taskId": taskId}
full_url= nb_url + "/ServicesAPI/API/V1/CMDB/Path/Calculation/Stop"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

try:
    response = requests.post(full_url, headers = headers, params = data, verify = False)
        #response = requests.delete(full_url, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Stop Path Failed! - " + str(response.text))
except Exception as e:
    print (str(e))
```

    Stop Path Failed! - {"statusCode":793001,"statusDescription":"Inner exception. please check system log(default location: log/NgThirdAPI.log)"}
    

# cURL Code from Postman


```python

```
