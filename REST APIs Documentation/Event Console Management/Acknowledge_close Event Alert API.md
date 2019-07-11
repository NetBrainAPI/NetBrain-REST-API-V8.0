
# Event Console API Design

## Detail Information

> **Title** : Acknowledge/close Event Alert API<br>

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
|eventStatus*| list of integer | A list shows the status of event alert.<br> 1: set as acknowledged,<br>2: set as closed|

***Example:***



```python
{
    "eventIDs": [{
        "eventID": "",
        "eventStatus": [] 
    },
    {
        "eventID": "",
        "eventStatus": [] 
    },
    {
        "eventID": "",
        "eventStatus": [] 
    }]
}
```

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
 
# Fail, all event ID can not found in system
{
    "statusCode": 794011
    "statusDescription": "Operation failed. Reason: No valid event ID."
}
 
# Fail, no valid eventID and eventStatus in the eventIDs
{
    "statusCode": 794011
    "statusDescription": "Operation failed. Reason: No valid event ID and event status."
}
 
# Partial Success
{
    "statusCode": 790210
    "statusDescription": "Warning: acknowledge/close partially successed! Reason: [xx,xx] in \"eventIDs\" list are not found."
}
{
    "statusCode": 790210
    "statusDescription": "Warning: acknowledge/close partially successed! Reason: There are some invalid event status."
}
```

# Full Example:


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

nb_url = "https://ie80.netbraintech.com"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'} 
token = "bae8df27-0366-4897-8d6b-6e52624c5036"
headers["Token"] = token

modifyEventAlert_URL = nb_url + "/ServicesAPI/API/V1/CMDB/EventConsole" 

body = {
    "eventIDs": [{
        "eventID": "6d5cce45-378a-2bea-c60b-267d7bd85cf3",
        "eventStatus": [1,2] 
    }]
}

def modifyEventAlert(modifyEventAlert_URL, body, headers):
    try:
        # Do the HTTP request
        response = requests.put(modifyEventAlert_URL, headers=headers, data = json.dumps(body), verify=False)
        #response = requests.get(getEventConsole_URL, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            js = response.json()
            #res = js["content"]
            return (js)
        else:
            return ("Modify Event Console failed! - " + str(response.text))
    except Exception as e:
        return (str(e))
    
result = modifyEventAlert(modifyEventAlert_URL, body, headers)
result # print out 
```

    D:\Anaconda\lib\site-packages\urllib3\connectionpool.py:847: InsecureRequestWarning: Unverified HTTPS request is being made. Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
      InsecureRequestWarning)
    




    {'statusCode': 790200, 'statusDescription': 'Success.'}



# cURL Code from Postman


```python
curl -X PUT \
  https://ie80.netbraintech.com/ServicesAPI/API/V1/CMDB/EventConsole \
  -H 'Accept: */*' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -H 'Host: ie80.netbraintech.com' \
  -H 'Postman-Token: 3fe72af6-95f0-4f45-bd0b-c07bdc85232c,0856cebf-140c-4fd9-978a-4e87bc300d80' \
  -H 'User-Agent: PostmanRuntime/7.13.0' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -H 'content-length: 335' \
  -H 'token: 6cb4a2bf-178d-4b63-bc21-26f6b4a8bf68' \
  -d '{
    "eventIDs": [{
        "eventID": "fa680954-6f62-8a51-b492-7021b97dd5c2",
        "eventStatus": [1,2] 
    },
    {
        "eventID": "28a61b3b-3341-7954-79bb-fc452bdc39a3",
        "eventStatus": [1,2] 
    },
    {
        "eventID": "504874a1-9d00-7927-e47a-6bda26fd9341",
        "eventStatus": [1,2] 
    }]
}'
```
