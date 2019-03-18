
# User API Design

## ***GET*** /V1/CMDB/Users/UsageReport{?tenantId}&{?domainId}&{?fromDate}&{?toDate}
Call this API to get all user usage summary according to tenant/domain in a time specified time range.

## Detail Information

> **Title** : Get Usage Report of Users API<br>

> **Version** : 02/01/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Users/UsageReport

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

>No request body.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|tenantId | string  | Tenant id, if not specified, check all tenants.  |
|domainId | string  | Domain id, if not specifed, check all domains on tenant specifed by tenantId.  |
|fromDate | string  | Start time, if not specified, from the beginning of the system installed. Format: yyyy-MM-ddThh:mm:ssZ, sample: "2018-03-07T04:59:59Z". Use default value 0001-01-01T00:00:00Z if value is null or of invalid format. |
|toDate | string  | End time, if not specified, use the current system time. Format: yyyy-MM-ddThh:mm:ssZ, sample: "2018-03-07T04:59:59Z". Use default value 0001-01-01T00:00:00Z if value is null or of invalid format.  |

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
|usageReport| list of object | List of report for all users.  |
|usageReport.username| string | User name.  |
|usageReport.ofLogins| long | Number of logins.  |
|usageReport.firstLoginTime| string | The first login time.  |
|usageReport.lastLogoutTime| string | Last logout time.  |
|usageReport.totalOnlineTime| string | Total online time.  |
|usageReport.ofLoginFailureDueToSeatLicense| long | Login failure due to seat license.  |

> ***Example***


