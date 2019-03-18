
# Benchmark API Design

## ***GET*** /V1/CMDB/Benchmark/Tasks/{taskname}/Status
Use this API to get the running status of a specific benchmark task.

If this task has never been triggered, NEVER_RUN (0) would be returned.

## Detail Information

> **Title** : Get Benchmark Task Status API<br>

> **Version** : 01/25/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/{taskname}/Status

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
|taskStatus| integer | Status of the scheduled task.<br>Possible values:<br>-1 Unknown<br>0, Never run<br>2, Running<br>10, Succeeded<br>11, Succeeded with warnning<br>20, Failed<br>30, Manually stopped<br>31, Automatically stopped due to timeout set by user or other system setting|

> ***Example***


```python
{
    'taskStatus': 10, 
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

#Status of the scheduled task.
#---Possible values:
#------1 Unknown
#------0, Never run
#------2, Running
#------10, Succeeded
#------11, Succeeded with warnning
#------20, Failed
#------30, Manually stopped
#------31, Automatically stopped due to timeout set by user or other system setting


# Set the request inputs
token = "35c83b3a-2c2c-4332-9d73-e21f2174904f"
nb_url = "http://192.168.28.79"
taskName = "Scheduled System DiscoveryGDL11"

full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/" + taskName + "/Status"
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

    {'taskStatus': 10, 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X GET \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Benchmark/Tasks/Scheduled%20System%20DiscoveryGDL11/Status \
  -H 'Postman-Token: 3d1d18c0-c3f3-48f8-9da7-744e1da9eb16' \
  -H 'cache-control: no-cache' \
  -H 'token: 35c83b3a-2c2c-4332-9d73-e21f2174904f'
```

# Error Examples:


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
