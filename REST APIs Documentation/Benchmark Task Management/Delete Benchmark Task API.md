
# Benchmark API Design

## ***DELETE*** /V1/CMDB/Benchmark/Tasks/{task_name}
This API call is used to delete a benchmark task definition by task name. It doesn't impact running task.

## Detail Information

> **Title** : Delete Benchmark Task API<br>

> **Version** : 01/24/2019.

> **API Server URL** : http(s)://"IP address of NetBrain Web API Server"/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/{task_name}

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No request body

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

# Full Example :


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

token = "dbd4e523-5964-4c2d-ba8f-da018cfb6299"
task_name = "Scheduled System DiscoveryGDL"
nb_url = "http://192.168.28.79"

full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/" + task_name

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

try:
    response = requests.delete(full_url, headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Benchmark Task running Failed! - " + str(response.text))
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X DELETE \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/Scheduled%20System%20DiscoveryGDL \
  -H 'Postman-Token: 2e8959b4-202f-40a3-bd4b-e13c6d02a728' \
  -H 'Token: 35c83b3a-2c2c-4332-9d73-e21f2174904f' \
  -H 'cache-control: no-cache'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty task_name"""

Input:
    task_name = ""

Response:
    "Benchmark Task running Failed! - 
        {
            "statusCode":793405,
            "statusDescription":"Method is not supported"
        }"
        
###################################################################################################################    

"""Error 2: the benchmark task with provided task_name does not exist"""

Input:
    task_name = "Scheduled System DiscoveryGDL"

Response:
    "Benchmark Task running Failed! - 
        {
            "statusCode":794004,
            "statusDescription":"Task 'Scheduled System DiscoveryGDL' does not exist."
        }"
```
