
# Discovery Devices and Check Devices Detail Informations
During this use case, we are going to focus on using discovery APIs to discover devices which we are willing to modify. As we know, the discovery APIs are provide very high utility in all of APIs and centralize on devices modification.

In this use case, we totally concern 11 APIs, as shown below:

**[Step 1: Use case preparation.](Step-1:-Use-case-preparation.)**
>>1a. import all useful modules and create global variables<br>
>>1b. call login API<br>
>>1c. call specify_a_working_domain API<br>

**[Step 2: Specify discovery task can be used and check task status.](Step-2:-Specify-discovery-task-can-be-used-and-check-task-status.)** 
>>2a. call get_all_discovery_taske API<br>
>>2b. call get_discovery_task_status API

**[Step 3: Modify devices information in discovery task.](Step-3:-Modify-devices-information-in-discovery-task.)**
>>3a. call add_seed_Ips_to_discovery API<br>
>>3b. call delete_seed_Ips_to_discovery API<br>
>>3c. call get_all_seed_Ips_from_discovery

**[Step 4: Run the task and check the running result.](Step-4:-Run-the-task-and-check-the-running-result.)**
>>4a. call run_discovery_task_now API<br>
>>4b. call get_discovery_tasks_result API<br>
>>4c. call get_discovery_live_access API



## Step 1: Use case preparation.
***1a. import the corresponding modules in python and some fixed input parameters.***<br>
> Note: If users try to use this code. please remember to change the "nb_url" to users' own working url.

***1b. login API.***<br>
>Same with use case 2, we calling the login API with "username" and "password" as inputs in the first step. As response we can get the authentication token as one fixed input in following APIs calling. If users get errors when calling this API please check the API documentation on [Github_login](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Login%20API.md).

