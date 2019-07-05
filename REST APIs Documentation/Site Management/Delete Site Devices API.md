
# Site API Design

## ***DELETE*** /V1/CMDB/Sites/Devices{?sitePath}|{?siteId}&{?Devices}
Calling this API to  remove devices from the site specified by site name or site Id. All devices will be marked as unassigned

Note: this API call needs to be invoked in a site transaction.

## Detail Information

> **Title** : Delete Site Devices API<br>

> **Version** : 02/04/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Sites/Devices

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
|sitePath^ | string  | Full path name of a site. For example, 'My Network/Site1/Boston/Dev'. |
|siteId^ | string  |  The unique id of specified site.   |
|devices* | list of string  | List of device hostnames.  |
>>**Note :** ^ required if the other parameter is null.

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
token = "9603ce1d-8271-4f77-a2df-0b748ef32084"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Devices"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

sitePath = "My Network/SiteT"
##OR##
#sitId = "37965f93-377c-46b9-852c-193870bb5933"
devices = ["R1", "R10"]

body = {
           "sitePath" : sitePath,
           "Devices": devices
    }         

try:
    response = requests.delete(full_url, data = json.dumps(body), headers = headers, verify = False)
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
curl -X DELETE \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Sites/Devices \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: f8fc0db6-0a1b-488f-a12e-0fcf3e5e0f4d' \
  -H 'cache-control: no-cache' \
  -H 'token: 9603ce1d-8271-4f77-a2df-0b748ef32084' \
  -d '{
        "sitePath":"My Network/Site2",
        "devices": ["R1", "R10"]

    }'
```

# Error Examples:
Same with "Add Site Devices API".
