
# Change Management Approval Process API Design


## ***POST*** /V1/CM/Approval/State
This API call is used to set CM Runbook approval state example - approve or reject.

## Detail Information
> **Title** : Approve a change management request<br>

> **Version** : 06/26/2019<br>

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CM/Approval/State <br>

> **Authentication** :

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 


 ## Request body(****required***)
|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|runbookId* | string  | ID of the Change Management Runbook  |
|ticketId* | string  | Other vendor's ticket number  |
|vendor* | string  | Name of the vendor  |
|state* | integer  | 0(planning)/1(pending)/2(approved) /3(rejected) /5(archived)  |
|ticketName | string  | Name of the ticket, for example: in ServiceNow CHG0030015 (means "number" in ServiceNow) |
|ticketUrl | string  | The full URL of the tichet, for example: in ServiceNow: https://dev65813.service-now.com/nav_to.do?uri=%2Fincident.do%3Fsys_id%3D1ecf1235dbe2330093890d53ca9619a2%26sysparm_record_target%3Dincident%26sysparm_record_row%3D1%26sysparm_record_rows%3D67%26sysparm_record_list%3DORDERBYDESCnumber  |

***Example***

```python
body = {
    'vendor': 'serviceNow',
    'ticketId': "00008",
    'runbookId': '7652cb62-c5e6-d0a3-3f22-29972d03ad4c',
    'ticketUrl': 'https://dev65813.service-now.com/nav_to.do?uri=%2Fincident.do%3Fsys_id%3D1ecf1235dbe2330093890d53ca9619a2%26sysparm_record_target%3Dincident%26sysparm_record_row%3D1%26sysparm_record_rows%3D67%26sysparm_record_list%3DORDERBYDESCnumber',
    'state': 2
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
|<img width=100/>|<img width=100/>|<img width=500/>|
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***


```python
{
    'statusCode': 790200,
    'statusDescription': 'Success.'
}
```

## Full Example:


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "5e372bc9-3b2e-48fc-b86b-e9c651968f85"
nb_url = "http://192.168.28.173"
full_url = nb_url + "/ServicesAPI/API/V1/CM/Approval/State"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

body = {
    'vendor': 'serviceNow',
    'ticketId': "00008",
    'runbookId': '7652cb62-c5e6-d0a3-3f22-29972d03ad4c',
    'ticketUrl': 'https://dev65813.service-now.com/nav_to.do?uri=%2Fincident.do%3Fsys_id%3D1ecf1235dbe2330093890d53ca9619a2%26sysparm_record_target%3Dincident%26sysparm_record_row%3D1%26sysparm_record_rows%3D67%26sysparm_record_list%3DORDERBYDESCnumber',
    'state': 2
}

try:
    response = requests.post(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Ticket change management failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```
API Response:

    {"runbookUrl":"http://192.168.28.173/map.html?t=823e096b-093a-10f1-1471-21a9a5ff509c&d=af4581fd-a705-4ddf-a878-fd4c6f304b96&id=378d70a2-f3e1-76c1-42fa-2bb088c1bda2&rba=9948608d-c755-d1e7-b5d1-325b917952b0","statusCode":790200,"statusDescription":"Success."}
    

# cURL Code from Postman


```python
curl -X POST \
  http://192.168.28.173/ServicesAPI/API/V1/CM/Approval/State \
  -H 'Accept: */*' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -H 'Host: 192.168.28.173' \
  -H 'Postman-Token: 6417cc52-66ee-44b7-9718-dad31ef898b7,1c24d1d5-441a-4a1f-870e-3127a913d67c' \
  -H 'User-Agent: PostmanRuntime/7.13.0' \
  -H 'accept-encoding: gzip, deflate' \
  -H 'cache-control: no-cache' \
  -H 'content-length: 389' \
  -H 'token: 5e372bc9-3b2e-48fc-b86b-e9c651968f85' \
  -d '{
    "vendor": "serviceNow",
    "ticketId": "00008",
    "runbookId": "7652cb62-c5e6-d0a3-3f22-29972d03ad4c",
    "ticketUrl": "https://dev65813.service-now.com/nav_to.do?uri=%2Fincident.do%3Fsys_id%3D1ecf1235dbe2330093890d53ca9619a2%26sysparm_record_target%3Dincident%26sysparm_record_row%3D1%26sysparm_record_rows%3D67%26sysparm_record_list%3DORDERBYDESCnumber",
    "state": 2
}'
```
