
# Authentication and Authorization API

## ***PUT*** /V1/Session/CurrentDomain
Use this API to specify a domain to work on to get or set NetBrain data by associating domainID to the current session. 

## Detail Information

> **Title** : Specify a domain to work on API<br>

> **Version** : 01/23/2019.

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server /ServicesAPI/API/V1/Session/CurrentDomain

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|Bearer Authentication| Parameters | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|tenantId* | string  | Unique identifier for the tenant from which you desire to retrieve the domain information. tenantId can be retrieved from get all accessible tenants.  |
|domainId | string  | Input the ID of the target domain. Get a domain ID by using the API [Get all accessible domains of a tenant.](https://www.netbraintech.com/docs/ie71/help/get-all-accessible-domains-of-tenant.htm)<br> **Note**: This parameter is optioanl if the following operations aim only on tenant. |

> ### ***Example***


```python
{
    "tenantId": "fb24f3f0-81a7-1929-4b8f-99106c23fa5b",
    "domainId": "0201adc4-ae96-46f0-ae3d-01cdba9e41d6"
}
```

## Parameters(****required***)

> No parameters required.

## Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
| token | string  | Authentication token, get from login API. |

 ## Response

|**Name**|**Type**|**Description**|
|------|------|------|
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***


```python
{
    "statusCode": 790200,
    "statusDescription": "Success."
}
```

## Full Example :


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

# Set the request inputs
token = "4f257785-d5f9-42d4-b896-d21f0cb62e6f"
full_url = "http://192.168.28.79/ServicesAPI/API/V1/Session/CurrentDomain"
tenantId = "fb24f3f0-81a7-1929-4b8f-99106c23fa5b"
domainId = "0201adc4-ae96-46f0-ae3d-01cdba9e41d6"
    
# Set proper headers
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

body = {
        "tenantId": tenantId,
        "domainId": domainId
    }

try:
    # Do the HTTP request
    response = requests.put(full_url, data=json.dumps(body), headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        result = response.json()
        print (result)
    elif response.status_code != 200:
        print ("Login failed! - " + str(response.text))

except Exception as e: print (str(e))

```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

 # cURL Code from Postman


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Domains?token=c00de805-9210-44a9-9a26-f0c1e944ea36&tenantId=fb24f3f0-81a7-1929-4b8f-99106c23fa5b' \
  -H 'Postman-Token: ee6dda7c-cbcc-43b8-8957-9c4f2d2a4b5b' \
  -H 'cache-control: no-cache'
```

 ## Error Examples:


```python
###################################################################################################################    

"""Error 1: empty tenantId and domainId"""

Input:
    token = "4f257785-d5f9-42d4-b896-d21f0cb62e6f"
    full_url = "http://192.168.28.79/ServicesAPI/API/V1/Session/CurrentDomain"
    tenantId = ""
    domainId = ""

Response:
    "{
        "statusCode": 791000,
        "statusDescription": "Null parameter: the parameter 'tenantId' cannot be null."
     }"

###################################################################################################################    

"""Error 1: wrong tenantId"""

Input:
    token = "4f257785-d5f9-42d4-b896-d21f0cb62e6f"
    full_url = "http://192.168.28.79/ServicesAPI/API/V1/Session/CurrentDomain"
    tenantId = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
    domainId = "0201adc4-ae96-46f0-ae3d-01cdba9e41d6"

Response:
    "{
        "statusCode": 791004,
        "statusDescription": "Invalid tenant id."
    }"
```
