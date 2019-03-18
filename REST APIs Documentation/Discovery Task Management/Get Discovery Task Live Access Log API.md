
# Discovery API Design

## ***GET*** /V1/CMDB/Discovery/Tasks/{task}/LiveAccessLog
Call this API to get live access log of all discovered devices (successfully or failed) for a discovery task.

## Detail Information

> **Title** : Get Discovery Live Access Log API<br>

> **Version** : 01/29/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Discovery/Tasks/{task}/LiveAccessLog

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Au
thentication token | 

## Request body(****required***)

> No request body.

## Path Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|tasks.id*| string | The ID of a discovery task.  |
|tasks.name*| string | The name of a discovery task. |
>>**Note:** two parameters can only provide one to call this API and must provide one parameter.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|ips| List of string | The list of IP for devices. Optional, returns results of all devices if not specified.|

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
|liveLogs| list of string | A collection of device logs. |
|liveLogs.mgmtIP| string | The management IP of a device. |
|liveLogs.livelog| string | The live log of device specified by ip liveLogs.mgmIP. |

> ***Example***


```python
# Without specified ips provided.
{
    "liveLogs": [
        {
            "mgmtIP": "123.20.1.3",
            "liveLog": "12:09:00 Ping [123.20.1.3] via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Send RO = [nb][version:v2c] to [123.20.1.3] via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Retrieving [123.20.1.3]'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Telnet to device 123.20.1.3 via Front Server (NetBrainServer)\n12:09:00 Telnet to device 123.20.1.3 successfully via Front Server (NetBrainServer)\n12:09:00 Return from Device:[Username:]\n12:09:00 Sending Username:nb\n12:09:00 Return from Device:[Password:]\n12:09:00 Sending Password:******\n12:09:02 Return from Device:[SW5#]\n12:09:02 Sending \"enable\" command\n12:09:02 Return from Device:[SW5#]\n12:09:02 Sending \"terminal length 0\" command\n12:09:02 Return from Device:[SW5#]\n12:09:02 Sending \"show run\" command\n12:09:02 Received:SW5#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:09:02 Sending \"exit\" command\n12:09:02 Telnet to device 123.20.1.3 disconnected.\n12:09:03 Update configuration file of SW5 successfully,(0.11s)\r\n12:09:03 Discovery of 123.20.1.3 complete"
        },
        {
            "mgmtIP": "123.1.1.1",
            "liveLog": "12:08:57 Ping [123.10.1.2] via Front Server (NetBrainServer); Succeeded\r\n12:08:57 Send RO = [nb][version:v2c] to [123.10.1.2] via Front Server (NetBrainServer); Succeeded\r\n12:08:57 Retrieving [123.10.1.2]'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:08:57 Telnet to device 123.10.1.2 via Front Server (NetBrainServer)\n12:08:57 Telnet to device 123.10.1.2 successfully via Front Server (NetBrainServer)\n12:08:57 Return from Device:[Username:]\n12:08:57 Sending Username:nb\n12:08:57 Return from Device:[Password:]\n12:08:57 Sending Password:******\n12:08:58 Return from Device:[R1#]\n12:08:58 Sending \"enable\" command\n12:08:58 Return from Device:[R1#]\n12:08:58 Sending \"terminal length 0\" command\n12:08:58 Return from Device:[R1#]\n12:08:58 Sending \"show run\" command\n12:08:58 Received:R1#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:08:58 Sending \"exit\" command\n12:08:59 Telnet to device 123.10.1.2 disconnected.\n12:08:59 Update configuration file of R1 successfully,(0.125s)\r\n12:08:59 Discovery of 123.1.1.1 complete"
        },
        {
            "mgmtIP": "10.1.14.2",
            "liveLog": "12:08:56 Ping [10.1.14.2] via Front Server (NetBrainServer); Succeeded\r\n12:08:56 Send RO = [nb][version:v2c] to [10.1.14.2] via Front Server (NetBrainServer); Succeeded\r\n12:08:56 Retrieving [10.1.14.2]'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:08:56 Telnet to device 10.1.14.2 via Front Server (NetBrainServer)\n12:08:56 Telnet to device 10.1.14.2 successfully via Front Server (NetBrainServer)\n12:08:56 Return from Device:[Username:]\n12:08:56 Sending Username:nb\n12:08:56 Return from Device:[Password:]\n12:08:56 Sending Password:******\n12:08:58 Return from Device:[Client3#]\n12:08:58 Sending \"enable\" command\n12:08:58 Return from Device:[Client3#]\n12:08:58 Sending \"terminal length 0\" command\n12:08:58 Return from Device:[Client3#]\n12:08:58 Sending \"show run\" command\n12:08:58 Received:Client3#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:08:58 Sending \"exit\" command\n12:08:58 Telnet to device 10.1.14.2 disconnected.\n12:08:59 Update configuration file of Client3 successfully,(0.125s)\r\n12:08:59 Discovery of 10.1.14.2 complete"
        },
        {
            "mgmtIP": "123.203.3.3",
            "liveLog": "12:09:00 Ping [123.203.3.3] via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Send RO = [nb][version:v2c] to [123.203.3.3] via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Retrieving [123.203.3.3]'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Telnet to device 123.203.3.3 via Front Server (NetBrainServer)\n12:09:00 Telnet to device 123.203.3.3 successfully via Front Server (NetBrainServer)\n12:09:00 Return from Device:[Username:]\n12:09:00 Sending Username:nb\n12:09:00 Return from Device:[Password:]\n12:09:00 Sending Password:******\n12:09:02 Return from Device:[SW3#]\n12:09:02 Sending \"enable\" command\n12:09:02 Return from Device:[SW3#]\n12:09:02 Sending \"terminal length 0\" command\n12:09:02 Return from Device:[SW3#]\n12:09:02 Sending \"show run\" command\n12:09:02 Received:SW3#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:09:02 Sending \"exit\" command\n12:09:02 Telnet to device 123.203.3.3 disconnected.\n12:09:02 Update configuration file of SW3 successfully,(0.093s)\r\n12:09:03 Discovery of 123.203.3.3 complete"
        },
        {
            "mgmtIP": "10.1.13.2",
            "liveLog": "12:08:55 Ping [10.1.13.2] via Front Server (NetBrainServer); Succeeded\r\n12:08:55 Send RO = [nb][version:v2c] to [10.1.13.2] via Front Server (NetBrainServer); Succeeded\r\n12:08:56 Retrieving [10.1.13.2]'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:08:56 Telnet to device 10.1.13.2 via Front Server (NetBrainServer)\n12:08:56 Telnet to device 10.1.13.2 successfully via Front Server (NetBrainServer)\n12:08:56 Return from Device:[Username:]\n12:08:56 Sending Username:nb\n12:08:56 Return from Device:[Password:]\n12:08:56 Sending Password:******\n12:08:57 Return from Device:[Client2#]\n12:08:57 Sending \"enable\" command\n12:08:57 Return from Device:[Client2#]\n12:08:57 Sending \"terminal length 0\" command\n12:08:57 Return from Device:[Client2#]\n12:08:57 Sending \"show run\" command\n12:08:57 Received:Client2#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:08:57 Sending \"exit\" command\n12:08:58 Telnet to device 10.1.13.2 disconnected.\n12:08:58 Update configuration file of Client2 successfully,(0.063s)\r\n12:08:58 Discovery of 10.1.13.2 complete"
        },
        {
            "mgmtIP": "123.204.4.4",
            "liveLog": "12:09:28 Ping [123.204.4.4] via Front Server (NetBrainServer); Succeeded\r\n12:09:28 Send RO = [nb][version:v2c] to [123.204.4.4] via Front Server (NetBrainServer); Succeeded\r\n12:09:28 Retrieving [123.204.4.4]'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:09:28 Telnet to device 123.204.4.4 via Front Server (NetBrainServer)\n12:09:28 Telnet to device 123.204.4.4 successfully via Front Server (NetBrainServer)\n12:09:28 Return from Device:[Username:]\n12:09:28 Sending Username:nb\n12:09:28 Return from Device:[Password:]\n12:09:28 Sending Password:******\n12:09:30 Return from Device:[SW4#]\n12:09:30 Sending \"enable\" command\n12:09:30 Return from Device:[SW4#]\n12:09:30 Sending \"terminal length 0\" command\n12:09:30 Return from Device:[SW4#]\n12:09:30 Sending \"show run\" command\n12:09:30 Received:SW4#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:09:30 Sending \"exit\" command\n12:09:31 Telnet to device 123.204.4.4 disconnected.\n12:09:31 Update configuration file of SW4 successfully,(0.094s)\r\n12:09:31 Discovery of 123.204.4.4 complete"
        }
    ],
    "statusCode": 790200,
    "statusDescription": "Success."
}

# With specified ips provided. ips = ["123.20.1.3"]
{
    "liveLogs": [
        {
            "mgmtIP": "123.20.1.3",
            "liveLog": "12:09:00 Ping [123.20.1.3] via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Send RO = [nb][version:v2c] to [123.20.1.3] via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Retrieving [123.20.1.3]'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Telnet to device 123.20.1.3 via Front Server (NetBrainServer)\n12:09:00 Telnet to device 123.20.1.3 successfully via Front Server (NetBrainServer)\n12:09:00 Return from Device:[Username:]\n12:09:00 Sending Username:nb\n12:09:00 Return from Device:[Password:]\n12:09:00 Sending Password:******\n12:09:02 Return from Device:[SW5#]\n12:09:02 Sending \"enable\" command\n12:09:02 Return from Device:[SW5#]\n12:09:02 Sending \"terminal length 0\" command\n12:09:02 Return from Device:[SW5#]\n12:09:02 Sending \"show run\" command\n12:09:02 Received:SW5#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:09:02 Sending \"exit\" command\n12:09:02 Telnet to device 123.20.1.3 disconnected.\n12:09:03 Update configuration file of SW5 successfully,(0.11s)\r\n12:09:03 Discovery of 123.20.1.3 complete"
        }
    ],
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
ips = [""]#No specified ips.


headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"]=token

full_url= nb_url + "/ServicesAPI/API/V1/CMDB/Discovery/Tasks/"+str(taskID)+"/LiveAccessLog"
##OR##
#full_url= nb_url + "ServicesAPI/API/V1/CMDB/Discovery/Tasks/"+str(taskName)+"/LiveAccessLog"

try:
    # Do the HTTP request
    response = requests.get(full_url, headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        result = response.json()
        print (result)
    else:
        print ("Get Discovery Live Access Log failed - " + str(response.text))

except Exception as e:
    print (str(e)) 
```

    {'liveLogs': [{'mgmtIP': '123.20.1.3', 'liveLog': '12:09:00 Ping [123.20.1.3] via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Send RO = [nb][version:v2c] to [123.20.1.3] via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Retrieving [123.20.1.3]\'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Telnet to device 123.20.1.3 via Front Server (NetBrainServer)\n12:09:00 Telnet to device 123.20.1.3 successfully via Front Server (NetBrainServer)\n12:09:00 Return from Device:[Username:]\n12:09:00 Sending Username:nb\n12:09:00 Return from Device:[Password:]\n12:09:00 Sending Password:******\n12:09:02 Return from Device:[SW5#]\n12:09:02 Sending "enable" command\n12:09:02 Return from Device:[SW5#]\n12:09:02 Sending "terminal length 0" command\n12:09:02 Return from Device:[SW5#]\n12:09:02 Sending "show run" command\n12:09:02 Received:SW5#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:09:02 Sending "exit" command\n12:09:02 Telnet to device 123.20.1.3 disconnected.\n12:09:03 Update configuration file of SW5 successfully,(0.11s)\r\n12:09:03 Discovery of 123.20.1.3 complete'}, {'mgmtIP': '123.1.1.1', 'liveLog': '12:08:57 Ping [123.10.1.2] via Front Server (NetBrainServer); Succeeded\r\n12:08:57 Send RO = [nb][version:v2c] to [123.10.1.2] via Front Server (NetBrainServer); Succeeded\r\n12:08:57 Retrieving [123.10.1.2]\'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:08:57 Telnet to device 123.10.1.2 via Front Server (NetBrainServer)\n12:08:57 Telnet to device 123.10.1.2 successfully via Front Server (NetBrainServer)\n12:08:57 Return from Device:[Username:]\n12:08:57 Sending Username:nb\n12:08:57 Return from Device:[Password:]\n12:08:57 Sending Password:******\n12:08:58 Return from Device:[R1#]\n12:08:58 Sending "enable" command\n12:08:58 Return from Device:[R1#]\n12:08:58 Sending "terminal length 0" command\n12:08:58 Return from Device:[R1#]\n12:08:58 Sending "show run" command\n12:08:58 Received:R1#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:08:58 Sending "exit" command\n12:08:59 Telnet to device 123.10.1.2 disconnected.\n12:08:59 Update configuration file of R1 successfully,(0.125s)\r\n12:08:59 Discovery of 123.1.1.1 complete'}, {'mgmtIP': '10.1.14.2', 'liveLog': '12:08:56 Ping [10.1.14.2] via Front Server (NetBrainServer); Succeeded\r\n12:08:56 Send RO = [nb][version:v2c] to [10.1.14.2] via Front Server (NetBrainServer); Succeeded\r\n12:08:56 Retrieving [10.1.14.2]\'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:08:56 Telnet to device 10.1.14.2 via Front Server (NetBrainServer)\n12:08:56 Telnet to device 10.1.14.2 successfully via Front Server (NetBrainServer)\n12:08:56 Return from Device:[Username:]\n12:08:56 Sending Username:nb\n12:08:56 Return from Device:[Password:]\n12:08:56 Sending Password:******\n12:08:58 Return from Device:[Client3#]\n12:08:58 Sending "enable" command\n12:08:58 Return from Device:[Client3#]\n12:08:58 Sending "terminal length 0" command\n12:08:58 Return from Device:[Client3#]\n12:08:58 Sending "show run" command\n12:08:58 Received:Client3#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:08:58 Sending "exit" command\n12:08:58 Telnet to device 10.1.14.2 disconnected.\n12:08:59 Update configuration file of Client3 successfully,(0.125s)\r\n12:08:59 Discovery of 10.1.14.2 complete'}, {'mgmtIP': '123.203.3.3', 'liveLog': '12:09:00 Ping [123.203.3.3] via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Send RO = [nb][version:v2c] to [123.203.3.3] via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Retrieving [123.203.3.3]\'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Telnet to device 123.203.3.3 via Front Server (NetBrainServer)\n12:09:00 Telnet to device 123.203.3.3 successfully via Front Server (NetBrainServer)\n12:09:00 Return from Device:[Username:]\n12:09:00 Sending Username:nb\n12:09:00 Return from Device:[Password:]\n12:09:00 Sending Password:******\n12:09:02 Return from Device:[SW3#]\n12:09:02 Sending "enable" command\n12:09:02 Return from Device:[SW3#]\n12:09:02 Sending "terminal length 0" command\n12:09:02 Return from Device:[SW3#]\n12:09:02 Sending "show run" command\n12:09:02 Received:SW3#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:09:02 Sending "exit" command\n12:09:02 Telnet to device 123.203.3.3 disconnected.\n12:09:02 Update configuration file of SW3 successfully,(0.093s)\r\n12:09:03 Discovery of 123.203.3.3 complete'}, {'mgmtIP': '10.1.13.2', 'liveLog': '12:08:55 Ping [10.1.13.2] via Front Server (NetBrainServer); Succeeded\r\n12:08:55 Send RO = [nb][version:v2c] to [10.1.13.2] via Front Server (NetBrainServer); Succeeded\r\n12:08:56 Retrieving [10.1.13.2]\'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:08:56 Telnet to device 10.1.13.2 via Front Server (NetBrainServer)\n12:08:56 Telnet to device 10.1.13.2 successfully via Front Server (NetBrainServer)\n12:08:56 Return from Device:[Username:]\n12:08:56 Sending Username:nb\n12:08:56 Return from Device:[Password:]\n12:08:56 Sending Password:******\n12:08:57 Return from Device:[Client2#]\n12:08:57 Sending "enable" command\n12:08:57 Return from Device:[Client2#]\n12:08:57 Sending "terminal length 0" command\n12:08:57 Return from Device:[Client2#]\n12:08:57 Sending "show run" command\n12:08:57 Received:Client2#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:08:57 Sending "exit" command\n12:08:58 Telnet to device 10.1.13.2 disconnected.\n12:08:58 Update configuration file of Client2 successfully,(0.063s)\r\n12:08:58 Discovery of 10.1.13.2 complete'}, {'mgmtIP': '123.204.4.4', 'liveLog': '12:09:28 Ping [123.204.4.4] via Front Server (NetBrainServer); Succeeded\r\n12:09:28 Send RO = [nb][version:v2c] to [123.204.4.4] via Front Server (NetBrainServer); Succeeded\r\n12:09:28 Retrieving [123.204.4.4]\'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:09:28 Telnet to device 123.204.4.4 via Front Server (NetBrainServer)\n12:09:28 Telnet to device 123.204.4.4 successfully via Front Server (NetBrainServer)\n12:09:28 Return from Device:[Username:]\n12:09:28 Sending Username:nb\n12:09:28 Return from Device:[Password:]\n12:09:28 Sending Password:******\n12:09:30 Return from Device:[SW4#]\n12:09:30 Sending "enable" command\n12:09:30 Return from Device:[SW4#]\n12:09:30 Sending "terminal length 0" command\n12:09:30 Return from Device:[SW4#]\n12:09:30 Sending "show run" command\n12:09:30 Received:SW4#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:09:30 Sending "exit" command\n12:09:31 Telnet to device 123.204.4.4 disconnected.\n12:09:31 Update configuration file of SW4 successfully,(0.094s)\r\n12:09:31 Discovery of 123.204.4.4 complete'}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    


```python
ips = ["123.20.1.3"] #Provide a specified ip
data = {"ips" : ips}

try:
    # Do the HTTP request
    response = requests.get(full_url, headers=headers, params = data, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        result = response.json()
        print (result)
    else:
        print ("Get Discovery Live Access Log failed - " + str(response.text))

except Exception as e:
    print (str(e)) 

```

    {'liveLogs': [{'mgmtIP': '123.20.1.3', 'liveLog': '12:09:00 Ping [123.20.1.3] via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Send RO = [nb][version:v2c] to [123.20.1.3] via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Retrieving [123.20.1.3]\'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n12:09:00 Telnet to device 123.20.1.3 via Front Server (NetBrainServer)\n12:09:00 Telnet to device 123.20.1.3 successfully via Front Server (NetBrainServer)\n12:09:00 Return from Device:[Username:]\n12:09:00 Sending Username:nb\n12:09:00 Return from Device:[Password:]\n12:09:00 Sending Password:******\n12:09:02 Return from Device:[SW5#]\n12:09:02 Sending "enable" command\n12:09:02 Return from Device:[SW5#]\n12:09:02 Sending "terminal length 0" command\n12:09:02 Return from Device:[SW5#]\n12:09:02 Sending "show run" command\n12:09:02 Received:SW5#show run\r\nBuilding configuration...\r\n\r\r\n\r\n12:09:02 Sending "exit" command\n12:09:02 Telnet to device 123.20.1.3 disconnected.\n12:09:03 Update configuration file of SW5 successfully,(0.11s)\r\n12:09:03 Discovery of 123.20.1.3 complete'}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Discovery/Tasks/34124e63-31d6-dfad-f5fa-05ae0ebb4b49/LiveAccessLog?ips=123.20.1.3' \
  -H 'Postman-Token: 6dbc5de9-528f-462d-9ed9-533b8d32268e' \
  -H 'cache-control: no-cache' \
  -H 'token: c4fcb468-bc36-4cca-acc8-2c863de34ed2'
```

# Error Examples:


```python
###################################################################################################################    

#Without ips
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

#Without ips
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

"""Error 3: empty ips"""

Input:
    
        ips = [""]
        data = {"ips" : ips}
        taskID = "34124e63-31d6-dfad-f5fa-05ae0ebb4b49"

Response:
    
    "{
        'liveLogs': [], 
        'statusCode': 790200, 
        'statusDescription': 'Success.'
    }"

###################################################################################################################    

"""Error 3: wrong ips"""

Input:
    
        ips = ["123.20."] # It should be "123.20.1.3"
        data = {"ips" : ips}
        taskID = "34124e63-31d6-dfad-f5fa-05ae0ebb4b49"

Response:
    
    "{
        'liveLogs': [], 
        'statusCode': 790200, 
        'statusDescription': 'Success.'
    }"

```
