
# Site API Design

## ***POST*** /V1/CMDB/Sites/Leaf
Calling this API to create a container site. If one parent site doesn't exist in current system, create it before create its child site. 

Note: this API call needs to be invoked in a site transaction.

## Detail Information

> **Title** : Create A Leaf Sites API<br>

> **Version** : 02/04/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/Sites/Leaf

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|sitePath* | string  | Full path name of a site. For example, 'My Network/Site1/Boston'.  |

> ***Example***


```python
{
     "sitePath":"My Network/Site1/Boston/Dev"
}

##OR##
{
     "sitePath":"Site2/Boston/Dev"
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
token = "9603ce1d-8271-4f77-a2df-0b748ef32084"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Sites/Leaf"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

sitePath = "My Network/SiteT"

body = {
            "sitePath" : sitePath       
        }

try:
    response = requests.post(full_url, data = json.dumps(body), headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Leaf Site Created Failed! - " + str(response.text))
    
except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X POST \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/Sites/Leaf \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 3bbf50c6-e17e-4a4e-ab04-d3cbc3b36958' \
  -H 'cache-control: no-cache' \
  -H 'token: 1c52cd65-3247-44ad-91e6-cd73fc6c64a6' \
  -d '{
        "sitePath" : "My Network/Site1"       
    }'
```

# Error Example:


```python
# Same with "Create a parent site API"
```
