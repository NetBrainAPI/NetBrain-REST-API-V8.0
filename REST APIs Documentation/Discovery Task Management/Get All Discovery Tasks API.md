
# Discovery API Design

## ***GET*** /V1/CMDB/Devices/Discovery/Tasks
Call this API to get all discovery tasks from current domain.
>* **Tip:** If there are no discovery tasks in the system, add a new discovery task from the Domain Management page. In the discovery task, select Once on the Frequency tab and Discover the following IPs on the Discovery Seed tab.
<br><br>

## Detail Information

> **Title** : Get All Discovery Tasks API<br>

> **Version** : 01/28/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Discovery/Tasks

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No request body.

## Parameters(****required***)

> No required parameters.


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
|tasks | array | A list of all discovery tasks. |
|tasks.id| string | The ID of a discovery task.  |
|tasks.name| string | The name of a discovery task. |
|tasks.enable| bool | Whether a discovery task is enabled.  |
|tasks.lastStatus| string | The last run (result) status of a discovery task. |
|tasks.lastRunSpan| integer | The duration of the last run of a discovery task.  |
|tasks.curStatus| string | The current status of a discovery task.  |
|tasks.nextRunTime| string | The start time of the next run of a discovery task.  |
|tasks.frequency| string | The run frequency of a discovery task.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
    "tasks": [
        {
            "id": "e86e73da-1829-fd8e-4f83-98f524533779",
            "name": "Scheduled System Discovery",
            "enable": false,
            "lastStatus": "",
            "lastRunSpan": "00:00:00",
            "curStatus": "Not Started",
            "nextRunTime": "1/2/3000,12:00:00 AM",
            "frequency": "Once"
        }
    ],
    "statusCode": 790200,
    "statusDescription": "Success."
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
token = "fd8b3f95-adc6-406d-9c18-bdb155de2ced"
nb_url = "http://192.168.28.79"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token
full_url= nb_url + "/ServicesAPI/API/V1/CMDB/Discovery/Tasks"
    
try:
    # Do the HTTP request
    response = requests.get(full_url, headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        result = response.json()
        print (result)
    else:
        print ("Get discovery task list failed - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'tasks': [{'id': 'e86e73da-1829-fd8e-4f83-98f524533779', 'name': 'Scheduled System Discovery', 'enable': False, 'lastStatus': '', 'lastRunSpan': '00:00:00', 'curStatus': 'Not Started', 'nextRunTime': '1/2/3000,12:00:00 AM', 'frequency': 'Once'}, {'id': '34124e63-31d6-dfad-f5fa-05ae0ebb4b49', 'name': 'testGDL_DT1', 'enable': True, 'lastStatus': '', 'lastRunSpan': '00:00:00', 'curStatus': 'Not Started', 'nextRunTime': '1/2/3000,12:00:00 AM', 'frequency': 'Once'}, {'id': 'a8e8904c-574b-07c3-5caf-07fc7584e4f8', 'name': 'testGDL_DT2', 'enable': True, 'lastStatus': '', 'lastRunSpan': '00:00:00', 'curStatus': 'Not Started', 'nextRunTime': '1/30/2019,2:00:00 PM', 'frequency': 'Weekly'}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X GET \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Discovery/Tasks \
  -H 'Postman-Token: f8cf0c55-79de-4d99-a06e-c36cb93dc43a' \
  -H 'cache-control: no-cache' \
  -H 'token: fd8b3f95-adc6-406d-9c18-bdb155de2ced'
```