***1b. specify_a_working_domain API.***<br>
>After we running this step successfully, we directly complete the full login processes which means we totally join in Netbrain System by calling APIs(because we have record our tenantId and domainId，if users don't know the ID of corresponding tenant and domain please fully follow step 1 to step 4 in use case 1). Next step, we will start to use Netbrain functions formally. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_domain](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Specify%20A%20Working%20Domain%20API.md).



```python
# import python modules and provide global variables.
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
tenant_id = "fb24f3f0-81a7-1929-4b8f-99106c23fa5b"
domain_id = "850ff5e9-c639-404d-85a3-d920dbee509c"
task_name = "gdlDisTask1"
```


```python
# Calling Login API

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

API Response:     aeada487-9cbf-4459-8796-3845feaf65af
    


```python
#specify_a_working_domain API.

Specify_a_working_domain_url = nb_url + "/ServicesAPI/API/V1/Session/CurrentDomain"

def specify_a_working_domain(tenant_id, domain_id, Specify_a_working_domain_url, headers, token):
    headers["Token"] = token
    body = {
        "tenantId": tenant_id,
        "domainId": domain_id
    }
    
    try:
        # Do the HTTP request
        response = requests.put(Specify_a_working_domain_url, data=json.dumps(body), headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()
            return ("Working Domain Specified Successfully, with domainId: " + domain_id)
            
        elif response.status_code != 200:
            return ("Login failed! - " + str(response.text))

    except Exception as e: print (str(e))

res =  specify_a_working_domain(tenant_id, domain_id, Specify_a_working_domain_url, headers, token)
print (res)
```

API Response:     Working Domain Specified Successfully, with domainId: 850ff5e9-c639-404d-85a3-d920dbee509c
    

## Step 2: Specify discovery task can be used and check task status.
During this step, we need to choose or create one discovery task for following operatings and if it is an exist task, the status also need to be checked.

***2a. Calling get all discovery tasks API***
> Call this API to get all discovery tasks from current domain. And to specify the taskId of the task which we going to use. If there are no discovery tasks in the system, add a new discovery task from the Domain Management page. In the discovery task, select Once on the Frequency tab and Discover the following IPs on the Discovery Seed tab. (more details: [Github_get_all_discovery_task](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Discovery%20Task%20Management/Get%20All%20Discovery%20Tasks%20API.md)).

***2b. Calling get discovery task status API***
> Call this API to get the status of the discovery task which identified by task ID. The reason why we calling this API at here is to confirm the discovery task can be used or not. (more details: [Github_get_all_discovery_task](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Discovery%20Task%20Management/Get%20All%20Discovery%20Tasks%20API.md)).


```python
get_all_discovery_taske_url = nb_url + "/ServicesAPI/API/V1/CMDB/Discovery/Tasks"

def get_all_discovery_task(get_all_discovery_taske_url, headers, token):
    headers["Token"] = token
    try:
        # Do the HTTP request
        response = requests.get(get_all_discovery_taske_url, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()["tasks"]
            for i in result:
                if i["name"] == task_name:
                    return i["id"]
        else:
            return ("Get discovery task list failed - " + str(response.text))

    except Exception as e:
        return (str(e)) 
    
taskId = get_all_discovery_taske(get_all_discovery_taske_url, headers, token)
taskId
```




API Response:     '1227fb4f-356d-76c8-d140-04d34ffaf79a'




```python
get_discovery_task_status_url = nb_url + "/ServicesAPI/API/V1/CMDB/Discovery/Tasks/"+str(taskId)+"/Status"

'''statusList = {
    -1: "Unknown",
     0: "Never run",
     2: "Running",
    10: "Succeeded",
    11: "Succeeded with warnings",
    20: "Failed",
    30: "Manually stopped",
    31: "Automatically stopped due to timeout set by users or another system setting"
}'''

def get_discovery_task_status(get_discovery_task_status_url, headers, token):
    try:
        # Do the HTTP request
        response = requests.get(get_discovery_task_status_url, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()["taskStatus"]
            #task_status = statusList[result]
            return ("Task Status: " + str(result))
        else:
            return("Get Running Status failed - " + str(response.text))

    except Exception as e:
        return (str(e)) 
    
status = get_discovery_task_status(get_discovery_task_status_url, headers, token)
status
```




API Response:     'Task Status: Succeeded with warnings'



## Step 3: Modify devices information in discovery task.
After Step 2, we are going to discover devices by using the task we have specified in this step. 

***3a. Calling add_seed_Ips_to_discovery API***
>Call this API to add a list of target ip addresses as seeds to an EXISTING scheduled discovery task along with optional cli information for each ip. (more details: [Github_add_seed_Ips_to_discovery](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Discovery%20Task%20Management/Add%20Seed%20IPs%20to%20Discovery%20Task%20API.md)).

***3b. Calling delete_seed_Ips_to_discovery API***
>Call this API to remove specific IP addresses from a discovery task, if list is empty, remove all. This calling is optional, users don't need to call this API if there is no problems in previous input devices list.(more details: [Github_delete_seed_Ips_to_discovery](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Discovery%20Task%20Management/Delete%20Seed%20IPs%20to%20Discovery%20Task%20API.md)).

***3c. Calling get_all_seed_Ips_from_discovery***
>Call this API to get ip addresses of all devices in one discovery task. In current step, we totally call this API twice. First time is after we input a devices list to add seed Ips to discovery task, the reason is to confirm we put in all device Ips successfully into the task. Second time is after we calling the delete seeds Ips from discovery, also to confirm the final devices list is acurracy (if users didn't call the delete API then there is no need to call this API twice). (more details: [Github_get_all_seed_Ips_from_discovery](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Discovery%20Task%20Management/Get%20All%20Seed%20IPs%20from%20Discovery%20Task%20API.md)).



```python
add_seed_Ips_to_discovery_url = nb_url + "/ServicesAPI/API//V1/CMDB/Discovery/Tasks/"+str(taskId)+"/Seeds"

mgmIP1 = "10.1.13.2"
mgmIP2 = "123.1.1.1"
mgmIP3 = "10.1.14.2"
mgmIP4 = "123.203.3.3"
mgmIP5 = "123.204.4.4"
mgmIP6 = "123.20.1.3"

body = {
    "seeds" : 
        [
            {"mgmtIP": mgmIP1},
            {"mgmtIP": mgmIP2},
            {"mgmtIP": mgmIP3},
            {"mgmtIP": mgmIP4},
            {"mgmtIP": mgmIP5},
            {"mgmtIP": mgmIP6}
        ]
    }
 
def add_seed_Ips_to_discovery(add_seed_Ips_to_discovery_url, body, headers, token):
    try:
        # Do the HTTP request
        response = requests.post(add_seed_Ips_to_discovery_url, data = json.dumps(body), headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()
            return (result)
        else:
            return("IP Add Failed - " + str(response.text))

    except Exception as e:
        return (str(e)) 
        
result = add_seed_Ips_to_discovery(add_seed_Ips_to_discovery_url, body, headers, token)
result
```


API Response: 

    {'statusCode': 790200, 'statusDescription': 'Success.'}




```python
get_all_seed_Ips_from_discovery_url =  nb_url + "/ServicesAPI/API//V1/CMDB/Discovery/Tasks/"+str(taskId)+"/Seeds"

def get_all_seed_Ips_from_discovery(get_all_seed_Ips_from_discovery_url, headers, token):
    try:
        # Do the HTTP request
        response = requests.get(get_all_seed_Ips_from_discovery_url, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()["ips"]
            return (result)
        else:
            return ("Get IPs from discovery task failed - " + str(response.text))

    except Exception as e:
        return (str(e)) 
    
result = get_all_seed_Ips_from_discovery(get_all_seed_Ips_from_discovery_url, headers, token)
result 
```

API Response: 


    ['10.1.13.2',
     '10.1.14.2',
     '123.1.1.1',
     '123.20.1.3',
     '123.203.3.3',
     '123.204.4.4']




```python
delete_seed_Ips_to_discovery_url = nb_url + "/ServicesAPI/API//V1/CMDB/Discovery/Tasks/"+str(taskId)+"/Seeds"

mgmIP1 = "10.1.13.2"


body1 = {
    "seeds" : 
        [
            {"mgmtIP": mgmIP1}
            #{"mgmtIP": mgmIP2},
           # {"mgmtIP": mgmIP3}
        ]
    }
 
def delete_seed_Ips_to_discovery(delete_seed_Ips_to_discovery_url, headers, token, body1):
    headers["Token"]=token
    try:
        # Do the HTTP request
        response = requests.delete(delete_seed_Ips_to_discovery_url, data = json.dumps(body1), headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()
            return (result)
        else:
            return("IP Add Failed - " + str(response.text))

    except Exception as e:
        return (str(e)) 
result = delete_seed_Ips_to_discovery(delete_seed_Ips_to_discovery_url, headers, token, body1)
result
```

API Response: 


    {'statusCode': 790200, 'statusDescription': 'Success.'}




```python
result = get_all_seed_Ips_from_discovery(get_all_seed_Ips_from_discovery_url, headers, token)
result 
```

API Response: 


    ['10.1.13.2',
     '10.1.14.2',
     '123.1.1.1',
     '123.20.1.3',
     '123.203.3.3',
     '123.204.4.4']



## Step 4: Run the task and check the running result.
In this step, we are going to run this task to modify our system conditions. 
> **Note:** users should call get_discovery_task_status API first before calling the get_discovery_tasks_result API to confirm wehther the task is finish running. 
 
***4a. Calling run_discovery_task_now API***<br>
>Call this API to run a scheduled discovery task right away. Error would return if the task is already running. (more details: [Github_run_discovery_task_now](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Discovery%20Task%20Management/Run%20Discovery%20Task%20Now%20API.md)).

***4b. Calling get_discovery_tasks_result API***<br>
>Call this API to get the running result of specified ip addresses for a discovery task of the latest run. Note that users cannot put too many ip addresses in the query parameter, which will make the URLs over 2,000 characters and it will not work in some web browsers.(more details: [Github_get_discovery_tasks_result](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Discovery%20Task%20Management/Get%20Discovery%20Tasks%20Result%20API.md)).

***4c. Calling get_discovery_live_access API***<br>
>Call this API to get live access log of all discovered devices (successfully or failed) for a discovery task.(more details: [Github_get_discovery_live_access](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Discovery%20Task%20Management/Get%20Discovery%20Task%20Live%20Access%20Log%20API.md)).



```python
run_discovery_task_now_url = nb_url + "/ServicesAPI/API//V1/CMDB/Discovery/Tasks/"+str(taskId)+"/Run"

def run_discovery_task_now(run_discovery_task_now_url, headers, token):
    headers["Token"]=token
    try:
        # Do the HTTP request
        response = requests.post(run_discovery_task_now_url, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()
            return (result)
        else:
            return ("Run Discovery Task failed - " + str(response.text))

    except Exception as e:
        return (str(e)) 
    
result = run_discovery_task_now(run_discovery_task_now_url, headers, token)
result
```

API Response: 


    {'statusCode': 790200, 'statusDescription': 'Success.'}




```python
get_discovery_tasks_result_url = nb_url + "/ServicesAPI/API/V1/CMDB/Discovery/Tasks/"+str(taskId)+"/Results"

def get_discovery_tasks_result(get_discovery_tasks_result_url, headers, token):
    headers["Token"]=token
    try:
        # Do the HTTP request
        #response = requests.get(full_url, headers=headers, params = data, verify=False)
        response = requests.get(get_discovery_tasks_result_url, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()
            return (result)
        else:
            return("Get discovery results failed - " + str(response.text))

    except Exception as e:
        return (str(e)) 
    
result = get_discovery_tasks_result(get_discovery_tasks_result_url, headers, token)
result
```

API Response: 


    {'devices': [{'mgmtIP': '123.204.4.4',
       'source': 'Scan 123.204.4.4/32',
       'hostname': 'SW4',
       'frontServerOrGroupId': 'NetBrainServer',
       'ping': 'Succeeded',
       'SNMP': 'nb',
       'vendor': 'Cisco',
       'oid': '1.3.6.1.4.1.9.1.1227',
       'type': 'Cisco IOS Switch',
       'config': 'Succeeded',
       'telnetSSH': 'Succeeded'},
      {'mgmtIP': '123.20.1.3',
       'source': 'Scan 123.20.1.3/32',
       'hostname': 'SW5',
       'frontServerOrGroupId': 'NetBrainServer',
       'ping': 'Succeeded',
       'SNMP': 'nb',
       'vendor': 'Cisco',
       'oid': '1.3.6.1.4.1.9.1.1227',
       'type': 'Cisco IOS Switch',
       'config': 'Succeeded',
       'telnetSSH': 'Succeeded'},
      {'mgmtIP': '123.1.1.1',
       'source': 'Scan 123.1.1.1/32',
       'hostname': 'R1',
       'frontServerOrGroupId': 'NetBrainServer',
       'ping': 'Succeeded',
       'SNMP': 'nb',
       'vendor': 'Cisco',
       'oid': '1.3.6.1.4.1.9.1.1',
       'type': 'Cisco Router',
       'config': 'Succeeded',
       'telnetSSH': 'Succeeded'},
      {'mgmtIP': '123.203.3.3',
       'source': 'Scan 123.203.3.3/32',
       'hostname': 'SW3',
       'frontServerOrGroupId': 'NetBrainServer',
       'ping': 'Succeeded',
       'SNMP': 'nb',
       'vendor': 'Cisco',
       'oid': '1.3.6.1.4.1.9.1.1227',
       'type': 'Cisco IOS Switch',
       'config': 'Succeeded',
       'telnetSSH': 'Succeeded'}],
     'statusCode': 790200,
     'statusDescription': 'Success.'}




```python
status = get_discovery_task_status(get_discovery_task_status_url, headers, token)
status
```

API Response: 


    'Task Status: Succeeded with warnings'




```python
get_discovery_live_access_log_url =  nb_url + "/ServicesAPI/API/V1/CMDB/Discovery/Tasks/"+str(taskId)+"/LiveAccessLog"

def get_discovery_live_access_log(get_discovery_live_access_log_url, headers, token):
    headers["Token"]=token
    try:
        # Do the HTTP request
        response = requests.get(get_discovery_live_access_log_url, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()
            return (result)
        else:
            return ("Get Discovery Live Access Log failed - " + str(response.text))

    except Exception as e:
        return (str(e)) 
    
result = get_discovery_live_access_log(get_discovery_live_access_log_url, headers, token)
result
```

API Response: 


    {'liveLogs': [{'mgmtIP': '123.204.4.4',
       'liveLog': '16:19:57 Ping [123.204.4.4] via Front Server (NetBrainServer); Succeeded\r\n16:19:57 Send RO = [nb][version:v2c] to [123.204.4.4] via Front Server (NetBrainServer); Succeeded\r\n16:19:57 Retrieving [123.204.4.4]\'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n16:19:57 Telnet to device 123.204.4.4 via Front Server (NetBrainServer)\n16:19:57 Telnet to device 123.204.4.4 successfully via Front Server (NetBrainServer)\n16:19:57 Return from Device:[Username:]\n16:19:57 Sending Username:nb\n16:19:57 Return from Device:[Password:]\n16:19:57 Sending Password:******\n16:19:59 Return from Device:[SW4#]\n16:19:59 Sending "enable" command\n16:19:59 Return from Device:[SW4#]\n16:19:59 Sending "terminal length 0" command\n16:19:59 Return from Device:[SW4#]\n16:19:59 Sending "show run" command\n16:19:59 Received:SW4#show run\r\nBuilding configuration...\r\n\r\r\n\r\n16:19:59 Sending "exit" command\n16:20:00 Telnet to device 123.204.4.4 disconnected.\n16:20:00 Update configuration file of SW4 successfully,(0.11s)\r\n16:20:00 Discovery of 123.204.4.4 complete'},
      {'mgmtIP': '123.20.1.3',
       'liveLog': '16:19:18 Ping [123.20.1.3] via Front Server (NetBrainServer); Succeeded\r\n16:19:18 Send RO = [nb][version:v2c] to [123.20.1.3] via Front Server (NetBrainServer); Succeeded\r\n16:19:19 Retrieving [123.20.1.3]\'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n16:19:19 Telnet to device 123.20.1.3 via Front Server (NetBrainServer)\n16:19:19 Telnet to device 123.20.1.3 successfully via Front Server (NetBrainServer)\n16:19:19 Return from Device:[Username:]\n16:19:19 Sending Username:nb\n16:19:19 Return from Device:[Password:]\n16:19:19 Sending Password:******\n16:19:20 Return from Device:[SW5#]\n16:19:20 Sending "enable" command\n16:19:20 Return from Device:[SW5#]\n16:19:20 Sending "terminal length 0" command\n16:19:20 Return from Device:[SW5#]\n16:19:20 Sending "show run" command\n16:19:20 Received:SW5#show run\r\nBuilding configuration...\r\n\r\r\n\r\n16:19:20 Sending "exit" command\n16:19:21 Telnet to device 123.20.1.3 disconnected.\n16:19:21 Update configuration file of SW5 successfully,(0.11s)\r\n16:19:21 Discovery of 123.20.1.3 complete'},
      {'mgmtIP': '123.1.1.1',
       'liveLog': '16:19:16 Ping [123.10.1.2] via Front Server (NetBrainServer); Succeeded\r\n16:19:16 Send RO = [nb][version:v2c] to [123.10.1.2] via Front Server (NetBrainServer); Succeeded\r\n16:19:16 Retrieving [123.10.1.2]\'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n16:19:16 Telnet to device 123.10.1.2 via Front Server (NetBrainServer)\n16:19:16 Telnet to device 123.10.1.2 successfully via Front Server (NetBrainServer)\n16:19:16 Return from Device:[Username:]\n16:19:16 Sending Username:nb\n16:19:16 Return from Device:[Password:]\n16:19:16 Sending Password:******\n16:19:18 Return from Device:[R1#]\n16:19:18 Sending "enable" command\n16:19:18 Return from Device:[R1#]\n16:19:18 Sending "terminal length 0" command\n16:19:18 Return from Device:[R1#]\n16:19:18 Sending "show run" command\n16:19:18 Received:R1#show run\r\nBuilding configuration...\r\n\r\r\n\r\n16:19:18 Sending "exit" command\n16:19:19 Telnet to device 123.10.1.2 disconnected.\n16:19:19 Update configuration file of R1 successfully,(0.344s)\r\n16:19:19 Discovery of 123.1.1.1 complete'},
      {'mgmtIP': '123.203.3.3',
       'liveLog': '16:19:18 Ping [123.203.3.3] via Front Server (NetBrainServer); Succeeded\r\n16:19:18 Send RO = [nb][version:v2c] to [123.203.3.3] via Front Server (NetBrainServer); Succeeded\r\n16:19:18 Retrieving [123.203.3.3]\'s Hostname ,Vendor and Model via Front Server (NetBrainServer); Succeeded\r\n16:19:19 Telnet to device 123.203.3.3 via Front Server (NetBrainServer)\n16:19:19 Telnet to device 123.203.3.3 successfully via Front Server (NetBrainServer)\n16:19:19 Return from Device:[Username:]\n16:19:19 Sending Username:nb\n16:19:19 Return from Device:[Password:]\n16:19:19 Sending Password:******\n16:19:20 Return from Device:[SW3#]\n16:19:20 Sending "enable" command\n16:19:20 Return from Device:[SW3#]\n16:19:20 Sending "terminal length 0" command\n16:19:20 Return from Device:[SW3#]\n16:19:20 Sending "show run" command\n16:19:20 Received:SW3#show run\r\nBuilding configuration...\r\n\r\r\n\r\n16:19:20 Sending "exit" command\n16:19:20 Telnet to device 123.203.3.3 disconnected.\n16:19:21 Update configuration file of SW3 successfully,(0.14s)\r\n16:19:21 Discovery of 123.203.3.3 complete'}],
     'statusCode': 790200,
     'statusDescription': 'Success.'}



### Logout Netbrain System by calling logout API


```python
Logout_url = nb_url + "/ServicesAPI/API/V1/Session"

def logout(Logout_url, token, headers):
    headers["token"] = token
    
    try:
        # Do the HTTP request
        response = requests.delete(Logout_url, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            js = response.json()
            return (js)
        else:
            return ("Session logout failed! - " + str(response.text))

    except Exception as e:
        return (str(e))

logout = logout(Logout_url, token, headers)
logout
```

API Response: 


    'Session logout failed! - {"statusCode":795010,"statusDescription":"Session not exist"}'
    # i have already log out before i calling this API, so it shows failed.




```python

```
