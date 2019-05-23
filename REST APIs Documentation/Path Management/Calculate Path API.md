
# Path API Design

## ***POST*** /V1/CMDB/Path/Calculation
Call this API to calculate the path from endpoint A (source) to endpoint B (destination). It returns the result of the calculated path in the form of a path ID (a string), and you can use the path ID in the GetPath API as the input parameter to get each hop information of the path.

## Detail Information

> **Title** : Calculate Path API<br>

> **Version** : 01/30/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Path/Calculation

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|sourceIP* | string  | Input the IP address of the source device.  |
|sourcePort | integer  | Specify the source protocol port If TCP/UDP is selected such as 23 for telnet. This parameter can be null.  |
|sourceGwIP* | string  | the ip address of the gateway device.  |
|sourceGwDev* | string  | the hostname of the gateway device.  |
|sourceGwIntf* | string  | the name of the gateway interface.  |
|destIP* | string  | Input the IP address of the destination device.  |
|destPort* | integer  | Specify the destination protocol port If TCP/UDP is selected, such as 23 for telnet. This parameter can be null.  |
|pathAnalysisSet* | integer  | Specify the path type to calculate:<br>▪ 1 - L3 Path<br>▪ 2 - L2 Path<br>▪ 3 - L3 Active Path |
|protocol* | integer  | Specify the application protocol. see list_of_ip_protocol_numbers, such as 4 for IPv4.  |
|isLive | integer  | ▪ 0 - Use data From current Baseline<br>▪ 1 - Use data via live access |

## Parameters(****required***)

>No parameters required.

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
|taskID| string | The task ID of the calculated path. You can call the hop information of the path with the taskID in the GetPath API. |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
    'taskID': 'b0bae173-af8c-418b-8dc4-1ffdb0e897fa',
    'statusCode': 790200,
    'statusDescription': 'Success.'
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
token = "c4edcb21-8d27-42a3-be0c-7e3b53b608c7"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Path/Calculation"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

sourceIP = "10.10.3.253"
sourcePort = 0
sourceGwIP = "10.10.3.253"
sourceGwDev = "GW2Lab"
sourceGwIntf =  "GigabitEthernet0/0.10"
destIP = "172.24.32.225"
destPort = 0
pathAnalysisSet = 1
protocol = 4
isLive = False

body = {
            "sourceIP" : sourceIP,                # IP address of the source device.
            "sourcePort" : sourcePort,
            "sourceGwDev" : sourceGwDev,          # Hostname of the gateway device.
            "sourceGwIP" : sourceGwIP,            # Ip address of the gateway device.
            "sourceGwIntf" : sourceGwIntf,        # Name of the gateway interface.
            "destIP" : destIP,                    # IP address of the destination device.
            "destPort" : destPort,
            "pathAnalysisSet" : pathAnalysisSet,  # 1:L3 Path; 2:L2 Path; 3:L3 Active Path
            "protocol" : protocol,                # Specify the application protocol, check online help, such as 4 for IPv4.
            "isLive" : isLive                     # False: Current Baseline; True: Live access
    } 

try:
    response = requests.post(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Create module attribute failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'taskID': 'acf3309e-99b1-4369-8e4b-79e065fd807e', 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Path/Calculation \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: f91143d0-c8c5-4a00-8ba6-908087452898' \
  -H 'cache-control: no-cache' \
  -H 'token: c4edcb21-8d27-42a3-be0c-7e3b53b608c7' \
  -d '{
            "sourceIP" : "10.10.3.253",                
            "sourcePort" : 0,
            "sourceGwIP" : "10.10.3.253",          
            "sourceGwDev" : "GW2Lab",            
            "sourceGwIntf" : "GigabitEthernet0/0.10",        
            "destIP" : "172.24.32.225",                    
            "destPort" : 0,
            "pathAnalysisSet" : 1,  
            "protocol" : 4,                
            "isLive" : 1                     
    } '
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
    
    sourceIP = "" # Can not be null
    sourcePort = "" 
    sourceGwIP = "" # Can not be null
    sourceGwDev = "" # Can not be null
    sourceGwIntf =  "" # Can not be null
    destIP = "" # Can not be null
    destPort = ""
    pathAnalysisSet = None # Can not be null
    protocol = None # Can not be null
    isLive = ""
    
Response:
    
    "Create module attribute failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'sourceIP' cannot be null."}

    Create module attribute failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'destIP' cannot be null."}

    Create module attribute failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'protocol' cannot be null."}

    Create module attribute failed! - 
    {"statusCode":791001,"statusDescription":"Invalid parameter: the parameter 'pathAnalysisSet' is invalid."}

    Create module attribute failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'sourceGwIP' cannot be null."}

    Create module attribute failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'sourceGwDev' cannot be null."}

    Create module attribute failed! - 
    {"statusCode":791000,"statusDescription":"Null parameter: the parameter 'sourceGwIntf' cannot be null."}"
    
###################################################################################################################    

"""Error 2: wrong format of Ips"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:

        sourceIP = "10103253" # "10.10.3.253"
        sourcePort = ""
        sourceGwIP = "10.10.3.253"
        sourceGwDev = "GW2Lab"
        sourceGwIntf =  "GigabitEthernet0/0.10"
        destIP = "172.24.32.225"
        destPort = ""
        pathAnalysisSet = 1
        protocol = 4
        isLive = ""

Response:
            "{
                'taskID': '061c0aa3-396d-4900-b350-23a6e57f4891', 
                'statusCode': 790200, 
                'statusDescription': 'Success.'
            }"
            
 #--------------------------------------------------------------------------------------------------------------------           

Input:""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

        sourceIP = "10.10.3.253" 
        sourcePort = ""
        sourceGwIP = "10103253" # "10.10.3.253"
        sourceGwDev = "GW2Lab"
        sourceGwIntf =  "GigabitEthernet0/0.10"
        destIP = "172.24.32.225" 
        destPort = ""
        pathAnalysisSet = 1
        protocol = 4
        isLive = ""

Response:
            "{
                'taskID': '061c0aa3-396d-4900-b350-23a6e57f4891', 
                'statusCode': 790200, 
                'statusDescription': 'Success.'
            }"
            
 #--------------------------------------------------------------------------------------------------------------------  

Input:""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

        sourceIP = "10.10.3.253" 
        sourcePort = ""
        sourceGwIP = "10.10.3.253" 
        sourceGwDev = "GW2Lab"
        sourceGwIntf =  "GigabitEthernet0/0.10"
        destIP = "1722432225" # "172.24.32.225"
        destPort = ""
        pathAnalysisSet = 1
        protocol = 4
        isLive = ""

Response:
            "{
                'taskID': '061c0aa3-396d-4900-b350-23a6e57f4891', 
                'statusCode': 790200, 
                'statusDescription': 'Success.'
            }"
            
```
