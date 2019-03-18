
# Network Setting API Design

## ***PUT*** /V1/CMDB/NetworkSettings	
Call this API to edit a network setting. The alias will be used as the key. 

Other parameters will be same as add network setting. They can be optional if no change occurs.

## Detail Information

> **Title** : Update Network Settings API<br>

> **Version** : 01/30/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/NetworkSettings

> **Authentication** : 

|**Type**|**In**|**Name**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|------|------|------|
|<img width=100/>|<img width=100/>|<img width=500/>|
|privateKey* | object  | Used to add a private key. |
|privateKey.alias * | string  | The alias of the private key. This field is required.  |
|privateKey.keyName  | string  | The name of the private key. Optional |
|privateKey.md5KeyContent | string  | The MD5 content of the key. Optional  |
|privateKey.keyContent | string  | The content of the key. Optional |
|privateKey.passphrase | string  | The passphrase of the key. Optional  |
|jumpBox* | object  | Used to add a jumpbox setting. |
|jumpBox.alias * | string  | The alias of the jump box. This field is required.  |
|jumpBox.ipAddr | string  | The IP address of the jump box. Optional  |
|jumpBox.mode  | string  | The access mode from the Front Server to the Jump box. Options:<br>0: Telnet<br>1: SSH<br>2: SSH Public Key<br>This field is optional in modification.  |
|jumpBox.port  | int  | The port of the access port such as 23 for telnet. Optional  |
|jumpBox.userName  | string  | The username to access the jump box. This field is only required when the mode is 1 or 2.  |
|jumpBox.password  | string  | The password to access the jump box. This field is only required when the mode is 1. |
|jumpBox.keyName  | string  | The name of the SSH public key.  This field is only required when the mode is 2.  |
|jumpBox.loginPrompt  | string  | The login prompt to connect the jump box. This field is optional.  |
|jumpBox.passwordPrompt  | string  | The login password to connect the jump box. This field is optional.  |
|jumpBox.commandPrompt  | string  | The prompt to enter CLI command, such as >. This field is optional.  |
|jumpBox.yesNoPrompt | string  | The prompt to save the SSH key.  |
|jumpBox.telnetCommand | string  |Configure a special command to access via Telnet if necessary. For example,telnet $(IP) $(PORT). This field is optional.  |
|jumpBox.SSHCommand | string  | Configure a special command to access via SSH if necessary. For example, ssh -l $(USERNAME) -p $(PORT) $(IP). This field is optional. |
|jumpBox.cmd2 | string  | The login command for the special command. This field is optional.  |
|jumpBox.cmd2PasswordPrompt | string  | The login prompt to connect the jump box for the special command. This field is optional.  |
|jumpBox.cmd2Password  | string  | The login password to connect the jump box for the special command. This field is optional.  |
|jumpBox.cmd2ModePrompt  | string  | The mode prompt for the special command. This field is optional.  |
|jumpBox.quitCmd  | string  | The quit command. This field is optional. |
|telnetInfo* | object  | Used to add telnet/SSH login credentials. |
|telnetInfo.alias* | string  | The alias of telnet/SSH login credentials. This field is required. |
|telnetInfo.userName | string  | The user name of the non-privilege login. This field is optional. |
|telnetInfo.password  | string  | The password of of the non-privilege login. This field is optional. |
|telnetInfo.cliMode  | integer  | The authentication method. This field is optional, and cannot be used to change current mode. If different from the current setting, an error would return<br>options:<br>0, Telnet/SSH Password<br>2: SSH public key |
|telnetInfo.keyName  | string  | The name of the SSH public key. This field is only required when the current cli mode is 2 in system. |
|privilegeInfo* | object | Used to add privilege credentials.|
|privilegeInfo.alias* | string | The alias of the privilege credentials. This field is required.|
|privilegeInfo.userName  | string | The user name of the privilege login. This field is optional.|
|privilegeInfo.password  | string |The password of of the privilege login. This field is optional.|
|snmpInfo* | object | Used to add SNMP credentials. |
|snmpInfo.alias * | string | The alias of the privilege credentials. This field is required. |
|snmpInfo.snmpVersion | integer |the version of the SNMP string. This field is optional.<br>options:<br>1: SNMP v1 and v2c<br>3: SNMP v3<br>The other parameters are different based on SNMP version.<br>▪ SNMP v1 and v2c:roString (string): SNMP read-only community string.<br>▪ SNMP v3:v3 information will be effective|
|snmpInfo.roString | string | SNMP read-only community string. | 
|snmpInfo.v3 | object | Settings if SNMP v3 is applied in snmpInfo.snmpVersion, Optional. Effecitve only if snmpInfo.snmpVersion is 3. | 
|snmpInfo.v3.authMode  | integer | the authentication mode of SNMP V3.<br>Options:<br>1: no authen no priv<br>2: auth no priv<br>3: auth priv | 
|snmpInfo.v3.authPro | integer |  The authentication method of SNMP V3. Effective when snmpInfo.v3.authMode is 2 or 3<br>Options:<br>1: md5<br>2: SHA| 
|snmpInfo.v3.encryptPro | integer |The encryption method.Effective when snmpInfo.v3.authMode is 3. Optional.<br>Options:<br>1: DES<br>2: AES<br>3: AES192<br>4: AES256 | 
|snmpInfo.v3.userName  | string |  The user name to access the network devices. Optional. | 
|snmpInfo.v3.contextName  | string | The context name of authentication privilege. Optional. | 
|snmpInfo.v3.authPassword | string | The authentication password. This field is effective when the authMode is 2 or 3. | 
|snmpInfo.v3.encryptPassword | string | The encryption password. This field is effectiverequired when the authMode is 3. | 

