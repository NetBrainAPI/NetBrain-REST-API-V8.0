
# Return the devices path results


During current use case, the final goal is present the path result between two specified devices in current domain. There are totally 8 REST APIs we are going to concern to be a part of this use case, as shown in following:

1. Import python modules and global variables for sample code.
2. Call login API to get authentication token.
3. Call get_all_accessible_tenants API to get all accessible tenant IDs.
4. Call get_all_accessible_domains API to get all accessible domain IDs in specified tenant.
5. Call specify_a_working_domain API to specified which domain to work with.
6. Call resolve_device_gateway API to get devices gateway information. 
7. Call calculate_path API to get the task ID.
8. Call get_path_result API to get the result of calculation path.
9. Call logout API to log out from current account.

The sequencial of provided APIs is also the sequence of our workflow steps.

***Note***: if users want to find the path results of devices, then the step sequence must be followed. If users call these APIs with a different sequential then there would be no results or some errors would be occured.

## Step Explanation
***1. Import python modules and global variables for sample code***<br>
> Note: If users try to use this code. please remember to change the "nb_url" to users' own working url.

***2. Call login API to get authentication token***<br>
>We calling the login API with "username" and "password" as inputs in the first step. As response we can get the authentication token as one fixed input in following APIs calling. If users get errors when calling this API please check the API documentation on [Github_login](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Login%20API.md).

***3. Call get_all_accessible_tenants API to get all accessible tenant IDs***
>After we got the token from previous section, we need to use this token as a key to find all tenants which we have the access authentication. During this step, the most important feature is to get the tenant id of the corresponding tenant which we decide to work inside. After running this API successfully, we will get the tenantId of the willing tenant which will be set as another input for next step API calling. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_tenant](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Get%20All%20Accessible%20Tenants%20API.md) 

***4. Call get_all_accessible_domains API to get all accessible domain IDs in specified tenant***
>In this section, we are going to find all accessible domains in the corressponding tenant which we have got the tenantId from previous step. Similar with step 2, during current API call, we have to decide which domain we are going to work inside and get the domainId at meanwhile to prepare for next API calling. If users get errors when calling this API please check the API documentation on [Github_domain](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Get%20All%20Accessible%20Domains%20API.md) 

***5. Call specify_a_working_domain API to specified which domain to work with***<br>
>After we running this step successfully, we directly complete the full login processes which means we totally join in Netbrain System by calling APIs(because we have record our tenantId and domainId，if users don't know the ID of corresponding tenant and domain please fully follow step 1 to step 4). Next step, we will start to use Netbrain functions formally. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_working_domain](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Specify%20A%20Working%20Domain%20API.md).

***6. Call resolve_device_gateway API to get source device gateway information***
>Becasue we have specified the Ip address of source device and destination device at beginning, we can calling resolve devices gateway API at here. Mention again, if users want to get path result by calling APIs then users must follow the sequencial from Step 6 to Step 8. After the API running successfully, we will get gateway information of two specyfied devices which is the required input for next section. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_Gateway](https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/Path%20API%20Design/STANDARD_formate_Resolve_Device_Gateway_API_Test.ipynb)

***7. Call calculate_path API to get the task ID***
>During this section, we are going to calling the Calculate Path API and set the gateway information as one input(other inputs are shown in following sample code). When calling this API, users must input the required parameters correctly and follow the format of each inputs examples([Github_calPath](https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/Path%20API%20Design/STANDARD_formate_Calculate_Path_API_Test.ipynb)). After calling this API successfully, we will get the taskId of calculated path information. And the taskId is the only one required input for next section. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_calPath](https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/Path%20API%20Design/STANDARD_formate_Calculate_Path_API_Test.ipynb).

***8. Call get_path_result API to get the result of calculation path***
>So far we attemp to the final functional step of this use case: to get the calculation result of the task which we have got the taskId in Step 7. After we running the following sample code successfully, we will finally get the path result in a json file. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_pathRes](https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/Path%20API%20Design/STANDARD_formate_Get_Path_Calculation_Result_API_Test.ipynb). 

***9. Call logout API to log out from current account***
>After we got all informations from this case, we have to logout from the Netbrain System.
If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_logout](https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/STANDARD_formate_Logout_Test1%20.ipynb). 

## Sample Code

### import python modules, global variables 


```python
# import python modules
import requests
import json
import time

# Global Variables
nb_url = "http://192.168.28.79"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'} 
TenantName = "Initial Tenant"
DomainName = "Netbrain REST API Lab"
username = "netbrain"
password = "netbrain"
source_device_Ip = "123.10.1.22"
destination_device_Ip = "123.10.1.10"
```

