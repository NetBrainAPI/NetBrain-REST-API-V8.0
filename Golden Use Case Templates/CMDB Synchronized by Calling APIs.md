
# CMDB Synchronized
During current use case, we are going to output One-Ip table of current domain devices, modify the devices attributes and return all detail informations of device interface attributes. The step sequential is showing below:

**[Step 1: Use case preparation](Step-1:-Use-case-preparation)**
>> 1a. import all useful modules and create global variables<br>
>> 1b. call login API<br>
>> 1e. call specify_a_working_domain API<br>

**[Step 2: Get One-Ip Table of current domain](Step-2:-Get-One-Ip-Table-of-current-domain)**
>> 2a. call get_all_devices API<br>
>> 2b. call get_one_ip_table API<br>

**[Step 3: Modify devices attributes of current domain](Step-3:-Modify-devices-attributes-of-current-domain)**
>> 3a. call get_devices_attribute API<br>
>> 3b. call create_devices_attribute API<br>
>> 3c. call set_devices_attribute API<br>

**[Step 4: Retrieve interface attributes of current domain](Step-4:-Retrieve-interface-attributes-of-current-domain)**
>> 4a. call get_all_interfaces_of_devices API<br>
>> 4b. call get_interfaces_attributes API<br>

## Step 1: Use case preparation
***1a. import all useful modules and create global variables***
> Note: If users try to use this code. please remember to change the "nb_url" to users' own working url.

***1b. call login API***
>In step 1, we calling the login API with "username" and "password" as inputs. As response we can get the authentication token as one fixed input in following APIs calling. If users get errors when calling this API please check the API documentation on [Github_login](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Login%20API.md) 

