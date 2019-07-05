
# Site API Design

## ***PUT*** /V1/CMDB/Sites/Devices
Calling this API to remove all existing devices from the site which specified by site name or site Id and add new devices provided in the devices  parameter. 
 
Note: this API call needs to be invoked in a site transaction.

## Detail Information

> **Title** : Replace Site Devices API<br>

> **Version** : 02/04/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Sites/Devices

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 


## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|sitesId^ | string  |  The unique id of specified site.  |
|sitePath^ | string  | Full path name of a site. For example, 'My Network/Site1/Boston'.  |
|devices* | list of string  | List of device hostnames.  |

> ***Example***


```python
{
    "sitePath": "XXXXXXX",
    "Devices": [
        "Bos-Core1"
    ]
}
 
##OR##

{
    "siteId": "XXXXXXX",
    "Devices": [
        "Bos-Core1"
    ]
}
```

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
token = "1cde0ef4-4956-43ce-9e24-96d9a471e4fd"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Devices"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

sitePath = "testSite1/testSiteLeaf"
##OR##
#sitId = "37965f93-377c-46b9-852c-193870bb5933"
devices = ["R1", "R10", "R11", "R12"]

body = {
           "sitePath" : sitePath,
           "Devices": devices
    }         

try:
    response = requests.put(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Devices added Fail! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X PUT \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Sites/Devices \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 18a526a2-7c9a-4e18-8c7b-d8d8906b3efe' \
  -H 'cache-control: no-cache' \
  -H 'token: 1cde0ef4-4956-43ce-9e24-96d9a471e4fd' \
  -d '{
        "sitePath":"testSite1/testSiteLeaf",
        "devices": ["R1", "R10", "R11", "R12"]

    }'
```

# Error Examples:
Same with "Add Site Devices API" 