> ***Example***


```python
privateKey = { "alias" : "nopass1111",
               "md5KeyContent" : "61e75483d397da0eda5a8dda67f13361",
               "keyContent" :   "8XmWX5It7yXvOw2J5UvziX@A==",
               "passwordPhrase" : "swed",
               "keyName" : "myRSA2048.ppk"}

telnetInfo = {"alias" : "nb1111",
              "password" : "were2",
              "cliMode" : 0, # 0:Telnet/SSH Password; 2: SSH public key
              "keyName" : "",
              "userName" : "x22"}

privilegeInfo = {"alias" : "bj_office1111",
                 "password" : "2dEd",
                 "userName" : "nl"}

snmpInfo = {"alias": "nb1111",
            "snmpVersion": 3,
            "v3": {"userName": "peter",
                   "contextName": "peter_c",
                   "authMode": 2, #1:no authen no priv 2:auth no priv 3:auth priv）
                   "authPro": 2,
                   "authPassword": "1"}
           }

jumpBox = {"alias": "nb1111",
           "mode" : 1,
           "port" : 22,
           "ipAddr" : "10.10.5.141",
           "password" : "abc12356sd",
           "userName" : "pliu"
          }

body = {
         "privateKey" : privateKey,
         "telnetInfo" : telnetInfo,
         "privilegeInfo" : privilegeInfo,
         "snmpInfo" : snmpInfo,
         "jumpBox" : jumpBox
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
    'statusCode': 790200, 
    'statusDescription': 'Success.'
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
token = "9c717c9a-4302-45b5-a068-2a3e9c4ea1a3"
nb_url = "http://192.168.28.79"
full_url = nb_url + "/ServicesAPI/API/V1/CMDB/NetworkSettings"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

privateKey = { "alias" : "nopass1111",
               "md5KeyContent" : "61e75483d397da0eda5a8dda67f13361",
               "keyContent" :   "8XmWX5It7yXvOw2J5UvziX@A==",
               "passwordPhrase" : "swed",
               "keyName" : "myRSA2048.ppk"}

telnetInfo = {"alias" : "nb1111",
              "password" : "",
              "cliMode" : 0, # 0:Telnet/SSH Password; 2: SSH public key
              "keyName" : "",
              "userName" : ""}

privilegeInfo = {"alias" : "bj_office1111",
                 "password" : "",
                 "userName" : ""}

snmpInfo = {"alias": "nb1111",
            "snmpVersion": 3,
            "v3": {"userName": "peter",
                   "contextName": "",
                   "authMode": 2, #1:no authen no priv 2:auth no priv 3:auth priv）
                   "authPro": 2,
                   "authPassword": "1"}
           }

jumpBox = {"alias": "nb1111",
           "mode" : 1,
           "port" : 22,
           "ipAddr" : "10.10.5.141",
           "password" : "abc12356sd",
           "userName" : "pliu"
          }

body = {
         "privateKey" : privateKey,
         "telnetInfo" : telnetInfo,
         "privilegeInfo" : privilegeInfo,
         "snmpInfo" : snmpInfo,
         "jumpBox" : jumpBox
     }

try:
    response = requests.put(full_url, data=json.dumps(body), headers=headers, verify=False)
    if response.status_code == 200:
        result = response.json()
        print (result)
    else:
        print ("Network settings update Failed! - " + str(response.text))

except Exception as e:
    print (str(e)) 
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X PUT \
  http://192.168.28.79/ServicesAPI/API/V1/CMDB/NetworkSettings \
  -H 'Content-Type: application/json' \
  -H 'Postman-Token: 55f96d7f-e2d7-479b-bd2a-7cccde000708' \
  -H 'cache-control: no-cache' \
  -H 'token: 9c717c9a-4302-45b5-a068-2a3e9c4ea1a3' \
  -d '{
        "privateKey" : 
            { 
                "alias" : "nopass23",
                "md5KeyContent" : "61e75483d397da0eda5a8dda67f13361",
                "keyContent" :   "8XmWX5It7yXvOw2J5UvziX@A==",
                "passwordPhrase" : "swed",
                "keyName" : "myRSA2048.ppk"

            },

        "telnetInfo" : 
            {
                "alias" : "nb23",
                "password" : "were2",
                "cliMode" : 0, 
                "keyName" : "",
                "userName" : "x22"

            },

        "privilegeInfo" : 
            {
                "alias" : "bj_office2",
                "password" : "2dEd",
                "userName" : "nl"
            },

        "snmpInfo" : 
            {
                "alias": "nb23",
                "snmpVersion": 3,

                "v3": 
                {
                    "userName": "peter",
                    "contextName": "peter_c",
                    "authMode": 2,
                    "authPro": 2,
                    "authPassword": "1"
                }
            },

        "jumpBox" : 
            {
                "alias": "nb3",
                "mode" : 1,
                "port" : 22,
                "ipAddr" : "10.10.5.141",
                "password" : "abc12356sd",
                "userName" : "pliu"
            }
    }'
```

# Error Examples:
Same with "Add Network Settings API"