***1c. call specify_a_working_domain API***
>After we running this step successfully, we directly complete the full login processes which means we totally join in Netbrain System by calling APIs(because we have record our tenantId and domainId，if users don't know the ID of corresponding tenant and domain please fully follow step 1 to step 4 in use case 1). Next step, we will start to use Netbrain functions formally. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_domain](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Specify%20A%20Working%20Domain%20API.md) 


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

nb_url = "http://192.168.28.79"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'} 
TenantName = "Initial Tenant"
DomainName = "Support and Service"
username = "gdluserTest"
password = "123456"
tenantId = "fb24f3f0-81a7-1929-4b8f-99106c23fa5b"
domainId = "850ff5e9-c639-404d-85a3-d920dbee509c"
```


```python
# call login API

body = {
    "username" : username,      
    "password" : password  
}

login_URL = nb_url + "/ServicesAPI/API/V1/Session"

def login(login_URL, body, headers):
    try:
        # Do the HTTP request
        response = requests.post(login_URL, headers=headers, data = json.dumps(body), verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            js = response.json()
            return (js["token"])
        else:
            return ("Get token failed! - " + str(response.text))
    except Exception as e:
        return (str(e))
    
token = login(login_URL, body, headers)
print(token) # print out the authentication token.
```

API Response:    d4c80094-cf9b-424b-9548-fca8050622b2
    


```python
# call specify_a_working_domain API

Specify_a_working_domain_url = nb_url + "/ServicesAPI/API/V1/Session/CurrentDomain"

def specify_a_working_domain(tenantId, domainId, Specify_a_working_domain_url, headers, token):
    headers["Token"] = token
    body = {
        "tenantId": tenantId,
        "domainId": domainId
    }
    
    try:
        # Do the HTTP request
        response = requests.put(Specify_a_working_domain_url, data=json.dumps(body), headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()
            return ("Working Domain Specified Successfully, with domainId: " + domainId)
            
        elif response.status_code != 200:
            return ("Login failed! - " + str(response.text))

    except Exception as e: print (str(e))

res =  specify_a_working_domain(tenantId, domainId, Specify_a_working_domain_url, headers, token)
print (res)
```

API Response:     Working Domain Specified Successfully, with domainId: 850ff5e9-c639-404d-85a3-d920dbee509c
    

## Step 2: Get One-Ip Table of current domain
***2a. call get_all_devices API***
>In this use case, we are going to get One-Ip Table, devices details and interface information. So the precondition of calling path APIs is the information of devices. In current section, after we calling this Api successfully, we will get information of all devices in current domain. As following, I provide a sub-section as a helper to filt information, because after calling get all devices API we will get a json file from API response, it 's include device id, device management ip, device hostname and some other information. But we only need the managment Ip as input for next section. Thus, we provide a small funtion to filt out the "mgmIp" from json file.
If users get errors when calling this API please check the API documentation on [Github_devices](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Devices%20Management/Get%20Device%20API.md) 

***2b. call get_one_ip_table API***
>After we got all devices management IP from step 5, we can calling the Get One-Ip table API. During this step, we totally call twice of corresponding API, first calling we loop all devices in current domian with full input parameters provided, second call we only provide the "beginIndex" and "count" parameters. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_One-Ip_Table](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Topology%20Management/Get%20One%20Ip-Table%20API.md) 


```python
# call get_all_devices API

Get_all_devices_in_domain_url = nb_url + "/ServicesAPI/API/V1/CMDB/Devices"

def get_all_devices(Get_all_devices_in_domain_url, headers, token):
    try:
        response = requests.get(Get_all_devices_in_domain_url, headers=headers, verify=False)
        if response.status_code == 200:
            result = response.json()
            devices = result["devices"]
            return devices
        else:
            return("Get Devices failed! - " + str(response.text))
    except Exception as e:
        return (str(e)) 
    
devices = get_all_devices(Get_all_devices_in_domain_url, headers, token)

print("Total Devices Number: " + str(len(devices)))

devices[0:10]
```

API Response:     Total Devices Number: 98
    




    [{'id': '1266a178-b829-43c8-9c24-c34154a15d30',
      'mgmtIP': '192.168.28.204',
      'hostname': 'R20',
      'deviceTypeName': 'Cisco Router',
      'firstDiscoverTime': '0001-01-01T00:00:00',
      'lastDiscoverTime': '0001-01-01T00:00:00'},
     {'id': '497b25bd-1f8c-4bfa-80be-49ab692ce4d4',
      'mgmtIP': '123.10.1.10',
      'hostname': 'R3',
      'deviceTypeName': 'Cisco Router',
      'firstDiscoverTime': '0001-01-01T00:00:00',
      'lastDiscoverTime': '0001-01-01T00:00:00'},
     {'id': 'f190b385-676f-4579-ad6d-700122a21caf',
      'mgmtIP': '123.10.1.17',
      'hostname': 'R2',
      'deviceTypeName': 'Cisco Router',
      'firstDiscoverTime': '0001-01-01T00:00:00',
      'lastDiscoverTime': '0001-01-01T00:00:00'},
     {'id': '1d48d218-06cf-4657-af2c-39796946122b',
      'mgmtIP': '123.10.1.1',
      'hostname': 'R4',
      'deviceTypeName': 'Cisco Router',
      'firstDiscoverTime': '0001-01-01T00:00:00',
      'lastDiscoverTime': '0001-01-01T00:00:00'},
     {'id': '81229708-571a-419a-a10d-9481661718a4',
      'mgmtIP': '123.10.1.2',
      'hostname': 'R1',
      'deviceTypeName': 'Cisco Router',
      'firstDiscoverTime': '0001-01-01T00:00:00',
      'lastDiscoverTime': '0001-01-01T00:00:00'},
     {'id': '6d62e420-af59-4ee3-948d-54df60fe05ca',
      'mgmtIP': '123.10.1.6',
      'hostname': 'R5',
      'deviceTypeName': 'Cisco Router',
      'firstDiscoverTime': '0001-01-01T00:00:00',
      'lastDiscoverTime': '0001-01-01T00:00:00'},
     {'id': '5c3d72d6-d0f2-41f4-8b1e-5762dff6e55a',
      'mgmtIP': '123.10.1.22',
      'hostname': 'R6',
      'deviceTypeName': 'Cisco Router',
      'firstDiscoverTime': '0001-01-01T00:00:00',
      'lastDiscoverTime': '0001-01-01T00:00:00'},
     {'id': 'b98f107a-622e-4985-8f95-f5b541f699f3',
      'mgmtIP': '123.7.7.7',
      'hostname': 'R7',
      'deviceTypeName': 'Cisco Router',
      'firstDiscoverTime': '0001-01-01T00:00:00',
      'lastDiscoverTime': '0001-01-01T00:00:00'},
     {'id': '9b60fcfc-a405-478d-83a3-99b0ce6c9b64',
      'mgmtIP': '123.8.8.8',
      'hostname': 'R8',
      'deviceTypeName': 'Cisco Router',
      'firstDiscoverTime': '0001-01-01T00:00:00',
      'lastDiscoverTime': '0001-01-01T00:00:00'},
     {'id': '087814b3-f372-4878-bdcd-c31ba061c864',
      'mgmtIP': '123.10.10.10',
      'hostname': 'R10',
      'deviceTypeName': 'Cisco Router',
      'firstDiscoverTime': '0001-01-01T00:00:00',
      'lastDiscoverTime': '0001-01-01T00:00:00'}]




```python
# A sub-section for filt out the "mgmIp" from json file.
ips = []

for i in range(len(devices)):
    ip = devices[i]["mgmtIP"]
    if ip != "":
        ips.append(ip)
  
print(str(len(ips)))
ips[0:10]
```

API Response:     93
    




    ['192.168.28.204',
     '123.10.1.10',
     '123.10.1.17',
     '123.10.1.1',
     '123.10.1.2',
     '123.10.1.6',
     '123.10.1.22',
     '123.7.7.7',
     '123.8.8.8',
     '123.10.10.10']




```python
Get_One_Ip_Table_url = nb_url + "/ServicesAPI/API/V1/CMDB/Topology/OneIPTable"
results_with_Ip_Input = []
results = []

def get_one_ip_table(Get_One_Ip_Table_url, ip, beginIndex, count, headers, token):
    headers["Token"] = token
    data = {
        "ip" : ip,
        "beginIndex" : beginIndex,
        "count" : count
    }
    try:
        response = requests.get(Get_One_Ip_Table_url, params = data, headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            return (result)
        else:
            return ("Get One-Ip Table failed! - " + str(response.text))

    except Exception as e:
        return (str(e))  

# First Calling
for i in range(len(ips)):
    result = get_one_ip_table(Get_One_Ip_Table_url, ips[i], 0, 3, headers, token)
    #res = result["OneIPList"]
    results.append(result)

print(str(len(results)))
results[0:10]

```

API Response:     93
    




    [{'OneIPList': [{'lanSegment': '192.168.28.0/22',
        'ip': '192.168.28.204',
        'mac': 'AABB.CC00.2210',
        'devName': 'R20',
        'interfaceName': 'Ethernet0/1',
        'switchName': '',
        'portName': '',
        'alias': '',
        'dns': 'R20.Ethernet0/1',
        'sourceDevice': 'R20',
        'serverType': 2,
        'switchType': 2001,
        'updateTime': '2019-02-25T06:04:17Z',
        'userFlag': 9,
        'source': 'Device Interface',
        'vendor': '',
        'descr': ''}],
      'statusCode': 790200,
      'statusDescription': 'Success.'},
     {'OneIPList': [{'lanSegment': '123.10.1.8/30',
        'ip': '123.10.1.10',
        'mac': 'AABB.CC00.0710',
        'devName': 'R3',
        'interfaceName': 'Ethernet0/1',
        'switchName': '',
        'portName': '',
        'alias': '',
        'dns': 'R3.Ethernet0/1',
        'sourceDevice': 'R3',
        'serverType': 2,
        'switchType': 2001,
        'updateTime': '2019-02-25T06:04:18Z',
        'userFlag': 9,
        'source': 'Device Interface',
        'vendor': '',
        'descr': ''}],
      'statusCode': 790200,
      'statusDescription': 'Success.'},
     {'OneIPList': [{'lanSegment': '123.10.1.16/30',
        'ip': '123.10.1.17',
        'mac': 'AABB.CC00.0620',
        'devName': 'R2',
        'interfaceName': 'Ethernet0/2',
        'switchName': '',
        'portName': '',
        'alias': '',
        'dns': 'R2.Ethernet0/2',
        'sourceDevice': 'R2',
        'serverType': 2,
        'switchType': 2001,
        'updateTime': '2019-02-25T06:04:17Z',
        'userFlag': 9,
        'source': 'Device Interface',
        'vendor': '',
        'descr': ''}],
      'statusCode': 790200,
      'statusDescription': 'Success.'},
     {'OneIPList': [{'lanSegment': '123.10.1.0/30',
        'ip': '123.10.1.1',
        'mac': 'AABB.CC00.0810',
        'devName': 'R4',
        'interfaceName': 'Ethernet0/1',
        'switchName': '',
        'portName': '',
        'alias': '',
        'dns': 'R4.Ethernet0/1',
        'sourceDevice': 'R4',
        'serverType': 2,
        'switchType': 2001,
        'updateTime': '2019-02-25T06:04:17Z',
        'userFlag': 9,
        'source': 'Device Interface',
        'vendor': '',
        'descr': ''}],
      'statusCode': 790200,
      'statusDescription': 'Success.'},
     {'OneIPList': [{'lanSegment': '123.10.1.0/30',
        'ip': '123.10.1.2',
        'mac': 'AABB.CC00.0520',
        'devName': 'R1',
        'interfaceName': 'Ethernet0/2',
        'switchName': '',
        'portName': '',
        'alias': '',
        'dns': 'R1.Ethernet0/2',
        'sourceDevice': 'R1',
        'serverType': 2,
        'switchType': 2001,
        'updateTime': '2019-02-25T06:04:17Z',
        'userFlag': 9,
        'source': 'Device Interface',
        'vendor': '',
        'descr': ''}],
      'statusCode': 790200,
      'statusDescription': 'Success.'},
     {'OneIPList': [{'lanSegment': '123.10.1.4/30',
        'ip': '123.10.1.6',
        'mac': 'AABB.CC00.0910',
        'devName': 'R5',
        'interfaceName': 'Ethernet0/1',
        'switchName': '',
        'portName': '',
        'alias': '',
        'dns': 'R5.Ethernet0/1',
        'sourceDevice': 'R5',
        'serverType': 2,
        'switchType': 2001,
        'updateTime': '2019-02-25T06:04:17Z',
        'userFlag': 9,
        'source': 'Device Interface',
        'vendor': '',
        'descr': ''}],
      'statusCode': 790200,
      'statusDescription': 'Success.'},
     {'OneIPList': [{'lanSegment': '123.10.1.20/30',
        'ip': '123.10.1.22',
        'mac': 'AABB.CC00.0A20',
        'devName': 'R6',
        'interfaceName': 'Ethernet0/2',
        'switchName': '',
        'portName': '',
        'alias': '',
        'dns': 'R6.Ethernet0/2',
        'sourceDevice': 'R6',
        'serverType': 2,
        'switchType': 2001,
        'updateTime': '2019-02-25T06:04:18Z',
        'userFlag': 9,
        'source': 'Device Interface',
        'vendor': '',
        'descr': ''}],
      'statusCode': 790200,
      'statusDescription': 'Success.'},
     {'OneIPList': [], 'statusCode': 790200, 'statusDescription': 'Success.'},
     {'OneIPList': [], 'statusCode': 790200, 'statusDescription': 'Success.'},
     {'OneIPList': [], 'statusCode': 790200, 'statusDescription': 'Success.'}]



## Step 3: Modify devices attributes of current domain
***3a. call get_devices_attribute API***
> Call this API to get the value for an attribute of a device, get all attributes if attribute name is not specifed. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_get_devices_attribute](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Devices%20Management/Get%20Device%20Attributes%20API.md) 

***3b. call create_devices_attribute API***
>Call this API to create a customized attribute for certain device types. User can use the SetDeviceAttribute API to set a value for the created attribute. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_create_devices_attribute](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Devices%20Management/Create%20Devices%20Attribute%20API.md) 

***3c. call set_devices_attribute API***
>Call this API to set a value for the specified attriute of a device. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_set_devices_attribute](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Devices%20Management/Set%20Device%20Attribute%20API.md)


```python
# A sub-section for filt out the "mgmIp" from json file.
hostnames = []

for i in range(len(devices)):
    hostname = devices[i]["hostname"]
    if hostname != "":
        hostnames.append(hostname)
  
print(str(len(hostnames)))
hostnames[0:10]
```

API Response:     98
    




    ['R20', 'R3', 'R2', 'R4', 'R1', 'R5', 'R6', 'R7', 'R8', 'R10']




```python
# call get_devices_attribute API

resultsHost = []

get_devices_attribute_url = nb_url + "/ServicesAPI/API/V1/CMDB/Devices/Attributes"

def get_devices_attribute(get_devices_attribute_url, hostname, headers, token):
    headers["Token"] = token
    data = {
        "hostname":hostname
    }
    try:
        response = requests.get(get_devices_attribute_url, params = data, headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()["attributes"]
            return (result)
        else:
            return ("Get device attributes failed! - " + str(response.text))

    except Exception as e:
        return (str(e))
        
for i in range(len(hostnames)):
    result = get_devices_attribute(get_devices_attribute_url, hostnames[i], headers, token)
    #res = result["OneIPList"]
    resultsHost.append(result)

print(str(len(resultsHost)))
resultsHost[0]

```

API Response:     98
    




    {'name': 'R20',
     'mgmtIP': '192.168.28.204',
     'mgmtIntf': 'Ethernet0/1',
     'subTypeName': 'Cisco Router',
     'vendor': 'Cisco',
     'model': 'DEVELOPMENT TEST SOFTWARE',
     'ver': '15.4(2)T4',
     'sn': '71372834',
     'site': 'My Network\\SAN JOSE',
     'loc': '',
     'contact': '',
     'mem': '356640420',
     'assetTag': '',
     'layer': '',
     'descr': '',
     'oid': '1.3.6.1.4.1.9.1.1',
     'driverName': 'Cisco Router',
     'fDiscoveryTime': {'$date': 1545934859000},
     'lDiscoveryTime': {'$date': 1548883406000},
     'assignTags': '',
     'hasBGPConfig': True,
     'hasOSPFConfig': False,
     'hasEIGRPConfig': False,
     'hasISISConfig': False,
     'hasMulticastConfig': False,
     'TestTable': ''}




```python
# call create_devices_attribute API

create_devices_attribute_url = nb_url + "/ServicesAPI/API/V1/CMDB/Devices/Attributes"

attributeName = "time"
attributeDisplayName = "time"
deviceTypeNames = "null"
dataType = "string"
subDataType = "null"
isFullSearch = True

body={
        "attributeName": attributeName,
        "attributeDisplayName": attributeDisplayName,
        "deviceTypeNames": deviceTypeNames, 
        "dataType": dataType,
        "subDataType" : subDataType,
        "isFullSearch": isFullSearch
    }

def create_devices_attribute(create_devices_attribute_url, body, headers, token):
    headers["Token"] = token
    try:
        response = requests.post(create_devices_attribute_url, data=json.dumps(body), headers=headers, verify=False)
        if response.status_code == 200:
            result = response.json()
            return (result)
        else:
            return ("Create device attribute failed! - " + str(response.text))

    except Exception as e:
        return (str(e))
    
res = create_devices_attribute(create_devices_attribute_url, body, headers, token)
res
```




API Response:     {'statusCode': 790200, 'statusDescription': 'Success.'}




```python
# call set_devices_attribute API

import datetime

set_devices_attribute_url = nb_url + "/ServicesAPI/API/V1/CMDB/Devices/Attributes"

def set_devices_attribute(set_devices_attribute_url, hostname, attributeValue, headers, token):
    headers["Token"] = token
    body = {
            "hostname": hostname,
            "attributeName": "time",
            "attributeValue": attributeValue
        }
    try:
        response = requests.put(set_devices_attribute_url, data=json.dumps(body), headers=headers, verify=False)
        if response.status_code == 200:
            result = response.json()
            return (result)
        else:
            return ("Set device attribute failed! - " + str(response.text))

    except Exception as e:
            return (str(e))
        
for i in hostnames:
    attributeValue = str(datetime.datetime.now())
    result = set_devices_attribute(set_devices_attribute_url, i, attributeValue, headers, token)
    print(result)

```
API Response: 

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    


```python
resultsHost1 = []

for i in range(len(hostnames)):
    result = get_devices_attribute(get_devices_attribute_url, hostnames[i], headers, token)
    #res = result["OneIPList"]
    resultsHost1.append(result)

print(str(len(resultsHost1)))
resultsHost1[0]

```

API Response:     98
    




    {'name': 'R20',
     'mgmtIP': '192.168.28.204',
     'mgmtIntf': 'Ethernet0/1',
     'subTypeName': 'Cisco Router',
     'vendor': 'Cisco',
     'model': 'DEVELOPMENT TEST SOFTWARE',
     'ver': '15.4(2)T4',
     'sn': '71372834',
     'site': 'My Network\\SAN JOSE',
     'loc': '',
     'contact': '',
     'mem': '356640420',
     'assetTag': '',
     'layer': '',
     'descr': '',
     'oid': '1.3.6.1.4.1.9.1.1',
     'driverName': 'Cisco Router',
     'fDiscoveryTime': {'$date': 1545934859000},
     'lDiscoveryTime': {'$date': 1548883406000},
     'assignTags': '',
     'hasBGPConfig': True,
     'hasOSPFConfig': False,
     'hasEIGRPConfig': False,
     'hasISISConfig': False,
     'hasMulticastConfig': False,
     'TestTable': '',
     'time': '2019-02-25 16:52:26.571397'}



## Step 4: Retrieve interface attributes of current domain
***4a. call get_all_interfaces_of_devices API***
>Call this API to get all interfaces of one device which specified by hostname. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_get_all_interfaces_of_devices](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Device%20Interfaces%20Management/Get%20All%20Interfaces%20of%20A%20Device%20API.md)

***4b. call get_interfaces_attributes API***
>Call this API to get the value for a specified attribute of a device interface, get all attributes if the attribute name is not specifed. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_get_interfaces_attributes](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Device%20Interfaces%20Management/Get%20Interface%20Attributes%20API.md)


```python
# call get_all_interfaces_of_devices API

get_all_interfaces_of_devices_url = nb_url + "/ServicesAPI/API/V1/CMDB/Interfaces"

def get_all_interfaces_of_devices(get_all_interfaces_of_devices_url, headers, token, hostname):
    headers["Token"]=token
    try:
        # Do the HTTP request
        response = requests.get(get_all_interfaces_of_devices_url, headers=headers, params = {"hostname" : hostname}, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()["interfaces"]
            return (result)
        else:
            return ("Get interfaces failed! - " + str(response.text))

    except Exception as e:
        return (str(e))

resultsHost2 = []
for i in hostnames:
    result = get_all_interfaces_of_devices(get_all_interfaces_of_devices_url, headers, token, i)
    #res = result["OneIPList"]
    resultsHost2.append(result)

print(str(len(resultsHost2)))
resultsHost2[0]  
```

API Response:     98
    




    ['Loopback0',
     'Ethernet0/0',
     'Ethernet0/1',
     'Ethernet0/2',
     'Ethernet0/3',
     'Ethernet1/0',
     'Ethernet1/1',
     'Ethernet1/2',
     'Ethernet1/3',
     'Ethernet2/0',
     'Ethernet2/0.12',
     'Ethernet2/0.13',
     'Ethernet2/0.14',
     'Ethernet2/0.15',
     'Ethernet2/0.99',
     'Ethernet2/0.100',
     'Ethernet2/1',
     'Ethernet2/2',
     'Ethernet2/3',
     'Ethernet3/0',
     'Ethernet3/0.12',
     'Ethernet3/0.13',
     'Ethernet3/0.14',
     'Ethernet3/0.15',
     'Ethernet3/0.99',
     'Ethernet3/1',
     'Ethernet3/2',
     'Ethernet3/3']




```python
# call get_interfaces_attributes API

get_interfaces_attributes_url = nb_url + "/ServicesAPI/API/V1/CMDB/Interfaces/Attributes"

def get_interfaces_attributes(get_interfaces_attributes_url, headers, token, hostname, interfaceName):
    headers["Token"]=token
    data = {
            "hostname":hostname,
            "interfaceName":interfaceName
        }

    try:
        response = requests.get(get_interfaces_attributes_url, params = data, headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()["attributes"]
            return (result)
        else:
            return ("Get interface attributes failed! - " + str(response.text))

    except Exception as e:
        return (str(e))

resS = []
#for i in range(98):
hostname = hostnames[0]
interF = resultsHost2[0]
#for j in interF:
res = get_interfaces_attributes(get_interfaces_attributes_url, headers, token, hostname, interF)
resS.append(res)

print(str(len(resS)))
resS[0]  
```

API Response:     1
    




    {'Loopback0': {'name': 'Loopback0',
      'ips': [{'ip': 2064913428, 'ipLoc': '123.20.20.20/32', 'maskLen': 32}],
      'ipv6s': '',
      'ipv6LinkLocalAddress': '',
      'mibIndex': 19,
      'bandwidth': 8000000,
      'speed': '',
      'duplex': '',
      'intfStatus': 'up/up',
      'macAddr': '',
      'moduleSlot': '',
      'moduleType': '',
      'descr': '',
      'routingProtocol': '',
      'multicastMode': '',
      'mplsVrf': '',
      'inAclName': '',
      'outAclName': '',
      'mode': '',
      'vlan': '',
      'trunkNativeVlan': '',
      'trunkEncapsulation': '',
      'ipUnnumberedIp': '',
      'ss': ''}}




```python

```
