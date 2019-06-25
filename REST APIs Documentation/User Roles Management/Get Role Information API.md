
# Role API Design 7.1a

## ***GET*** /V1/CMDB/Roles{?roleName}
Calling this API to get role(s) information. If users provide a input value for "roleName", API will return the information of the input "roleName" respectively. If no specified "roleName" input, API will return all default roles information in Netbrain syatem.

## Detail Information

> **Title** : Get Role(s) information API<br>

> **Version** : 02/06/2019.

> **API Server URL** : http(s):// IP address of your NetBrain Web API Server /ServicesAPI/API/V1/CMDB/Roles

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Parameters | Authentication token | 

## Request body(****required***)

>No request body.

## Query Parameters(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
| roleName | string  | The role name. This field is the key to get the role information, if roleName = null or roleName == "", API will returns all role information. |

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
| token* | string  | Authentication token, get from login API. |

## Response

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|RoleData| list of object | List of role info.  |
|RoleData.roleName| string | The name of the created role, length between 0 and 128. |
|RoleData.description| string | The description of the role. |
|RoleData.privileges| list of string | The privileges belong to the role. |
|statusCode| integer | Code issued by NetBrain server indicating the execution result.  |
|statusDescription| string | The explanation of the status code. |

> ***Example***


```python
# Without roleName input.

{
    "RoleData": [
        {
            "roleName": "SystemAdmin",
            "newRoleName": null,
            "description": "System Admin",
            "privileges": [
                "systemManagement",
                "userManagement"
            ]
        },
        {
            "roleName": "TenantAdmin",
            "newRoleName": null,
            "description": "Tenant Admin",
            "privileges": [
                "tenantManagement"
            ]
        },
        {
            "roleName": "DomainAdmin",
            "newRoleName": null,
            "description": "Domain Admin",
            "privileges": [
                "domainManagement",
                "sharePolicyManagement",
                "deviceManagement",
                "sharedResourceManagement",
                "siteManagement",
                "discoverTuneNetworkDevice",
                "scheduleBenchmark",
                "manageNetworkSettings",
                "manageDeviceSettings",
                "accessToLiveNetwork",
                "monitor",
                "createNetworkChange",
                "executeNetworkChange",
                "mapLayoutManagement",
                "approveNetworkChange",
                "scheduleQapp",
                "viewNetworkChange",
                "variableMappingManagement"
            ]
        },
        {
            "roleName": "AddDomain",
            "newRoleName": null,
            "description": "Add Domain",
            "privileges": [
                "addDomain"
            ]
        },
        {
            "roleName": "TenantUser",
            "newRoleName": null,
            "description": "TenantUser",
            "privileges": [
                "tenantUser"
            ]
        },
        {
            "roleName": "DomainUser",
            "newRoleName": null,
            "description": "Domain User",
            "privileges": [
                "domainUser"
            ]
        },
        {
            "roleName": "PowerUser",
            "newRoleName": null,
            "description": "Power User",
            "privileges": [
                "domainManagement",
                "deviceManagement",
                "sharedResourceManagement",
                "siteManagement",
                "discoverTuneNetworkDevice",
                "scheduleBenchmark",
                "manageNetworkSettings",
                "manageDeviceSettings",
                "accessToLiveNetwork",
                "monitor",
                "createNetworkChange",
                "executeNetworkChange",
                "mapLayoutManagement",
                "approveNetworkChange",
                "scheduleQapp",
                "viewNetworkChange",
                "variableMappingManagement"
            ]
        },
        {
            "roleName": "Engineer",
            "newRoleName": null,
            "description": "Engineer",
            "privileges": [
                "manageDeviceSettings",
                "accessToLiveNetwork",
                "scheduleQapp",
                "sharedResourceManagement"
            ]
        },
        {
            "roleName": "Guest",
            "newRoleName": null,
            "description": "Guest",
            "privileges": [
                "accessToLiveNetwork"
            ]
        },
        {
            "roleName": "networkChangeCreator",
            "newRoleName": null,
            "description": "Network Change Creator",
            "privileges": [
                "domainManagement",
                "deviceManagement",
                "sharedResourceManagement",
                "siteManagement",
                "discoverTuneNetworkDevice",
                "scheduleBenchmark",
                "manageNetworkSettings",
                "manageDeviceSettings",
                "accessToLiveNetwork",
                "monitor",
                "mapLayoutManagement",
                "scheduleQapp",
                "createNetworkChange",
                "viewNetworkChange",
                "variableMappingManagement"
            ]
        },
        {
            "roleName": "networkChangeExecutor",
            "newRoleName": null,
            "description": "Network Change Executor",
            "privileges": [
                "domainManagement",
                "deviceManagement",
                "sharedResourceManagement",
                "siteManagement",
                "discoverTuneNetworkDevice",
                "scheduleBenchmark",
                "manageNetworkSettings",
                "manageDeviceSettings",
                "accessToLiveNetwork",
                "monitor",
                "mapLayoutManagement",
                "scheduleQapp",
                "executeNetworkChange",
                "viewNetworkChange",
                "variableMappingManagement"
            ]
        },
        {
            "roleName": "networkChangeApprover",
            "newRoleName": null,
            "description": "Network Change Approver",
            "privileges": [
                "domainManagement",
                "deviceManagement",
                "sharedResourceManagement",
                "siteManagement",
                "discoverTuneNetworkDevice",
                "scheduleBenchmark",
                "manageNetworkSettings",
                "manageDeviceSettings",
                "accessToLiveNetwork",
                "viewConfigurationFile",
                "monitor",
                "approveNetworkChange",
                "mapLayoutManagement",
                "scheduleQapp",
                "viewNetworkChange",
                "variableMappingManagement"
            ]
        },
        {
            "roleName": "UserManager",
            "newRoleName": null,
            "description": "User Manager",
            "privileges": [
                "userManagement"
            ]
        },
        {
            "roleName": "SystemManager",
            "newRoleName": null,
            "description": "System Manager",
            "privileges": [
                "systemManagement"
            ]
        },
        {
            "roleName": "Test Role",
            "newRoleName": null,
            "description": "",
            "privileges": [
                "domainManagement"
            ]
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
token = "220d6462-ba64-4058-83cb-affb2d55de78"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/Roles"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

roleName = ""

data = {
        "roleName":roleName
    }

try:
    response = requests.get(full_url, params = data, headers = headers, verify = False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Get Roles Information failed! - " + str(response.text))
    
except Exception as e:
    print (str(e))  

```

    {'RoleData': [{'roleName': 'SystemAdmin', 'newRoleName': None, 'description': 'System Admin', 'privileges': ['systemManagement', 'userManagement']}, {'roleName': 'TenantAdmin', 'newRoleName': None, 'description': 'Tenant Admin', 'privileges': ['tenantManagement']}, {'roleName': 'DomainAdmin', 'newRoleName': None, 'description': 'Domain Admin', 'privileges': ['domainManagement', 'sharePolicyManagement', 'deviceManagement', 'sharedResourceManagement', 'siteManagement', 'discoverTuneNetworkDevice', 'scheduleBenchmark', 'manageNetworkSettings', 'manageDeviceSettings', 'accessToLiveNetwork', 'monitor', 'createNetworkChange', 'executeNetworkChange', 'mapLayoutManagement', 'approveNetworkChange', 'scheduleQapp', 'viewNetworkChange', 'variableMappingManagement']}, {'roleName': 'AddDomain', 'newRoleName': None, 'description': 'Add Domain', 'privileges': ['addDomain']}, {'roleName': 'TenantUser', 'newRoleName': None, 'description': 'TenantUser', 'privileges': ['tenantUser']}, {'roleName': 'DomainUser', 'newRoleName': None, 'description': 'Domain User', 'privileges': ['domainUser']}, {'roleName': 'PowerUser', 'newRoleName': None, 'description': 'Power User', 'privileges': ['domainManagement', 'deviceManagement', 'sharedResourceManagement', 'siteManagement', 'discoverTuneNetworkDevice', 'scheduleBenchmark', 'manageNetworkSettings', 'manageDeviceSettings', 'accessToLiveNetwork', 'monitor', 'createNetworkChange', 'executeNetworkChange', 'mapLayoutManagement', 'approveNetworkChange', 'scheduleQapp', 'viewNetworkChange', 'variableMappingManagement']}, {'roleName': 'Engineer', 'newRoleName': None, 'description': 'Engineer', 'privileges': ['manageDeviceSettings', 'accessToLiveNetwork', 'scheduleQapp', 'sharedResourceManagement']}, {'roleName': 'Guest', 'newRoleName': None, 'description': 'Guest', 'privileges': ['accessToLiveNetwork']}, {'roleName': 'networkChangeCreator', 'newRoleName': None, 'description': 'Network Change Creator', 'privileges': ['domainManagement', 'deviceManagement', 'sharedResourceManagement', 'siteManagement', 'discoverTuneNetworkDevice', 'scheduleBenchmark', 'manageNetworkSettings', 'manageDeviceSettings', 'accessToLiveNetwork', 'monitor', 'mapLayoutManagement', 'scheduleQapp', 'createNetworkChange', 'viewNetworkChange', 'variableMappingManagement']}, {'roleName': 'networkChangeExecutor', 'newRoleName': None, 'description': 'Network Change Executor', 'privileges': ['domainManagement', 'deviceManagement', 'sharedResourceManagement', 'siteManagement', 'discoverTuneNetworkDevice', 'scheduleBenchmark', 'manageNetworkSettings', 'manageDeviceSettings', 'accessToLiveNetwork', 'monitor', 'mapLayoutManagement', 'scheduleQapp', 'executeNetworkChange', 'viewNetworkChange', 'variableMappingManagement']}, {'roleName': 'networkChangeApprover', 'newRoleName': None, 'description': 'Network Change Approver', 'privileges': ['domainManagement', 'deviceManagement', 'sharedResourceManagement', 'siteManagement', 'discoverTuneNetworkDevice', 'scheduleBenchmark', 'manageNetworkSettings', 'manageDeviceSettings', 'accessToLiveNetwork', 'viewConfigurationFile', 'monitor', 'approveNetworkChange', 'mapLayoutManagement', 'scheduleQapp', 'viewNetworkChange', 'variableMappingManagement']}, {'roleName': 'UserManager', 'newRoleName': None, 'description': 'User Manager', 'privileges': ['userManagement']}, {'roleName': 'SystemManager', 'newRoleName': None, 'description': 'System Manager', 'privileges': ['systemManagement']}, {'roleName': 'Test Role', 'newRoleName': None, 'description': '', 'privileges': ['domainManagement']}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman:


```python
curl -X GET \
  'http://192.168.28.79/ServicesAPI/API/V1/CMDB/Roles?roleName=' \
  -H 'Postman-Token: ad6afe31-0c72-4d5e-ae7b-99b26075f677' \
  -H 'cache-control: no-cache' \
  -H 'token: 220d6462-ba64-4058-83cb-affb2d55de78'
```

# Error Examples:


```python
###################################################################################################################    

"""Error 1: wrong inputs"""

Input:
    
        hostname = "Superman" # No role name called superman.
    
Response:
    
    "Get Roles Information failed! - 
        {
            "RoleData":[],
            "statusCode":791006,
            "statusDescription":"roleName Superman does not exist."
        }"
```
