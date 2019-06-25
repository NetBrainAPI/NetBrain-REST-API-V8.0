
# Benchmark API Design

## ***GET*** /V1/CMDB/Benchmark/Tasks/{taskname}/Runs
This API call returns historical executions of a  scheduled benchmark task .

## Detail Information

> **Title** : Get Benchmark Task Running History API<br>

> **Version** : 01/25/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/{taskname}/Runs

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

> No Request Body.

## Path Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| taskname* | string  | The name of benchmark task which created by user calling the Add Benchmark API|

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
|runs| list of object | One scheduled task can be executed many times, periodically or manually by user. Each exection creates a run record.|
|runId| string | ID of this execution. which can be used as input of get results of one specific run. |
|startTime| string | start time |
|endTime| string | end time, if not end yet, this field would not present |
|status| integer | Status of this exection of scheduled task.<br>Possible values:<br>2, Running<br>10, Succeeded<br>11, Succeeded with warnning<br>20, Failed<br>30, Manually stopped<br>31, Automatically stopped due to timeout set by user or other system setting |
|isFinished| bool | true or false |
|isStopByUser| bool | true or false |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***


```python
{
    'runs': [
        {
            'runId': 'f99c21ba-6f92-4958-9964-37597ec65b69', 
            'startTime': '2019-01-25T14:04:30Z', 
            'endTime': '2019-01-25T14:04:42Z', 
            'status': 10, 
            'isFinished': True
        }, 
        {
            'runId': '7a43e9fc-a7e2-4cb2-8b33-6412c2ab551d', 
            'startTime': '2019-01-25T14:07:03Z', 
            'endTime': '2019-01-25T14:07:16Z', 
            'status': 10, 
            'isFinished': True
        }, 
        {
            'runId': 'c34a2b3b-c0b6-465f-8008-f7a16c757202', 
            'startTime': '2019-01-25T14:07:38Z', 
            'endTime': '2019-01-25T14:07:51Z', 
            'status': 10, 
            'isFinished': True
        }, 
        {
            'runId': '8dca23b8-89d7-427a-8007-72c87529cb16', 
            'startTime': '2019-01-25T14:08:06Z', 
            'endTime': '2019-01-25T14:08:18Z', 
            'status': 10, 
            'isFinished': True
        }, 
        {
            'runId': 'edce37a6-093d-438a-836b-18114cb92785', 
            'startTime': '2019-01-25T14:12:55Z', 
            'endTime': '2019-01-25T14:13:08Z', 
            'status': 10, 
            'isFinished': True
        },
        {
            'runId': '7e3843ac-8f07-45dc-8335-b1bbb163f8ef', 
            'startTime': '2019-01-25T14:13:17Z', 
            'endTime': '2019-01-25T14:13:29Z', 
            'status': 10, 
            'isFinished': True
        }, 
        {
            'runId': '476cd7ea-bb26-43e0-a3a6-8fd1a6e70a58', 
            'startTime': '2019-01-25T17:58:20Z', 
            'endTime': '2019-01-25T17:58:32Z', 
            'status': 10, 
            'isFinished': True
        }
    ], 
    
    'statusCode': 790200, 
    'statusDescription': 'Success.'
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
token = "e074d192-3f21-4ae8-b5f1-405d240b65ca"
nb_url = "http://192.168.28.79"
taskName = "Scheduled System DiscoveryGDL11"

full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/" + taskName + "/Runs"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

try:
    response = requests.get(full_url, headers=headers, verify=False)
    if response.status_code == 200:
        res = response.json()
        print (res)
    else:
        print ("Benchmark Task running Failed! - " + str(response.text))

except Exception as e:
        print (str(e)) 
```

    {'runs': [{'runId': 'f99c21ba-6f92-4958-9964-37597ec65b69', 'startTime': '2019-01-25T14:04:30Z', 'endTime': '2019-01-25T14:04:42Z', 'status': 10, 'isFinished': True}, {'runId': '7a43e9fc-a7e2-4cb2-8b33-6412c2ab551d', 'startTime': '2019-01-25T14:07:03Z', 'endTime': '2019-01-25T14:07:16Z', 'status': 10, 'isFinished': True}, {'runId': 'c34a2b3b-c0b6-465f-8008-f7a16c757202', 'startTime': '2019-01-25T14:07:38Z', 'endTime': '2019-01-25T14:07:51Z', 'status': 10, 'isFinished': True}, {'runId': '8dca23b8-89d7-427a-8007-72c87529cb16', 'startTime': '2019-01-25T14:08:06Z', 'endTime': '2019-01-25T14:08:18Z', 'status': 10, 'isFinished': True}, {'runId': 'edce37a6-093d-438a-836b-18114cb92785', 'startTime': '2019-01-25T14:12:55Z', 'endTime': '2019-01-25T14:13:08Z', 'status': 10, 'isFinished': True}, {'runId': '7e3843ac-8f07-45dc-8335-b1bbb163f8ef', 'startTime': '2019-01-25T14:13:17Z', 'endTime': '2019-01-25T14:13:29Z', 'status': 10, 'isFinished': True}, {'runId': '476cd7ea-bb26-43e0-a3a6-8fd1a6e70a58', 'startTime': '2019-01-25T17:58:20Z', 'endTime': '2019-01-25T17:58:32Z', 'status': 10, 'isFinished': True}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X GET \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/Scheduled%20System%20DiscoveryGDL11/Runs \
  -H 'Postman-Token: 6cd44976-3232-4d79-a0a3-adf8290e2ea0' \
  -H 'cache-control: no-cache' \
  -H 'token: e074d192-3f21-4ae8-b5f1-405d240b65ca'
```

# Error Examples


```python
###################################################################################################################    

"""Error 1: empty task name"""

Input:
    
    taskName = ""

Response:
    
    "Benchmark Task running Failed! - 
        {
            "statusCode":793405,
            "statusDescription":"Method is not supported"
        }"

###################################################################################################################    

"""Error 1: Benchmark Task dosn't exist"""

Input:
    
    taskName = ""

Response:
    
    "Benchmark Task running Failed! - 
        {
            "statusCode":794004,
            "statusDescription":"Task 'Scheduled System DiscoveryGDL' does not exist."
        }"
```