```python
{
    "usageReport": [
        {
            "username": "Netbrain1",
            "ofLogins": 51,
            "firstLoginTime": "1/15/2019 6:31:40 PM",
            "lastLogoutTime": "1/25/2019 4:55:57 PM",
            "totalOnlineTime": "99h 57m 37s",
            "ofLoginFailureDueToSeatLicense": 0
        },
        {
            "username": "Netbrain2",
            "ofLogins": 32,
            "firstLoginTime": "1/15/2019 5:16:28 PM",
            "lastLogoutTime": "1/24/2019 2:13:16 AM",
            "totalOnlineTime": "65h 19m 21s",
            "ofLoginFailureDueToSeatLicense": 0
        },
        {
            "username": "Netbrain3",
            "ofLogins": 16,
            "firstLoginTime": "1/16/2019 8:05:48 PM",
            "lastLogoutTime": "1/28/2019 6:46:14 PM",
            "totalOnlineTime": "28h 48m 59s",
            "ofLoginFailureDueToSeatLicense": 0
        },
        {
            "username": "Netbrain4",
            "ofLogins": 10,
            "firstLoginTime": "1/22/2019 7:02:30 PM",
            "lastLogoutTime": "2/1/2019 1:38:42 AM",
            "totalOnlineTime": "67h 05m 2s",
            "ofLoginFailureDueToSeatLicense": 0
        },
        {
            "username": "Netbrain5",
            "ofLogins": 1,
            "firstLoginTime": "1/15/2019 10:17:25 PM",
            "lastLogoutTime": "1/15/2019 10:33:49 PM",
            "totalOnlineTime": "16m 24s",
            "ofLoginFailureDueToSeatLicense": 0
        },
        {
            "username": "Netbrain6",
            "ofLogins": 1,
            "firstLoginTime": "1/16/2019 2:47:14 PM",
            "lastLogoutTime": "1/16/2019 10:58:03 PM",
            "totalOnlineTime": "8h 10m 49s",
            "ofLoginFailureDueToSeatLicense": 0
        },
        {
            "username": "Netbrain7",
            "ofLogins": 1,
            "firstLoginTime": "1/25/2019 2:31:35 PM",
            "lastLogoutTime": "1/25/2019 7:28:19 PM",
            "totalOnlineTime": "4h 56m 44s",
            "ofLoginFailureDueToSeatLicense": 0
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
token = "3d0f475d-dbae-4c44-9080-7b08ded7d35b"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Users/UsageReport"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

tenantId = "fb24f3f0-81a7-1929-4b8f-99106c23fa5b"
domainId = "0201adc4-ae96-46f0-ae3d-01cdba9e41d6"
fromDate = ""
toDate = ""

data = {
        "tenantId" : tenantId,
        "domainId" : domainId,
        "fromDate" : fromDate,
        "toDate" : toDate
    }

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get User Report failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'usageReport': [{'username': 'gongdailiu', 'ofLogins': 51, 'firstLoginTime': '1/15/2019 6:31:40 PM', 'lastLogoutTime': '1/25/2019 4:55:57 PM', 'totalOnlineTime': '99h 57m 37s', 'ofLoginFailureDueToSeatLicense': 0}, {'username': 'haoran.song', 'ofLogins': 32, 'firstLoginTime': '1/15/2019 5:16:28 PM', 'lastLogoutTime': '1/24/2019 2:13:16 AM', 'totalOnlineTime': '65h 19m 21s', 'ofLoginFailureDueToSeatLicense': 0}, {'username': 'admin', 'ofLogins': 16, 'firstLoginTime': '1/16/2019 8:05:48 PM', 'lastLogoutTime': '1/28/2019 6:46:14 PM', 'totalOnlineTime': '28h 48m 59s', 'ofLoginFailureDueToSeatLicense': 0}, {'username': 'gongdaiAdmin', 'ofLogins': 10, 'firstLoginTime': '1/22/2019 7:02:30 PM', 'lastLogoutTime': '2/1/2019 1:38:42 AM', 'totalOnlineTime': '67h 05m 2s', 'ofLoginFailureDueToSeatLicense': 0}, {'username': 'weiwei', 'ofLogins': 1, 'firstLoginTime': '1/15/2019 10:17:25 PM', 'lastLogoutTime': '1/15/2019 10:33:49 PM', 'totalOnlineTime': '16m 24s', 'ofLoginFailureDueToSeatLicense': 0}, {'username': 'georgej', 'ofLogins': 1, 'firstLoginTime': '1/16/2019 2:47:14 PM', 'lastLogoutTime': '1/16/2019 10:58:03 PM', 'totalOnlineTime': '8h 10m 49s', 'ofLoginFailureDueToSeatLicense': 0}, {'username': 'henryshen', 'ofLogins': 1, 'firstLoginTime': '1/25/2019 2:31:35 PM', 'lastLogoutTime': '1/25/2019 7:28:19 PM', 'totalOnlineTime': '4h 56m 44s', 'ofLoginFailureDueToSeatLicense': 0}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Users/UsageReport?tenantId=fb24f3f0-81a7-1929-4b8f-99106c23fa5b&domainId=0201adc4-ae96-46f0-ae3d-01cdba9e41d6&fromDate=&toDate=' \
  -H 'Postman-Token: a766379a-e824-43dc-9687-5e5319b05ebe' \
  -H 'cache-control: no-cache' \
  -H 'token: 3d0f475d-dbae-4c44-9080-7b08ded7d35b'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty inputs"""

Input:
        
        tenantId = ""
        domainId = ""
        fromDate = ""
        toDate = ""

Response:
    
    # Note: get the full response successfully. Because this API allows all inputs as null.
    
###################################################################################################################  
    
"""Error 2: wrong tenantId inputs"""

Input:
        
        tenantId = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" # No tenants have an ID like this.
        domainId = ""
        fromDate = ""
        toDate = ""

Response:
    
    "Get User Report failed! - 
        {
            "statusCode":791006,
            "statusDescription":"tenantId XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX does not exist."
        }"
    
###################################################################################################################  
    
"""Error 3: wrong domainId inputs"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
        
        tenantId = "" 
        domainId = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" # No domains have an ID like this.
        fromDate = ""
        toDate = ""

Response:
    
    # Get full response successfully, no alert or error.

###################################################################################################################  
    
"""Error 4: wrong date format inputs"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
        
        tenantId = "" 
        domainId = "" 
        fromDate = "20190107T045959Z" # "2019-01-07T04:59:59Z"
        toDate = ""

Response:
    
    # Get full response successfully, no alert or error.
    
###################################################################################################################  
    
"""Error 5: "fromDate" after the day user calling this API""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Input:
        
        tenantId = "" 
        domainId = "" 
        fromDate = "2019-03-07T04:59:59Z" # Today is 2019-02-01 when i calling this API.
        toDate = ""

Response:
    
        "{
            'usageReport': [], 
            'statusCode': 790200, 
            'statusDescription': 'Success.'
        }"
```
