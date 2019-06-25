
# Discovery API Design

## ***GET*** /V1/CMDB/Discovery/Tasks/{taskId or taskName}/Status
Call this API to get all discovered devices (successfully or failed) for a  discovery task.

> **Caution :** If too many devices would be discovered (for example, up to 50,000 network devices), this API may return a large amount of data and have the performance issue. 

## Detail Information

> **Title** : Get Discovery Task Status API<br>

> **Version** : 01/28/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Discovery/Tasks/{taskId or taskName}/Status

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
|taskStatus| integer | The status of the task. The status has the following values: <br> ▪ -1: Unknown <br> ▪ 0: Never run<br> ▪ 2: Running<br> ▪ 10: Succeeded<br> ▪ 11: Succeeded with warnings<br> ▪ 20: Failed<br> ▪ 30: Manually stopped<br> ▪ 31: Automatically stopped due to timeout set by users or another system setting. |

> ***Example***


```python
{
    "taskStatus": 0,
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
token = "fd8b3f95-adc6-406d-9c18-bdb155de2ced"
nb_url = "http://192.168.28.79"

#taskID = "34124e63-31d6-dfad-f5fa-05ae0ebb4b49"
##OR##
taskName = "testGDL_DT1"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

#full_url= nb_url + "/ServicesAPI/API/V1/CMDB/Discovery/Tasks/"+str(taskID)+"/Status"
##OR##
full_url= nb_url + "/ServicesAPI/API/V1/CMDB/Discovery/Tasks/"+str(taskName)+"/Status"
    
try:
    # Do the HTTP request
    response = requests.get(full_url, headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        result = response.json()
        print (result)
    else:
        print("Get Running Status failed - " + str(response.text))

except Exception as e:
    print (str(e)) 
```

    {'taskStatus': 0, 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X GET \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Discovery/Tasks/e86e73da-1829-fd8e-4f83-98f524533779/Status \
  -H 'Postman-Token: 749cc9f7-b13f-499b-8da0-d91b03c83322' \
  -H 'cache-control: no-cache' \
  -H 'token: fd8b3f95-adc6-406d-9c18-bdb155de2ced'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
        taskID = ""
        taskName = ""

Response:
    
    "Get Running Status failed - 
        {
            "statusCode":793404,
            "statusDescription":"No resource"
        }"

###################################################################################################################    

"""Error 2: taskId or taskName dose not exist"""

Input:
     
        taskID = "34124e63-31d6-dfad-f5fa" # it should be "34124e63-31d6-dfad-f5fa-05ae0ebb4b49"
        ##OR##
        taskName = "blahblahblah"# it should be "testGDL_DT1"


Response:
    
    "Get Running Status failed - 
        {
            "taskStatus":0,
            "statusCode":794004,
            "statusDescription":"Task 'blahblahblah' does not exist."
        }"
        
    ##OR##
    "Get Running Status failed - 
        {
            "taskStatus":0,
            "statusCode":794004,
            "statusDescription":"Task '34124e63-31d6-dfad-f5fa' does not exist."
        }"
        
```