### Define calling Functions



```python
# call login API
def login(nb_url, username, password, headers):
    login_URL = nb_url + "/ServicesAPI/API/V1/Session"
    data = {
        "username" : username,      
        "password" : password  
    }
    try:
        # Do the HTTP request
        response = requests.post(login_URL, headers=headers, data = json.dumps(data), verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            js = response.json()
            return (js)
        else:
            return ("Get token failed! - " + str(response.text))
    except Exception as e: return (str(e))

# call get_all_accessible_tenants API
def get_all_accessible_tenants(nb_url, token, headers):
    Accessible_tenants_url = nb_url + "/ServicesAPI/API/V1/CMDB/Tenants"
    headers["Token"] = token
    try:
        # Do the HTTP request
        response = requests.get(Accessible_tenants_url, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()
            tenants = result["tenants"]   
            return tenants
        else:
            return ("Get tenants failed! - " + str(response.text))
    except Exception as e: return (e)
    
# call get_all_accessible_domains API
def get_all_accessible_domains(nb_url, tenantId, token, headers):
    Accessible_domains_url = nb_url + "/ServicesAPI/API/V1/CMDB/Domains"
    headers["Token"] = token
    data = {"tenantId": tenantId}
    try:
        # Do the HTTP request
        response = requests.get(Accessible_domains_url, params = data, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()
            domains = result["domains"]
            return domains
        else:
            return ("Get domains failed! - " + str(response.text))
    except Exception as e: print (str(e))
        
# call specify_a_working_domain API
def specify_a_working_domain(tenantId, domainId, nb_url, headers, token):
    Specify_a_working_domain_url = nb_url + "/ServicesAPI/API/V1/Session/CurrentDomain"
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
            return (domainId)
        else:
            return ("Login failed! - " + str(response.text))
    except Exception as e: print (str(e))

# call resolve_device_gateway API
def resolve_device_gateway(nb_url, token, ipOrHost, headers):
    Resolve_Device_Gateway_url = nb_url + "/ServicesAPI/API/V1/CMDB/Path/Gateways"
    headers["Token"] = token
    data = {"ipOrHost":ipOrHost}
    try:
        response = requests.get(Resolve_Device_Gateway_url, params = data, headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            return (result)
        else:
            return ("Create module attribute failed! - " + str(response.text))
    except Exception as e: print (str(e))

        
# call calculate_path API
def calculate_path(nb_url, headers, token, gw, destination_device_Ip):
    Calculate_Path_url = nb_url + "/ServicesAPI/API/V1/CMDB/Path/Calculation"
    headers["Token"] = token

    sourceIP = gw["ip"]
    sourcePort = 0 #can be 8080
    sourceGwIP = gw["ip"]
    sourceGwDev = gw["devName"]
    sourceGwIntf = gw["intfName"]
    destIP = destination_device_Ip
    destPort = 0
    pathAnalysisSet = 2
    protocol = 4
    isLive = False

    body = {
                "sourceIP" : sourceIP,                # IP address of the source device.
                "sourcePort" : sourcePort,
                "sourceGwDev" : sourceGwDev,          # Hostname of the gateway device.
                "sourceGwIP" : sourceGwIP,            # Ip address of the gateway device.
                "sourceGwIntf" : sourceGwIntf,        # Name of the gateway interface.
                "destIP" : destIP,                    # IP address of the destination device.
                "destPort" : destPort,
                "pathAnalysisSet" : pathAnalysisSet,  # 1:L3 Path; 2:L2 Path; 3:L3 Active Path
                "protocol" : protocol,                # Specify the application protocol, check online help, such as 4 for IPv4.
                "isLive" : isLive                     # False: Current Baseline; True: Live access
            } 

    try:
        response = requests.post(Calculate_Path_url, data = json.dumps(body), headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            return (result["taskID"])
        else:
            return ("Create module attribute failed! - " + str(response.text))
    except Exception as e: return (str(e)) 

# call get_path_result API
def get_path_result(nb_url, headers, token, res):
    Get_Path_Calulation_Result_url = nb_url + "/ServicesAPI/API/V1/CMDB/Path/Calculation/" + str(res) + "/Result"
    headers["Token"] = token
    try:
        response = requests.get(Get_Path_Calulation_Result_url, headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            return (result)
        else:
            return (str(response))
    except Exception as e: return (str(e)) 

# call logout API
def logout(nb_url, token, headers):
    Logout_url = nb_url + "/ServicesAPI/API/V1/Session"
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
    except Exception as e: return (str(e))

```

### Define main function


