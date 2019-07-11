
# Event Console API Design

## Detail Information

> **Title** : Get Event Console API<br>

> **Version** : 06/26/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/EventConsole

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|eventType*| string | The collection region of events.<br>1: My Events<br>2: Shared Events<br>1: Global Events|
|eventLevel*|string | The collection level of events.<br>0: information<br>1: Warning<br>2:Error|
|startTime| date |customized events collection start time. We only support UTC time structure, customer need to follow the data example.|
|endTime| date |customized events collection end time.We only support UTC time structure, customer need to follow the data example.|

***Example:***



```python
{
    "eventType": "", # 1: My Events, 2: Shared Events, 3：Global Events e.g. "1",  or "1,2"
    "eventLevel": "", # 0: Information, 1: Warning, 2: Error
    "startTime": DateTime, # optional - need formal DateTime Format in UTC, e.g. "2019-05-02T04:00:00.000Z"
    "endTime": DateTime # optional
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
| device | string |device name or ip|
| event | string |description of event |
| firstTime |string| The first time of happend of this event|
| lastTime |string| The last happend time of this event|
| count | integer |The count number of same event.|
|acknowleged| bool | Whether this event has been acknowledged by customer |
|status| bool | The status of event, closed: False or open: True.|
|executedBy|string| The executer of current event.|
|fromTask|string| The task which this event delongs to.|
|taskType|integer|<br> 1: Run Qapp<br> 2: Instant Qapp<br> 3: 7*24 Monitor Task<br> 4: Dashboard<br> 5: Schedule Task<br> 6: Qapp Scheduler(Qapp)<br> 7: Qapp Scheduler(Path calculation)<br> 8: Manually Verified<br> 9: Benchmark Verified<br> 10: Runbook Verified<br> 11: API Triggered Diagnosis Verified|
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |



```python
{
    "content": [{
        "device": "", 
        "event": "",
        "firstTime": DateTime, 
        "lastTime": DateTime,
        "count": int,
        "acknowledged": bool,
        "status": bool,
        "executedBy": "", 
        "fromTask": "",
        "taskType": int
       
    }]
    "statusCode": 790200
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
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

nb_url = "https://ie80.netbraintech.com"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'} 
token = "6cb4a2bf-178d-4b63-bc21-26f6b4a8bf68"
headers["Token"] = token

getEventConsole_URL = nb_url + "/ServicesAPI/API/V1/CMDB/EventConsole" 

data = {
    "eventType": "1,2,3", 
    "eventLevel": "0,1,2",
    "startTime": "",
    "endTime": ""
}

def getEventConsole(getEventConsole_URL, data, headers):
    try:
        # Do the HTTP request
        response = requests.get(getEventConsole_URL, headers=headers, params = data, verify=False)
        #response = requests.get(getEventConsole_URL, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            js = response.json()
            res = js["content"]
            return (js)
        else:
            return ("Get Event Console failed! - " + str(response.text))
    except Exception as e:
        return (str(e))
    
result = getEventConsole(getEventConsole_URL, data, headers)
result # print out 
```

API response:

    {'content': [{'eventId': '496bcc0f-6054-9e81-1eee-d8cf113b4703',
       'device': 'NY-core-bak',
       'event': 'Interface is down or SNMP failed.',
       'firstTime': '2019-07-11T17:37:25Z',
       'lastTime': '2019-07-11T17:38:24Z',
       'count': 3,
       'acknowleged': False,
       'status': False,
       'executedBy': 'APITestGL',
       'fromTask': '<Map>:Result 1.Overall Health Monitor [SNMP]',
       'taskType': 1},
      {'eventId': '59ab12ef-62b8-08ef-a70d-1313b643f80b',
       'device': 'BJ*POP',
       'event': 'Interface is down or SNMP failed.',
       'firstTime': '2019-07-11T17:37:25Z',
       'lastTime': '2019-07-11T17:38:24Z',
       'count': 3,
       'acknowleged': False,
       'status': False,
       'executedBy': 'APITestGL',
       'fromTask': '<Map>:Result 1.Overall Health Monitor [SNMP]',
       'taskType': 1},
      {'eventId': '6f8d0fa5-cbd3-a24d-35d1-f4f9599653d3',
       'device': 'NY_POPP',
       'event': 'Interface is down or SNMP failed.',
       'firstTime': '2019-07-11T17:37:25Z',
       'lastTime': '2019-07-11T17:38:24Z',
       'count': 3,
       'acknowleged': False,
       'status': False,
       'executedBy': 'APITestGL',
       'fromTask': '<Map>:Result 1.Overall Health Monitor [SNMP]',
       'taskType': 1},
      {'eventId': '28a61b3b-3341-7954-79bb-fc452bdc39a3',
       'device': 'BJ_core_3550',
       'event': 'Interface is down or SNMP failed.',
       'firstTime': '2019-07-11T17:37:25Z',
       'lastTime': '2019-07-11T17:38:24Z',
       'count': 3,
       'acknowleged': False,
       'status': False,
       'executedBy': 'APITestGL',
       'fromTask': '<Map>:Result 1.Overall Health Monitor [SNMP]',
       'taskType': 1},
      {'eventId': '504874a1-9d00-7927-e47a-6bda26fd9341',
       'device': 'NY_Router',
       'event': 'Interface is down or SNMP failed.',
       'firstTime': '2019-07-11T17:37:25Z',
       'lastTime': '2019-07-11T17:38:24Z',
       'count': 3,
       'acknowleged': False,
       'status': False,
       'executedBy': 'APITestGL',
       'fromTask': '<Map>:Result 1.Overall Health Monitor [SNMP]',
       'taskType': 1},
      {'eventId': '87a502e7-a861-e7ce-727f-4b00d632abd1',
       'device': 'GW2Lab',
       'event': 'Interface is down or SNMP failed.',
       'firstTime': '2019-07-11T17:37:25Z',
       'lastTime': '2019-07-11T17:38:21Z',
       'count': 3,
       'acknowleged': False,
       'status': False,
       'executedBy': 'APITestGL',
       'fromTask': '<Map>:Result 1.Overall Health Monitor [SNMP]',
       'taskType': 1},
      {'eventId': '6d5cce45-378a-2bea-c60b-267d7bd85cf3',
       'device': 'NY-core-bak',
       'event': 'Interface is down or SNMP failed.',
       'firstTime': '2019-07-11T17:37:25Z',
       'lastTime': '2019-07-11T17:38:21Z',
       'count': 3,
       'acknowleged': False,
       'status': False,
       'executedBy': 'APITestGL',
       'fromTask': '<Map>:Result 1.Overall Health Monitor [SNMP]',
       'taskType': 1},
      {'eventId': '069a742f-9462-e229-d56a-3f48563cd0e7',
       'device': 'BJ*POP',
       'event': 'Interface is down or SNMP failed.',
       'firstTime': '2019-07-11T17:37:25Z',
       'lastTime': '2019-07-11T17:38:21Z',
       'count': 3,
       'acknowleged': False,
       'status': False,
       'executedBy': 'APITestGL',
       'fromTask': '<Map>:Result 1.Overall Health Monitor [SNMP]',
       'taskType': 1},
      {'eventId': '2df40804-11a3-6f4b-9f54-a001e8946ae8',
       'device': 'NY_POPP',
       'event': 'Interface is down or SNMP failed.',
       'firstTime': '2019-07-11T17:37:25Z',
       'lastTime': '2019-07-11T17:38:21Z',
       'count': 3,
       'acknowleged': False,
       'status': False,
       'executedBy': 'APITestGL',
       'fromTask': '<Map>:Result 1.Overall Health Monitor [SNMP]',
       'taskType': 1},
      {'eventId': 'e93214b0-576c-fec0-acf6-745944133003',
       'device': 'NY_Router',
       'event': 'Interface is down or SNMP failed.',
       'firstTime': '2019-07-11T17:37:25Z',
       'lastTime': '2019-07-11T17:38:21Z',
       'count': 3,
       'acknowleged': False,
       'status': False,
       'executedBy': 'APITestGL',
       'fromTask': '<Map>:Result 1.Overall Health Monitor [SNMP]',
       'taskType': 1},
      {'eventId': '846738f5-d84f-0ce3-5bca-02f69f270569',
       'device': 'BJ_core_3550',
       'event': 'Interface is down or SNMP failed.',
       'firstTime': '2019-07-11T17:37:25Z',
       'lastTime': '2019-07-11T17:38:21Z',
       'count': 3,
       'acknowleged': False,
       'status': False,
       'executedBy': 'APITestGL',
       'fromTask': '<Map>:Result 1.Overall Health Monitor [SNMP]',
       'taskType': 1},
      {'eventId': 'c8378c67-fcef-dd69-1eb9-327323725fb4',
       'device': 'BJ-L2-Core-A',
       'event': 'Interface is down or SNMP failed.',
       'firstTime': '2019-07-11T17:37:25Z',
       'lastTime': '2019-07-11T17:38:21Z',
       'count': 3,
       'acknowleged': False,
       'status': False,
       'executedBy': 'APITestGL',
       'fromTask': '<Map>:Result 1.Overall Health Monitor [SNMP]',
       'taskType': 1}],
     'statusCode': 790200,
     'statusDescription': 'Success.'}



# cURL Code from Postman


```python
curl -X GET \
  'https://ie80.netbraintech.com/ServicesAPI/API/V1/CMDB/EventConsole?eventType=1,2,3&eventLevel=0,1,2' \
  -H 'Accept: */*' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Host: ie80.netbraintech.com' \
  -H 'Postman-Token: 4fa07cf2-6635-41f7-ab9f-1014b553eb1b,766632b0-da69-4305-b61b-968fceb14034' \
  -H 'User-Agent: PostmanRuntime/7.13.0' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -H 'token: ea58b13f-b26d-49e1-8e07-eaa657f48f72'
```
