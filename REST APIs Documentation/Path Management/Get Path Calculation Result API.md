
# Path API Design

## ***GET*** /V1/CMDB/Path/Calculation/{taskID}/Result	
Call this API to get the hop information of a path calculated through the CalcPath API. 

If the Calculation Path task is not finished yet or failed, user will get an error with messsage reminding.which means you don't need to wait anymore before trying to query the result.

All directed links in the result consists of a directed path grapth, which contains all possible reachable paths from the original source to the destination specified in path calculation

## Detail Information

> **Title** : Get Path Calulation Result API<br>

> **Version** : 01/30/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Path/Calculation/{taskID}/Result	

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

> No request body.

## Path Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|taskID* | string  | Input the task ID returned by the CalcPath API. |

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
|hopList| list of Object | Hop list. |
|hopList.hopId| string | Hop Guid. |
|hopList.srcDeviceName| string | Source device name. |
|hopList.inboundIneterface| string | Source interface name. |
|hopList.mediaName| string | Media name. |
|hopList.dstDeviceName| string | Destination device name. |
|hopList.outboundInterface| string | Destination interface name. |
|hopList.nextHopIdList| list of string | List of next hop ids. |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
    "hopList": [
        {
            "hopId": "01383651-8bec-41fd-bea2-c3a673ac394c",
            "srcDeviceName": "R5",
            "inboundInterface": "Ethernet0/0",
            "mediaName": "123.10.1.12/30",
            "dstDeviceName": "R3",
            "outboundInterface": "Ethernet0/2",
            "nextHopIdList": [
                "5775d104-f2e7-409d-92f5-8e6b1ed9594a"
            ]
        },
        {
            "hopId": "5775d104-f2e7-409d-92f5-8e6b1ed9594a",
            "srcDeviceName": "R3",
            "inboundInterface": "Ethernet0/1",
            "mediaName": "123.10.1.8/30",
            "dstDeviceName": "R2",
            "outboundInterface": "Ethernet0/1",
            "nextHopIdList": [
                "3f28a2db-fd15-4f3c-b4ab-de7cc2d7cad9"
            ]
        },
        {
            "hopId": "3f28a2db-fd15-4f3c-b4ab-de7cc2d7cad9",
            "srcDeviceName": "R2",
            "inboundInterface": "Ethernet0/2",
            "mediaName": "123.10.1.16/30",
            "dstDeviceName": "R4",
            "outboundInterface": "Ethernet0/0",
            "nextHopIdList": []
        },
        {
            "hopId": "b4b4ddc7-02c5-41bd-b0d3-a7e72d3100e9",
            "srcDeviceName": "R5",
            "inboundInterface": "Ethernet0/1",
            "mediaName": "123.10.1.4/30",
            "dstDeviceName": "R1",
            "outboundInterface": "Ethernet0/1",
            "nextHopIdList": [
                "3e4b6c71-8e6e-4845-b6c3-acdca0ac7377"
            ]
        },
        {
            "hopId": "3e4b6c71-8e6e-4845-b6c3-acdca0ac7377",
            "srcDeviceName": "R1",
            "inboundInterface": "Ethernet0/2",
            "mediaName": "123.10.1.0/30",
            "dstDeviceName": "R4",
            "outboundInterface": "Ethernet0/1",
            "nextHopIdList": []
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
taskID = "498c10a7-0011-4a59-a4cd-0258af3edd19"

token = "c4edcb21-8d27-42a3-be0c-7e3b53b608c7"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Path/Calculation/" + str(taskID) + "/Result"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

try:
    response = requests.get(full_url, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        
        print ("Get path calulation result failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'hopList': [{'hopId': '01383651-8bec-41fd-bea2-c3a673ac394c', 'srcDeviceName': 'R5', 'inboundInterface': 'Ethernet0/0', 'mediaName': '123.10.1.12/30', 'dstDeviceName': 'R3', 'outboundInterface': 'Ethernet0/2', 'nextHopIdList': ['5775d104-f2e7-409d-92f5-8e6b1ed9594a']}, {'hopId': '5775d104-f2e7-409d-92f5-8e6b1ed9594a', 'srcDeviceName': 'R3', 'inboundInterface': 'Ethernet0/1', 'mediaName': '123.10.1.8/30', 'dstDeviceName': 'R2', 'outboundInterface': 'Ethernet0/1', 'nextHopIdList': ['3f28a2db-fd15-4f3c-b4ab-de7cc2d7cad9']}, {'hopId': '3f28a2db-fd15-4f3c-b4ab-de7cc2d7cad9', 'srcDeviceName': 'R2', 'inboundInterface': 'Ethernet0/2', 'mediaName': '123.10.1.16/30', 'dstDeviceName': 'R4', 'outboundInterface': 'Ethernet0/0', 'nextHopIdList': []}, {'hopId': 'b4b4ddc7-02c5-41bd-b0d3-a7e72d3100e9', 'srcDeviceName': 'R5', 'inboundInterface': 'Ethernet0/1', 'mediaName': '123.10.1.4/30', 'dstDeviceName': 'R1', 'outboundInterface': 'Ethernet0/1', 'nextHopIdList': ['3e4b6c71-8e6e-4845-b6c3-acdca0ac7377']}, {'hopId': '3e4b6c71-8e6e-4845-b6c3-acdca0ac7377', 'srcDeviceName': 'R1', 'inboundInterface': 'Ethernet0/2', 'mediaName': '123.10.1.0/30', 'dstDeviceName': 'R4', 'outboundInterface': 'Ethernet0/1', 'nextHopIdList': []}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X GET \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Path/Calculation/498c10a7-0011-4a59-a4cd-0258af3edd19/Result \
  -H 'Postman-Token: b9cd18dd-5cd4-4b15-a2b1-8f5fb8376a4f' \
  -H 'cache-control: no-cache' \
  -H 'token: c4edcb21-8d27-42a3-be0c-7e3b53b608c7'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
        
        taskID = ""
        
Response:
     
        "Get path calulation result failed! - 
            {
                "statusCode":793404,
                "statusDescription":"No resource"
            }"
            
###################################################################################################################    

"""Error 2: wrong inputs"""

Input:
        
        taskID = "ssssssssssssssssssssssssssssss" # "498c10a7-0011-4a59-a4cd-0258af3edd19"
        
Response:
     
        "Get path calulation result failed! - 
            {
                "statusCode":794004,
                "statusDescription":"Task 'sssssssssssssssssssssssssssss' does not exist."
            }"

###################################################################################################################    

"""Error 3: Un-useful taskID"""

Input:
        
        taskID = "3402b0ad-e8ca-472f-b470-7d31bd20bcce" # Although this "taskID" has been created successfully, but it useless.
        
Response:
     
        "Get path calulation result failed! - 
            {
                "hopList":[],
                "statusCode":794008,
                "statusDescription":"Task '3402b0ad-e8ca-472f-b470-7d31bd20bcce' is failed"
            }"
                                            
```
