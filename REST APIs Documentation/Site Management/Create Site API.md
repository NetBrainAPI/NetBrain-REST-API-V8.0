
# Site API Design

## ***POST*** /V1/CMDB/Sites
Calling this API to create sites with full path names. 

Note that 

a) a new site will be created as a parent site if a site doesn't have its parent site in current system. 

b) this API will replace the ImportSiteTree in 7.0b 

c) this API call needs to be invoked in a site transaction.

## Detail Information

> **Title** : Create Sites API<br>

> **Version** : 02/04/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Sites

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 


## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|sites* | List of object  | The list of sites.  |
|sites.sitePath* | string  | Full path name of a site. For example, 'My Network/Site1/Boston'.  |
|sites.isContainer* | bool  | Specify whether the site being added is a container site.  |

> ***Example***


```python
{
   "sites": [
                {
                    "sitePath":"My Network/Site1/",
                    "isContainer": "True"
                },
                {
                    "sitePath":"My Network/Site1/Boston",
                    "isContainer": "False"
                }
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
token = "929b76e4-8a75-4500-9c1b-ee9b7d1c850f"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

sitePath = "My Network/SiteTest1"
isContainer = True

body = {
   "sites": [
                {
                    "sitePath" : sitePath,
                    "isContainer": isContainer
                }
            ]
        }         

try:
    response = requests.post(full_url, data = json.dumps(body), headers = headers, verify = False)
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
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Sites \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 9c14eea6-55f7-43a0-b7a1-8df253d55c3a' \
  -H 'cache-control: no-cache' \
  -H 'token: 1c52cd65-3247-44ad-91e6-cd73fc6c64a6' \
  -d '{
   "sites": [
                {
                    "sitePath":"My Network/Site1",
                    "isContainer": "True"
                },
                {
                    "sitePath":"My Network/Site1/Boston",
                    "isContainer": "False"
                }
            ]
        }         '
```

# Error Example:


```python
###################################################################################################################    

"""Error 1: empty inputs1"""

Input:
        
        sitePath = ""
        isContainer = None

Response:
    
       "Site Created Failed! - 
            {
                "statusCode":792100,
                "statusDescription":"No transaction for site modification opertaion" # I don't very understand this error.
            }"

###################################################################################################################    

"""Error 1: empty inputs2"""

Input:
        
        site = []
        
Response:
    
       "Site Created Failed! - 
            {
                "statusCode":791000,
                "statusDescription":"Null parameter: the parameter 'sites' cannot be null."
            }"


```
