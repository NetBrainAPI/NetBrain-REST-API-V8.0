
# Discovery API Design

## ***POST*** /V1/CMDB/Discovery/Tasks/{task}/Run
Call this API to run a  scheduled discovery task right away. Error would return if the task is already running.
> **Note**: {task} means {taskId} or {taskName}

## Detail Information

> **Title** : Run Discovery Task  Now API<br>

> **Version** : 01/29/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Discovery/Tasks/{taskId or taskName}/Run

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
|tasks.id*| string | The ID of a discovery task.  |
|tasks.name*| string | The name of a discovery task. |

>>**Note:** two parameters can only provide one to call this API and must provide one parameter.

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
token = "c4fcb468-bc36-4cca-acc8-2c863de34ed2"
nb_url = "http://192.168.28.79"

taskID = "34124e63-31d6-dfad-f5fa-05ae0ebb4b49"
##OR##
#taskName = "testGDL_DT1"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

full_url= nb_url + "/ServicesAPI/API//V1/CMDB/Discovery/Tasks/"+str(taskID)+"/Run"
##OR##
#full_url= nb_url + "/ServicesAPI/API//V1/CMDB/Discovery/Tasks/"+str(taskName)+"/Run"

try:
    # Do the HTTP request
    response = requests.post(full_url, headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        result = response.json()
        print (result)
    else:
        print ("Run Discovery Task failed - " + str(response.text))

except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Discovery/Tasks/34124e63-31d6-dfad-f5fa-05ae0ebb4b49/Run \
  -H 'Postman-Token: e41ca84d-ba5b-4a69-b759-1e29ed549b06' \
  -H 'cache-control: no-cache' \
  -H 'token: c4fcb468-bc36-4cca-acc8-2c863de34ed2'
```

# Error Examples :


```python
###################################################################################################################    

"""Error 1: empty taskID"""

Input:
    
        taskID = ""

Response:
    
    "Get IPs from discovery task failed - 
        {
            "statusCode":793404,
            "statusDescription":"No resource"
        }"

###################################################################################################################    

"""Error 2: wrong taskID"""

Input:
    
        taskID = "c4fcb468-bc36-4cca-acc8" # it should be "c4fcb468-bc36-4cca-acc8-2c863de34ed2"

Response:
    
    "Get IPs from discovery task failed - 
        {
            "statusCode":794004,
            "statusDescription":"Task 'c4fcb468-bc36-4cca-acc8' does not exist."
        }"

###################################################################################################################    

"""Error 3: duplicate running task"""

Input:
    
        taskID = "34124e63-31d6-dfad-f5fa-05ae0ebb4b49" # this task is still running when user call running task now API

Response:
    
    "Run Discovery Task failed - 
        {
            "statusCode":794001,
            "statusDescription":"The task is running."
        }"

###################################################################################################################    

"""Error 4: Unknown Error, i don't understand why."""

Input:
    
        taskID = "a8e8904c-574b-07c3-5caf-07fc7584e4f8" # this task is still running when user call running task now API

Response:
    
    "Run Discovery Task failed - 
        {
            "statusCode": 794003,
            "statusDescription": "Incorrect discovery type. Please select \"Scan the following IPs\" as the discovery type."
        }"

#### Note: you have to change the frequency setting of your bidcovery task.

###################################################################################################################    

"""Note: the running time (duration) of a DT is decided by how many IPs need to be discovery.
         Which means the DT which created by users may stop very fast. That would be cause when the user check the status of 
         a DT, it always shows "Successed", nothing else. Or maybe the user will consider the trigger API can be multiple called 
         even the corresponding DT is running.(Actually it is already stop.) That is not the problem of our API."""
```
