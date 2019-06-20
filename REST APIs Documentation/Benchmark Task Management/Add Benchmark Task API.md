
# Benchmark API Design

## ***POST*** /V1/CMDB/Benchmark/Tasks
This API call is used to add a benchmark task to a domain. 

Note that, as the key, task name should be unique system wide. 

The option for data to be retrieved in this task is 'Built-in Live Data' which the user can see from UI.

## Detail Information

> **Title** : Add Benchmark Task API<br>

> **Version** : 01/24/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Benchmark/Tasks	

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
|description | string  | The description of the task. This field is optional.  |
|startDate* | string  | The date when the task starts to run. The standard time format is required, for example, '2017-07-13', '2017/07/13'. This field is optional. Current date will be used by default.  |
|schedule* | object  | The schedule to run the task. The following sub parameters are included in this object: <br>▪ frequency* (string) - the frequency to run the task. This field is required and includes ”once”, “hourly”,” daily”, “weekly” and “monthly” options.<br>▪ interval(string) - the interval to run the task (optional). This field is only valid for “hourly”,” daily”, and “weekly” options and the default value is 1, such as every 1 hour, 1 week.<br>▪ startTime* (string) - the time to run the task. This field is required and startTime should be in format: ["HH:mm:ss"], if you put date time format such as "2018/04/04 19:20:20 ", "19:20:20" will be used and the date part "2018/04/04" will be ignored.<br> **Note:** Set the time according to your IIS server time zone since the time zone of your ISS server rather than your physical time zone is adopted by the benchmark task.<br>▪ weekday(integer) - the day of the week to run the task. This field is optional and only valid when the frequency is weekly.  0 stands for Sunday, 6 for Saturday and 1-5 for Monday to Friday respectively.<br>▪ dayOfMonth(integer) - which day of a month to run the task. This field is optional and only valid when the frequency is monthly. The default is 1.<br>▪ Months(integer) - which month to run the task. This field is optional and only valid when the frequency is monthly. The default is all 12 months.|
|deviceScope* | string  |  The devices included in this task.   |
|deviceScope.scopeType | string  | scope type options:<br>"all" for all devices of current domain, deviceScope.scopes will be ignored if this field is set to "all";<br>"deviceGroup" for specified group, if set deviceScope.scopes would be list of full path to device groups, such as \["Public/devgrp1", "Private/devgrp2", "System/devgrp3"\];<br>"site" for a particular site. if set deviceScope.scopes would be list of full path to sites, for example: \["My Networks/US/MA/Boston", "My Networks/US/ME/Portland"\]  |
|deviceScope.scopes | list of string  | ignored if deviceScope.scopeType is set to "all";<br>full path to device groups, such as \["Public/devgrp1", "Private/devgrp2", "System/devgrp3"\] if deviceScope.scopeType is set to "deviceGroup";<br>full path to sites, such as \["My Networks/US/MA/Boston", "My Networks/US/ME/Portland"\] if deviceScope.scopeType is set to "site";  |
|limitRunMins | string  | The time used to retrieve the data. When it reaches the specified time, the task will stop retrieving more data. This field is optional.  |
|cliCommands | string  | The customized CLI commands to retrieve data (for example, ["show version", "show arp"]. This field is optional.  |
|isBuildIPv4L3Topo | bool  | Determine whether to build IPv4 L3 topology. This field is optional and the default value is false.  |
|isBuildIPv6L3Topo | bool  | Determine whether to build IPv6 L3 topology. This field is optional and the default value is false.  |
|isBuildL2Topo | bool  | Determine whether to build L2 topology. This field is optional and the default value is false.  |
|isBuildIPsecVPNTopology | bool  | Determine whether to build IPsecVPN topology. This field is optional and the default value is false. |
|isRecalculateDynamicDeviceGroups | bool  | Determine whether to recalculate dynamic device groups. This field is optional and the default value is false.  |
|sRecalculateSite | bool  | Determine whether to rebuild sites. This field is optional and the default value is false.|
|isRecalculateMPLSVirtualRouteTables | bool  | Determine whether to recalculate MPLS Virtual Route Table. This field is optional and the default value is false.  |
|isbuildDefaultDeviceDataView | bool  | Determine whether to build default device data view. This field is optional and the default value is false.  |
|isEnable | bool  | Determine whether to enable the task. This field is optional and the default value is true. |

> ### ***Example***


```python
body = {
        "taskName":taskName, #The name of the task.
        "startDate":startDate, #The date when the task starts to run. The standard time format is required, for example, '2017-07-13', '2017/07/13'.
        "schedule":{
            "frequency":frequency, #The frequency to run the task. This field is required and includes ”once”, “hourly”,” daily”, “weekly” and “monthly” options.
            "startTime":[startTime] #The time to run the task. This field is required and startTime should be in format: ["HH:mm:ss"], if you put date time format such as "2018/04/04 19:20:20 ", "19:20:20" will be used and the date part "2018/04/04" will be ignored.
            },
        "deviceScope" : {
            "scopeType" : scopeType
            }
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

# Set the request inputs
token = "dbd4e523-5964-4c2d-ba8f-da018cfb6299"
nb_url = "http://192.168.28.79"
taskName = "Scheduled System DiscoveryGDL11"
startDate = "2019-01-16"
frequency = "once"
startTime = "14:40:20"
scopeType = "all"

# Add a new Benchmark
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Benchmark/Tasks"

headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

body = {
    "taskName":taskName, 
    "startDate":startDate, 
    "schedule":{
        "frequency":frequency, 
        "startTime":[startTime]
        },
    "deviceScope" : {
        "scopeType" : scopeType
        }
}

try:
    response = requests.post(full_url, data=json.dumps(body), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Benchmark Task added Failed! - " + str(response.text))

except Exception as e:
    print (str(e))

```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Benchmark/Tasks \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: a6b7d2b7-f331-4418-bcc0-d1a90f01dbd8' \
  -H 'Token: c00de805-9210-44a9-9a26-f0c1e944ea36' \
  -H 'cache-control: no-cache' \
  -d 'body = {
                "taskName":"Scheduled System DiscoveryGDL1", 
                "startDate":"2019-01-16", 
                "schedule":{
                    "frequency":"once", 
                    "startTime":["14:40:20"]
                    },
                "deviceScope" : {
                    "scopeType" : "all"
                    }
            }'
```

# Error Examples


```python
###################################################################################################################    

"""Error 1: empty taskName"""

Input:
    
    taskName = "" 
    startDate = "2019-01-16"
    frequency = "once"
    startTime = "14:40:20"
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'taskName' cannot be null."
        }"