```python
def main(nb_url, headers, TenantName, DomainName, username, password, source_device_Ip, destination_device_Ip):
    # Calling login API
    print("Calling login API")
    result = login(nb_url, username, password, headers)
    print(result) # print out the authentication token.
    token = result["token"]
    print(token)
    print("")
    
    # Calling get accessible tenant API
    print("Calling get accessible tenant API")
    tenants = get_all_accessible_tenants(nb_url, token, headers)
    tenant =  [x for x in tenants if x["tenantName"] == TenantName] # Name of the tenant which we are going to work inside
    tenantId = tenant[0]["tenantId"]
    print("Tenant ID : " + tenantId) # print out the specified tenant id.
    print("")

    # Calling get accessible domain API
    print("Calling get accessible domain API")
    domains = get_all_accessible_domains(nb_url, tenantId, token, headers)
    domain =  [x for x in domains if x["domainName"] == DomainName]# Name of the domain which we are going to work inside
    domainId = domain[0]["domainId"]
    print("Domain ID : " + domainId) # Print out the specified domain Id.
    print("")

    # Calling specify domain API
    print("Calling specify domain API")
    res =  specify_a_working_domain(tenantId, domainId, nb_url, headers, token)
    print ("Domain ID of Specified Working Domain : " + res)
    print("")

    # Calling resolve device gateway API
    print("Calling resolve device gateway API")
    source_gateway = resolve_device_gateway(nb_url, token, source_device_Ip, headers)
    gw = source_gateway["gatewayList"][0]
    print(source_gateway)
    print(gw)
    
    # Calling calculate path API
    print("Calling calculate path API")
    res = calculate_path(nb_url, headers, token, gw, destination_device_Ip)
    print("Task ID : " + res)
    print("")
    
    # Calling get path result API
    print("Calling get path result API")
    while True:
        final = get_path_result(nb_url, headers, token, res)
        if final != "<Response [400]>":
            print(final)
            break
        time.sleep(1)   # Delays for 1 seconds. You can also use a float value.
    print("")
            
    # Calling logout API
    print("Calling logout API")
    Logout = logout(nb_url, token, headers)
    print(Logout)
    
main(nb_url, headers, TenantName, DomainName, username, password, source_device, destination_device)
```
### Response from API callings

    Calling login API
    {'token': 'a343eeab-1997-42d9-8064-3b7c466f547a', 'statusCode': 790200, 'statusDescription': 'Success.'}
    a343eeab-1997-42d9-8064-3b7c466f547a
    
    Calling get accessible tenant API
    Tenant ID : fb24f3f0-81a7-1929-4b8f-99106c23fa5b
    
    Calling get accessible domain API
    Domain ID : 850ff5e9-c639-404d-85a3-d920dbee509c
    
    Calling specify domain API
    Domain ID of Specified Working Domain : 850ff5e9-c639-404d-85a3-d920dbee509c
    
    Calling resolve device gateway API
    {'gatewayList': [{'ip': '123.10.1.22', 'devName': 'R6', 'intfName': 'Ethernet0/2'}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    {'ip': '123.10.1.22', 'devName': 'R6', 'intfName': 'Ethernet0/2'}
    
    Calling calculate path API
    Task ID : 1e5c130e-f86c-46b5-bd80-fd7618ce93a5
    
    Calling get path result API
    {'hopList': [{'hopId': 'a1480dde-8b74-4003-ba7d-ce1e755d5d56', 'srcDeviceName': 'R6', 'inboundInterface': 'Ethernet0/2', 'mediaName': '123.10.1.20/30', 'dstDeviceName': 'R4', 'outboundInterface': 'Ethernet0/2', 'nextHopIdList': ['49b25629-cf66-4182-b896-b31efad87c07']}, {'hopId': '49b25629-cf66-4182-b896-b31efad87c07', 'srcDeviceName': 'R4', 'inboundInterface': 'Ethernet0/0', 'mediaName': '123.10.1.16/30', 'dstDeviceName': 'R2', 'outboundInterface': 'Ethernet0/2', 'nextHopIdList': ['4466a0b8-a6aa-4ab5-825f-104c37923919']}, {'hopId': '4466a0b8-a6aa-4ab5-825f-104c37923919', 'srcDeviceName': 'R2', 'inboundInterface': 'Ethernet0/1', 'mediaName': '123.10.1.8/30', 'dstDeviceName': 'R3', 'outboundInterface': 'Ethernet0/1', 'nextHopIdList': []}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    
    Calling logout API
    {'statusCode': 790200, 'statusDescription': 'Success.'}
    


```python

```
