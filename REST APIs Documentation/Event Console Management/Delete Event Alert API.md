
# Event Console Design API

## Detail Information

> **Title** : Delete Event Alert API<br>

> **Version** : 06/26/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/EventConsole

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|eventIDs* | list of object  | The list of event alert information  |
|eventID*| string | The unique ID of each event. |

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
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

***Example:***


```python
# Success
{
    "statusCode": 790200
    "statusDescription": 'Success.'
}
 
# Fail
{
    "statusCode": 794011
    "statusDescription": "Operation failed. Reason: No valid event ID."
}
 
# Partial Success
{
    "statusCode": 790210
    "statusDescription": "Warning: delete partially successed! Reason: [xx,xx] in \"eventIDs\" list are not found."
}
 
# parameter null
{
    "statusCode": 791000
    "statusDescription": "Null parameter: the parameter 'eventIDs' cannot be null."
}
 
# others
{
    "statusCode": 793001
    "statusDescription": "inner exception"
}
```

# Full Example:


```python
# import python modules 
import requests
import time
import urllib3
import pprint
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

nb_url = "https://ie80.netbraintech.com"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'} 
token = "6cb4a2bf-178d-4b63-bc21-26f6b4a8bf68"
headers["Token"] = token

deleteEventAlert_URL = nb_url + "/ServicesAPI/API/V1/CMDB/EventConsole" 

body = {
    "eventIDs": [
        {"eventID": "28a61b3b-3341-7954-79bb-fc452bdc39a3"},
        {"eventID": "504874a1-9d00-7927-e47a-6bda26fd9341"},
        {"eventID": "fa680954-6f62-8a51-b492-7021b97dd5c2"}
    ]
}

def deleteEventAlert(deleteEventAlert_URL, body, headers):
    try:
        # Do the HTTP request
        response = requests.delete(deleteEventAlert_URL, headers=headers, data = json.dumps(body), verify=False)
        #response = requests.get(getEventConsole_URL, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            js = response.json()
            #res = js["content"]
            return (js)
        else:
            return ("Delete Event Console failed! - " + str(response.text))
    except Exception as e:
        return (str(e))
    
result = deleteEventAlert(deleteEventAlert_URL, body, headers)
result # print out 
```




    {'statusCode': 790200, 'statusDescription': 'Success.'}



# cURL Code from Postman


```python
curl -X DELETE \
  https://ie80.netbraintech.com/ServicesAPI/API/V1/CMDB/EventConsole \
  -H 'Accept: */*' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -H 'Host: ie80.netbraintech.com' \
  -H 'Postman-Token: 22991515-445d-406f-a12d-21ef009cd2b8,99306598-0a88-4062-9dc6-cbee489004ed' \
  -H 'User-Agent: PostmanRuntime/7.13.0' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -H 'content-length: 215' \
  -H 'token: 6cb4a2bf-178d-4b63-bc21-26f6b4a8bf68' \
  -d '{
    "eventIDs": [
        {"eventID": "28a61b3b-3341-7954-79bb-fc452bdc39a3"},
        {"eventID": "504874a1-9d00-7927-e47a-6bda26fd9341"},
        {"eventID": "fa680954-6f62-8a51-b492-7021b97dd5c2"}
    ]
}'
```
