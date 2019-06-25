
# Site API Design

## ***DELETE*** /V1/CMDB/Sites{?sitePath}|{?siteId}
Calling this API to delete one specified site by site ID or site path. If the site is a container site(parent site), all child sites will be deleted at same time.

Note: this API call needs to be invoked in a site transaction.

## Detail Information

> **Title** : Delete A Sites API<br>

> **Version** : 02/04/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Sites

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
|siteId^ | string  | Guid of this site to be deleted. Optional. However, parameter must be siteId or sitePath, use siteId if both set. |
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

# Full Examples:


```python
# import python modules 
import requests
import time
import urllib3
import pprint
import json
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Set the request inputs
token = "1c52cd65-3247-44ad-91e6-cd73fc6c64a6"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

sitePath = "My Network/Site1"
siteId = ""

data = {
           "sitePath" : sitePath
            #"siteId": siteId
        } 

try:
    response = requests.delete(full_url, params = data, headers = headers, verify = False)
    #response = requests.delete(full_url, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Site Created Failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X DELETE \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Sites?sitePath=My%20Network/Site1' \
  -H 'Postman-Token: 11f365ff-de38-420e-8de0-2ff47d3367ba' \
  -H 'cache-control: no-cache' \
  -H 'token: 1c52cd65-3247-44ad-91e6-cd73fc6c64a6'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: empty input"""

Input:
        
        sitePath = ""
        
Response:

    "Site Created Failed! - 
        {
            "statusCode":791000,
            "statusDescription":"Null parameter: the parameter 'siteId and sitePath' cannot be null."
        }"
        
###################################################################################################################    

"""Error 1: wrong input"""

Input:
        
        siteId = "1123456789" # not site has a Id like this.
        
Response:

    "Site Created Failed! - 
        {
            "statusCode":793000,
            "statusDescription":"Unexpected Error."
        }"
        
#-----------------------------------------------------------------------------------------------------------------        

Input:
        
        sitePath = "111/222/333/44" # no such site path exist.
        ##OR##
        sitePath = "My Network/Site1" # the site with site path "My Network/Site1" has already been deleted.
        
Response:

    "Site Created Failed! - 
        {
            "statusCode":791006,
            "statusDescription":"site with path 111/222/333/44 does not exist."
        }"        
```
