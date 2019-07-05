
# Site API Design

## ***GET*** /V1/CMDB/Sites/ChildSites{?sitePath}|{?siteId}
Calling this API to  get all descedant sites of a container site. Return error if it is a leaf site.

## Detail Information

> **Title** : Create Sites API<br>

> **Version** : 02/04/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Sites/ChildSites

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 


## Request body(****required***)

> No request body.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|sitePath^ | string  | Full path name of a site. For example, 'My Network/Site1/Boston/Dev'. |
|siteId^ | string  |  The unique id of specified site. |
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
|sites | list of obeject | A list of all child sites.  |
|sites.siteId| string | Id of site.  |
|sites.sitePath| string | Full path of site. |
|sites.isContainer| bool | If this site is a container site. |
|sites.children| list of string | Id list of immediate child site if this site is a container site. |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
    "sites": [
        {
            "siteId": "47e5d1c1-5ddc-4e5f-b37c-21616193dd36",
            "sitePath": "My Network/C0/L4",
            "isContainer": false,
            "siteType": 2
        },
      {
            "siteId": "020cb2a2-d192-4c29-a9bd-53787d866f85",
            "sitePath": "My Network/C0",
            "isContainer": true,
            "children": [
                "47e5d1c1-5ddc-4e5f-b37c-21616193dd36",
                "16d1cd8e-eb8e-42ca-a19d-54d7a7fbd2a2",
                "c36eb043-a30f-4b58-b05f-957f845c40e3",
                "688bc6b2-3b34-42ff-96a2-c06687d2c03a",
                "6348e733-1c6b-4d76-8926-2d20622cf164",
                "3ed2ccba-9a00-48d7-9af0-a17e9aa8ccfb"
            ],
            "siteType": 1
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
token = "9603ce1d-8271-4f77-a2df-0b748ef32084"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/ChildSites"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

sitePath = "My Network"
siteId = ""

data = {
           "sitePath" : sitePath
            #"siteId": siteId
        } 

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    #response = requests.delete(full_url, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Site Created Failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'sites': [{'siteId': '37965f93-377c-46b9-852c-193870bb5933', 'sitePath': 'My Network/Americas', 'isContainer': True, 'children': ['e109a735-752a-41bf-ae22-ddf9c6a2a09f'], 'siteType': 1}, {'siteId': 'e109a735-752a-41bf-ae22-ddf9c6a2a09f', 'sitePath': 'My Network/Americas/Argentina', 'isContainer': True, 'children': ['bd85e5ba-453c-4a91-9204-9b0a75a923d9'], 'siteType': 1}, {'siteId': 'bd85e5ba-453c-4a91-9204-9b0a75a923d9', 'sitePath': 'My Network/Americas/Argentina/Provincia de Buenos Aires', 'isContainer': True, 'children': ['ae1a0cd5-c9cb-4821-a40e-f3441ad71a23'], 'siteType': 1}, {'siteId': 'ae1a0cd5-c9cb-4821-a40e-f3441ad71a23', 'sitePath': 'My Network/Americas/Argentina/Provincia de Buenos Aires/Benavidez', 'isContainer': True, 'children': ['e762eaa7-507f-4c02-9d40-c616f6d64702'], 'siteType': 1}, {'siteId': 'e762eaa7-507f-4c02-9d40-c616f6d64702', 'sitePath': 'My Network/Americas/Argentina/Provincia de Buenos Aires/Benavidez/AM-ARG-BA-BEN-1621-KM375RAM1618', 'isContainer': False, 'siteType': 2}, {'siteId': '732e8ab6-6b69-417d-ad03-2cc447100166', 'sitePath': 'My Network', 'isContainer': True, 'children': ['37965f93-377c-46b9-852c-193870bb5933'], 'siteType': 0}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Sites/ChildSites?sitePath=My%20Network' \
  -H 'Postman-Token: 9305ea5d-3241-40dd-9943-978c19bca9fe' \
  -H 'cache-control: no-cache' \
  -H 'token: 9603ce1d-8271-4f77-a2df-0b748ef32084'
```

# Error Examples:
Same with "Delete Site API"