###################################################################################################################    

"""Error 2: tesk name duplicated"""

Input:
    
    taskName = "Scheduled System Discovery" #Benchmark with task name "Scheduled System Discovery" already exist in system.
    startDate = "2019-01-16"
    frequency = "once"
    startTime = "14:40:20"
    scopeType = "all"
    
Response:        
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791007,
            "statusDescription":"Benchmark task with name Scheduled System Discovery already exists."
        }"

###################################################################################################################    

"""Error 1: empty startData"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = ""
    frequency = "once"
    startTime = "14:40:20"
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'startDate' cannot be null."
        }"
        
###################################################################################################################    

"""Error 1: wrong startData format"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "20190116"
    frequency = "once"
    startTime = "14:40:20"
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'startDate' cannot be null."
        }"
        
###################################################################################################################    

"""Error 1: empty frequency"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "2019-01-16"
    frequency = ""
    startTime = "14:40:20"
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'schedule frequency' cannot be null."
        }"
        
###################################################################################################################    

"""Error 1: wrong frequency input"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "2019-01-16"
    frequency = "1"
    startTime = "14:40:20"
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791001,
            "statusDescription":"Invalid  parameter: the parameter 'schedule.frequency' is invalid.  
            "Options: once, hourly, daily, weekly, monthly "
        }"
        
###################################################################################################################    

"""Error 1: empty startTime"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "2019-01-16"
    frequency = "once"
    startTime = ""
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'schedule startTime' cannot be null."
        }"
        
###################################################################################################################    

"""Error 1: wrong format startTime"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "2019-01-16"
    frequency = "once"
    startTime = "14/40/20"
    scopeType = "all"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'schedule startTime' cannot be null."
        }"
        
###################################################################################################################    

"""Error 1: empty scopeType"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "2019-01-16"
    frequency = "once"
    startTime = "14:40:20"
    scopeType = ""
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":792009,
            "statusDescription":"Invalid data type. deviceScope.scopeType, options: all,deviceGroup,site"
        }"
        
###################################################################################################################    

"""Error 1: wrong scopeType input"""

Input:
    
    taskName = "Scheduled System DiscoveryGDL" 
    startDate = "2019-01-16"
    frequency = "once"
    startTime = "14:40:20"
    scopeType = "whole"
    
Response:
    
    "Benchmark Task added Failed! - 
        {
            "statusCode":792009,
            "statusDescription":"Invalid data type. deviceScope.scopeType, options: all,deviceGroup,site"
        }"
```
