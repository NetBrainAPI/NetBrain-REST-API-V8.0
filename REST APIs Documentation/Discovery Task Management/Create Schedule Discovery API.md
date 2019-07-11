
# Discovery API Design

## ***POST*** /V1/CMDB/Discovery/Tasks
This API call is used to create a scheduled Discovery task to a domain.

Note that, as the key, task name should be unique system wide.

## Detail Information

> **Title** : Create Scheduled Discovery Task API<br>

> **Version** : 04/22/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Discovery/Tasks	

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

 ## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|taskName* | string  | The name of the task.  |
|newTaskName* | string  | The new name of the task.  |
|description | string  | The description of the task. This field is optional.  |
|startDate* | string  | The date when the task starts to run. The standard time format is required, for example, '2017-07-13', '2017/07/13'. Current date will be used by default.  |
|endDate* | string  | The date when the task end to run. The standard time format is required, for example, '2017-07-13', '2017/07/13'.  |
|schedule* | object  | The schedule to run the task. The following sub parameters are included in this object: <br>▪ frequency* (string) - the frequency to run the task. This field is required and includes ”once”, “hourly”,” daily”, “weekly” and “monthly” options.<br>▪ interval(string) - the interval to run the task (optional). This field is only valid for “hourly”,” daily”, and “weekly” options and the default value is 1, such as every 1 hour, 1 week.<br>▪ startTime* (string) - the time to run the task. This field is required and startTime should be in format: ["HH:mm:ss"], if you put date time format such as "2018/04/04 19:20:20 ", "19:20:20" will be used and the date part "2018/04/04" will be ignored.<br> **Note:** Set the time according to your IIS server time zone since the time zone of your ISS server rather than your physical time zone is adopted by the benchmark task.<br>▪ weekday(integer) - the day of the week to run the task. This field is optional and only valid when the frequency is weekly.  0 stands for Sunday, 6 for Saturday and 1-5 for Monday to Friday respectively.<br>▪ dayOfMonth(integer) - which day of a month to run the task. This field is optional and only valid when the frequency is monthly. The default is 1.<br>▪ Months(integer) - which month to run the task. This field is optional and only valid when the frequency is monthly. The default is all 12 months.|
|isEnable | bool  | Determine whether to enable the task. This field is optional and the default value is true. |

> ### ***Example***


```python
body = {
    "taskName": "APITest", 
    "newTaskName": "",  
    "description": "",  
    "startDate": "2019/06/25", 
    "endDate": "2019/06/25",  
    "schedule": {  
        "frequency": "once",  
        "interval": "",  
        "startTime": ["19:20:20"],  
        "weekday": [], 
        "dayOfMonth": "", 
        "months": []   
    },
    "isEnable": True  
}

```

## Parameters(****required***)

> No parameters required.

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
|taskID| string | The unique ID of the new created discovery task|
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***



```python
{
  "statusCode": 790200,   
  "statusDescription": "Success",   
  "taskId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"   
}
```

# Full Example


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

nb_url = "http://192.168.28.173"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'} 
token = "bf414c1e-a3b8-4ef1-9812-b2ba8529e6e7"
headers["Token"] = token

body = {
    "taskName": "APITest", 
    "newTaskName": "",  
    "description": "",  
    "startDate": "2019/06/25", 
    "endDate": "2019/06/26",  
    "schedule": {  
        "frequency": "once",  
        "interval": "",  
        "startTime": ["19:20:20"],  
        "weekday": [], 
        "dayOfMonth": "", 
        "months": []   
    },
    "isEnable": True  
}

addDiscoveryTask_URL = nb_url + "/ServicesAPI/API/V1/CMDB/Discovery/Tasks"

def addDiscoveryTask(addDiscoveryTask_URL, body, headers):
    try:
        # Do the HTTP request
        response = requests.post(addDiscoveryTask_URL, headers=headers, data = json.dumps(body), verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            js = response.json()
            return (js)
        else:
            return ("Delete discovery task failed! - " + str(response.text))
    except Exception as e:
        return (str(e))
    
result = addDiscoveryTask(addDiscoveryTask_URL, body, headers)
print(result) # print out 
```

    {'taskId': 'c8facc3b-fa43-45b3-a3e0-5efa958802a6', 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X POST \
  https://ie80.netbraintech.com/ServicesAPI/API/V1/CMDB/Discovery/Tasks \
  -H 'Accept: */*' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -H 'Host: ie80.netbraintech.com' \
  -H 'Postman-Token: 25d0adec-2849-4796-8c9e-c52ebe8b990d,f7091d88-145a-496b-8fa7-d29b5ddecc84' \
  -H 'User-Agent: PostmanRuntime/7.13.0' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -H 'content-length: 380' \
  -H 'token: 1b6d0451-c598-497d-91b2-1a28db1ac089' \
  -d '{
    "taskName": "APITest", 
    "newTaskName": "",  
    "description": "",  
    "startDate": "2019/06/25", 
    "endDate": "2019/06/25",  
    "schedule": {  
        "frequency": "once",  
        "interval": "",  
        "startTime": ["19:20:20"],  
        "weekday": [], 
        "dayOfMonth": "", 
        "months": []   
    },
    "isEnable": True  
}
'
```
