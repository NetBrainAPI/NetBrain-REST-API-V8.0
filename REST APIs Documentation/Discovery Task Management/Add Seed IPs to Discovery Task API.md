
# Discovery API Design

## ***POST*** /V1/CMDB/Discovery/Tasks/{task}/Seeds
Call this API to add a list of target ip addresses as seeds to an EXISTING scheduled discovery task along with optional cli information for each ip.
> **Note**: {task} means {taskId} or {taskName}

## Detail Information

> **Title** : Add Seed IP(s) To Discovery API<br>

> **Version** : 01/28/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Discovery/Tasks/{taskId or taskName}/Seeds

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|seeds* | list  | The list of IP entries for devices. |
|seeds.mgmtIP* | string  | The management IP address of a device.  |
|seeds.cliType | integer  | The access method to the device. When it is null, the SNMP method will be used. <br> 0: Telnet <br> 1: SSH|
|seeds.username | string  | Specify the username to access the devices.   |
|seeds.password | string  | Specify the password to access the devices.  |
|seeds.privilegeUsername | string  | Specify the username to enter the privilege mode of the devices. |
|seeds.privilegePassword | string  | Specify the password to enter the privilege mode of the devices.  |
|seeds.designatedCredentials | bool  | Determine whether the API only uses the credentials you have specified. If false, the API will use credentials in network settings.  |
|seeds.snmpCommunityString | string  | The SNMP community of the devices. |
|seeds.frontServerOrGroupAlias | string  | Specify the NetBrain front server or front server group to access live network by alias .   |

> ***Example***



```python
{
  "seeds": [
    {
      "mgmtIP": "string",
      "cliType": 0,
      "userName": "NetBrain",
      "password": "NetBrain",
      "privilegeUsername": "string",
      "privilegePassword": "string",
      "designatedCredentials": true,
      "snmpCommunityString": "string",
      "frontServerOrGroupAlias": "string"
    }
  ]
}
```

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
token = "fd8b3f95-adc6-406d-9c18-bdb155de2ced"
nb_url = "http://192.168.28.79"

mgmIP1 = "10.1.13.2"
mgmIP2 = "123.1.1.1"
mgmIP3 = "10.1.14.2"
mgmIP4 = "123.203.3.3"
mgmIP5 = "123.204.4.4"
mgmIP6 = "123.20.1.3"
taskID = "34124e63-31d6-dfad-f5fa-05ae0ebb4b49"
##OR##
#taskName = "testGDL_DT1"

body = {
    "seeds" : 
        [
            {"mgmtIP": mgmIP1},
            {"mgmtIP": mgmIP2},
            {"mgmtIP": mgmIP3},
            {"mgmtIP": mgmIP4},
            {"mgmtIP": mgmIP5},
            {"mgmtIP": mgmIP6}
        ]
    }
 
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token
full_url= nb_url + "/ServicesAPI/API//V1/CMDB/Discovery/Tasks/"+str(taskID)+"/Seeds"

try:
    # Do the HTTP request
    response = requests.post(full_url, data = json.dumps(body), headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        result = response.json()
        print (result)
    else:
        print("IP Add Failed - " + str(response.text))
    
except Exception as e:
    print (str(e)) 

```

    { "statusCode": 790200, "statusDescription": "Success." }
    

 # cURL Code from Postman


```python
curl -X POST \
  http://193.168.28.79/ServicesAPI/API/V1/CMDB/Discovery/Tasks/34124e63-31d6-dfad-f5fa-05ae0ebb4b49/Seeds \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 17a5445d-a878-4803-a03f-cd22280d42b4' \
  -H 'cache-control: no-cache' \
  -H 'token: fd8b3f95-adc6-406d-9c18-bdb155de2ced' \
  -d 'body = {
	"seeds" : [
		{"mgmtIP": "10.1.13.2"},
        {"mgmtIP": "123.1.1.1"},
        {"mgmtIP": "10.1.14.2"},
        {"mgmtIP": "123.15.15.15"},
        {"mgmtIP": "123.10.10.10"},
        {"mgmtIP": "123.11.11.11"}
        ]
    }'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty taskID"""

Input:
    
        mgmIP1 = "10.1.13.2"
        mgmIP2 = "123.1.1.1"
        mgmIP3 = "10.1.14.2"
        mgmIP4 = "123.203.3.3"
        mgmIP5 = "123.204.4.4"
        mgmIP6 = "123.20.1.3"
        taskID = ""

Response:
    
    "IP Add Failed - 
        {
            "statusCode":793404,
            "statusDescription":"No resource"
        }"

###################################################################################################################    

"""Error 2: empty mgmIPs list"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    
        mgmIP1 = ""
        mgmIP2 = ""
        mgmIP3 = ""
        mgmIP4 = ""
        mgmIP5 = ""
        mgmIP6 = ""
        taskID = "34124e63-31d6-dfad-f5fa-05ae0ebb4b49"

Response:
    
    "{
        'statusCode': 790200, 
        'statusDescription': 'Success.'
    }"

###################################################################################################################   

"""Error 2: wrong ip addresses"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
    
        mgmIP1 = "10.1.13.2"
        mgmIP2 = "123.1.1.1"
        mgmIP3 = "10.1.14.2"
        mgmIP4 = "123.203.3.200"# no such device exist
        mgmIP5 = "123.204.4.200"# no such device exist
        mgmIP6 = "123.20.1.200"# no such device exist
        taskID = "34124e63-31d6-dfad-f5fa-05ae0ebb4b49"

Response:
    
    "{
        'statusCode': 790200, 
        'statusDescription': 'Success.'
    }"

```
