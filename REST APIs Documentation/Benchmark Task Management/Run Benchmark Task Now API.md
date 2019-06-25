
# Benchmark API Design

## ***POST*** /V1/CMDB/Benchmark/Tasks/{taskname}/Run
This API call is used to run a  benchmark task right away, specified by task name. Error would return if the task is already running.

## Detail Information

> **Title** : Run Benchmark Task Now API<br>

> **Version** : 01/25/2019.

> **API Server URL** : http(s)://"IP address of NetBrain Web API Server"/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/{taskname}/Run

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
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***


```python
{
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
token = "35c83b3a-2c2c-4332-9d73-e21f2174904f"
nb_url = "http://192.168.28.79"
taskName = "Scheduled System DiscoveryGDL11"

full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/" + taskName + "/Run"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

try:
    response = requests.post(full_url, headers=headers, verify=False)
    if response.status_code == 200:
        res = response.json()
        print(res) 
    else:
        print ("Benchmark Task running Failed! - " + str(response.text))

except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/Scheduled%20System%20DiscoveryGDL11/Run \
  -H 'Postman-Token: 5dca2710-942f-4a1d-a133-cc38b6f4a9a1' \
  -H 'cache-control: no-cache' \
  -H 'token: 35c83b3a-2c2c-4332-9d73-e21f2174904f'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: trigger a running benchmark task"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL11" #This task is already running before calling this API
    
Response:
    
    "Benchmark Task running Failed! - 
        {
            "statusCode":794005,
            "statusDescription":"Failed to start task This benchmark task is running currently. 
                                "It cannot be submitted again until it is completed."
        }"
        
###################################################################################################################    

"""Error 2: empty task name"""

Input:
    
    taskName = "" 
    
Response:
    
    "Benchmark Task running Failed! - 
        {
            "statusCode":793405,
            "statusDescription":"Method is not supported"
        }"
        
###################################################################################################################    

"""Error 3: wrong task name"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" #user type the wrong name of a benchmark task.
    
Response:
    
    "Benchmark Task running Failed! - 
        {
            "statusCode":794004,
            "statusDescription":"Task 'Scheduled System DiscoveryGDL' does not exist."
        }"
        
###################################################################################################################    

"""Error 4: trigger a benchmark which end date has passed and the corresponding job not exist anymore."""

Input:
    
    taskName = "Basic System Benchmark" # The end date of this task was in 2016 and the corresponding job has been deleted.
    
Response:
    
    "Benchmark Task running Failed! - 
        {
            "statusCode":794005,
            "statusDescription":"Failed to start task Failed to run benchmark task due to exception \"
            Job 58121100-20fa-4461-8d40-31e4923e00e2 not found in database.\"."
        }"
```
