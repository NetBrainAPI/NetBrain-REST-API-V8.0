
# Discovery API Design

## ***GET*** /V1/CMDB/Discovery/Tasks/{taskId or taskName}/Seeds
Call this API to get ip addresses of all devices in one discovery task

## Detail Information

> **Title** : Get All Seed IPs From Discovery API<br>

> **Version** : 01/29/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Discovery/Tasks/{taskId or taskName}/Seeds

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
|IPs | List of string | A list of all devices IPs|

> ***Example***


```python
{
    "ips": [
        "10.1.13.2",
        "10.1.14.2",
        "123.1.1.1",
        "123.20.1.3",
        "123.203.3.3",
        "123.204.4.4"
    ],
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

# Set the request inputs
token = "c4fcb468-bc36-4cca-acc8-2c863de34ed2"
nb_url = "http://192.168.28.79"

taskID = "34124e63-31d6-dfad-f5fa-05ae0ebb4b49"
##OR##
#taskName = "testGDL_DT1"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

full_url= nb_url + "/ServicesAPI/API//V1/CMDB/Discovery/Tasks/"+str(taskID)+"/Seeds"
##OR##
#full_url= nb_url + "/ServicesAPI/API//V1/CMDB/Discovery/Tasks/"+str(taskName)+"/Seeds"

try:
    # Do the HTTP request
    response = requests.get(full_url, headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        result = response.json()
        print (result)
    else:
        print ("Get IPs from discovery task failed - " + str(response.text))

except Exception as e:
    print (str(e)) 
```

    {'ips': ['10.1.13.2', '10.1.14.2', '123.1.1.1', '123.20.1.3', '123.203.3.3', '123.204.4.4'], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X GET \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Discovery/Tasks/34124e63-31d6-dfad-f5fa-05ae0ebb4b49/Seeds \
  -H 'Postman-Token: 17b335d7-84c6-4221-bd12-bc8d376dbf91' \
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

```
