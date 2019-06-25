
# Return the devices path results
During current use case, the final goals are present the One-Ip Table of devices in current domain and path result between two random devices in current domain.
There are totally 10 REST APIs we going to concerned to be a part of this use case, as shown in following:<br> 

**[Step 1: Use case preparation](Step-1:-Use-case-preparation)**
>> 1a. import all useful modules and create global variables<br>
>> 1b. call login API<br>
>> 1c. call get_all_accessible_tenants API<br>
>> 1d. call get_all_accessible_domains API<br>
>> 1e. call specify_a_working_domain API<br>

**[Step 2: Get the devices path results](Step-2:-Get-the-devices-path-results)**
>> 2a. call resolve_device_gateway API<br>
>> 2b. call calculate_path API<br>
>> 2c. call get_path_result API<br>
>> 2d. call logout API

The sequencial of provided APIs is also the sequence of our workflow steps.<br>

***Note:*** if users want to find the path results of devices, then the step sequence must be followed in Step 2. If users call these APIs with a different sequential then there would be no results or some errors would be occured. 

## Step 1: Use case preparation.
***1a. import all useful modules and create global variables.***<br>
> Note: If users try to use this code. please remember to change the "nb_url" to users' own working url.

***1b. call login API.***<br>
>Same with use case 2, we calling the login API with "username" and "password" as inputs in the first step. As response we can get the authentication token as one fixed input in following APIs calling. If users get errors when calling this API please check the API documentation on [Github_login](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Login%20API.md).

***1c. call get_all_accessible_tenants API***
>After we got the token from previous section, we need to use this token as a key to find all tenants which we have the access authentication. During this step, the most important feature is to get the tenant id of the corresponding tenant which we decide to work inside. After running this API successfully, we will get the tenantId of the willing tenant which will be set as another input for next step API calling. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_tenant](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Get%20All%20Accessible%20Tenants%20API.md) 

***1d. call get_all_accessible_domains API.***
>In this section, we are going to find all accessible domains in the corressponding tenant which we have got the tenantId from previous step. Similar with step 2, during current API call, we have to decide which domain we are going to work inside and get the domainId at meanwhile to prepare for next API calling. If users get errors when calling this API please check the API documentation on [Github_domain](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Get%20All%20Accessible%20Domains%20API.md) 

***1e. call specify_a_working_domain API.***<br>
>After we running this step successfully, we directly complete the full login processes which means we totally join in Netbrain System by calling APIs(because we have record our tenantId and domainId，if users don't know the ID of corresponding tenant and domain please fully follow step 1 to step 4 in use case 1). Next step, we will start to use Netbrain functions formally. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_working_domain](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Specify%20A%20Working%20Domain%20API.md).



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
source_device = "R4"
destination_device = "R5"
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

API response:    

    cc25cbea-d244-494d-b6f9-32937bdadc69
    


```python
# call get_all_accessible_tenants API

Accessible_tenants_url = nb_url + "/ServicesAPI/API/V1/CMDB/Tenants"

def get_all_accessible_tenants(Accessible_tenants_url, token, headers):
    headers["Token"] = token
    try:
        # Do the HTTP request
        response = requests.get(Accessible_tenants_url, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()
            tenants = result["tenants"]
            tenant =  [x for x in tenants if x["tenantName"] == TenantName] # Name of the tenant which we are going to work inside
            #tj = tenant.json()
            tenantId = tenant[0]["tenantId"]
            #ten_j = json.dumps(tenant)
            return tenantId
        else:
            return ("Get tenants failed! - " + str(response.text))
    except Exception as e: return (e)

tenantId = get_all_accessible_tenants(Accessible_tenants_url, token, headers)
print(tenantId) # print out the specified tenant id.
```

API response:    

    fb24f3f0-81a7-1929-4b8f-99106c23fa5b
    


```python
# call get_all_accessible_domains API

Accessible_domains_url = nb_url + "/ServicesAPI/API/V1/CMDB/Domains"

def get_all_accessible_domains(tenantId, token, headers):
    headers["Token"] = token
    data = {"tenantId": tenantId}
    try:
        # Do the HTTP request
        response = requests.get(Accessible_domains_url, params=data, headers=headers, verify=False)
        # Check for HTTP codes other than 200
        if response.status_code == 200:
            # Decode the JSON response into a dictionary and use the data
            result = response.json()
            domains = result["domains"]
            domain =  [x for x in domains if x["domainName"] == DomainName]# Name of the domain which we are going to work inside
            domainId = domain[0]["domainId"]
            return domainId
        else:
            return ("Get domains failed! - " + str(response.text))

    except Exception as e: print (str(e))

domainId = get_all_accessible_domains(tenantId, token, headers)
print(domainId) # Print out the specified domain Id.
```

API response:     

    850ff5e9-c639-404d-85a3-d920dbee509c
    


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
            return (domainId)
            
        elif response.status_code != 200:
            return ("Login failed! - " + str(response.text))

    except Exception as e: print (str(e))

res =  specify_a_working_domain(tenantId, domainId, Specify_a_working_domain_url, headers, token)
print (res)
```
API response:     

    850ff5e9-c639-404d-85a3-d920dbee509c
    

## Step 2: Get the devices path results

***2a. call resolve_device_gateway API***
>Becasue we have specified the hostname of source device and destination device at beginning, we can calling resolve devices gateway API at here. Mention again, if users want to get path result by calling APIs then users must follow the sequencial of Step 2. In current section, we will input the Ips list we have got from previous section. After the API running successfully, we will get a gateway list with some device informations which is the required input for next section. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_Gateway](https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/Path%20API%20Design/STANDARD_formate_Resolve_Device_Gateway_API_Test.ipynb)

***2b. call calculate_path API***
>During this section, we are going to calling the Calculate Path API and set the gateway information list as one input(other inputs are shown in following code cell). When calling this API, users must input the required parameters correctly and follow the format of each inputs examples([Github_calPath](https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/Path%20API%20Design/STANDARD_formate_Calculate_Path_API_Test.ipynb)). After calling this API successfully, we will get the corresponding taskId of gateway information which has been put in. And the taskId is the only one required input for next section. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_calPath](https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/Path%20API%20Design/STANDARD_formate_Calculate_Path_API_Test.ipynb).

***2c. call get_path_result API***
>So far we attemp to the final functional step of this use case: to get the calculation result of the task which we have got the taskId in section 2c. After we running the following sample code successfully, we will finally get the path result in a json file. If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_pathRes](https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/Path%20API%20Design/STANDARD_formate_Get_Path_Calculation_Result_API_Test.ipynb). 

***2d. call logout API***
>After we got all informations from this case, we have to logout from the Netbrain System.
If users want to get more details about this API or get errors when calling this API please check the API documentation on [Github_logout](https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/STANDARD_formate_Logout_Test1%20.ipynb). 

```python
# call resolve_device_gateway API

Resolve_Device_Gateway_url = nb_url + "/ServicesAPI/API/V1/CMDB/Path/Gateways"

def resolve_device_gateway(Resolve_Device_Gateway_url, token, ipOrHost, headers):
    headers["Token"] = token
    data = {"ipOrHost":ipOrHost}
    try:
        response = requests.get(Resolve_Device_Gateway_url, params = data, headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            return (result)
        else:
            return ("Create module attribute failed! - " + str(response.text))

    except Exception as e:
        print (str(e))

source_gateway = resolve_device_gateway(Resolve_Device_Gateway_url, token, "R2", headers)
print(source_gateway)
destination_gateway = resolve_device_gateway(Resolve_Device_Gateway_url, token, "R3", headers)
print(destination_gateway)
```

API Response:    

    {'gatewayList': [{'ip': '123.10.1.17', 'devName': 'R4', 'intfName': 'Ethernet0/2'}], 'statusCode': 790200, 'statusDescription': 'Success.'}
    {'gatewayList': [{'ip': '123.10.1.10', 'devName': 'R5', 'intfName': 'Ethernet0/1'}], 'statusCode': 790200, 'statusDescription': 'Success.'}

```python
# call calculate_path API

Calculate_Path_url = nb_url + "/ServicesAPI/API/V1/CMDB/Path/Calculation"

def calculate_path(Calculate_Path_url, body, headers, token):
    headers["Token"] = token
    
    try:
        response = requests.post(Calculate_Path_url, data = json.dumps(body), headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            return (result["taskID"])
        else:
            return ("Create module attribute failed! - " + str(response.text))

    except Exception as e:
        return (str(e)) 

#for i in range(len(results))
gw = source_gateway["gatewayList"][0]
gw1 = destination_gateway["gatewayList"][0]

sourceIP = gw["ip"]
sourcePort = 0 #can be 8080
sourceGwIP = gw["ip"]
sourceGwDev = gw["devName"]
sourceGwIntf = gw["intfName"]
destIP = gw1["ip"]
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

res = calculate_path(Calculate_Path_url, body, headers, token)
res
```

API Response:    
    
    '459861bb-37ab-40e8-809c-4808071c7c7f'


```python
# call get_path_result API

import time
#time.sleep(30)   # Delays for 5 seconds. You can also use a float value.
def get_patth_result(Get_Path_Calulation_Result_url, headers, token):
    headers["Token"] = token
    try:
        response = requests.get(Get_Path_Calulation_Result_url, headers = headers, verify = False)
        if response.status_code == 200:
            result = response.json()
            return (result)
        else:
            return (response)

    except Exception as e:
        return (str(e)) 

Get_Path_Calulation_Result_url = nb_url + "/ServicesAPI/API/V1/CMDB/Path/Calculation/" + str(res) + "/Result"
final = get_patth_result(Get_Path_Calulation_Result_url, headers, token)
final

```



API Response:

    {
    "hopList": [
        {
            "hopId": "01383651-8bec-41fd-bea2-c3a673ac394c",
            "srcDeviceName": "R5",
            "inboundInterface": "Ethernet0/0",
            "mediaName": "123.10.1.12/30",
            "dstDeviceName": "R3",
            "outboundInterface": "Ethernet0/2",
            "nextHopIdList": [
                "5775d104-f2e7-409d-92f5-8e6b1ed9594a"
            ]
        },
        {
            "hopId": "5775d104-f2e7-409d-92f5-8e6b1ed9594a",
            "srcDeviceName": "R3",
            "inboundInterface": "Ethernet0/1",
            "mediaName": "123.10.1.8/30",
            "dstDeviceName": "R2",
            "outboundInterface": "Ethernet0/1",
            "nextHopIdList": [
                "3f28a2db-fd15-4f3c-b4ab-de7cc2d7cad9"
            ]
        },
        {
            "hopId": "3f28a2db-fd15-4f3c-b4ab-de7cc2d7cad9",
            "srcDeviceName": "R2",
            "inboundInterface": "Ethernet0/2",
            "mediaName": "123.10.1.16/30",
            "dstDeviceName": "R4",
            "outboundInterface": "Ethernet0/0",
            "nextHopIdList": []
        },
        {
            "hopId": "b4b4ddc7-02c5-41bd-b0d3-a7e72d3100e9",
            "srcDeviceName": "R5",
            "inboundInterface": "Ethernet0/1",
            "mediaName": "123.10.1.4/30",
            "dstDeviceName": "R1",
            "outboundInterface": "Ethernet0/1",
            "nextHopIdList": [
                "3e4b6c71-8e6e-4845-b6c3-acdca0ac7377"
            ]
        },
        {
            "hopId": "3e4b6c71-8e6e-4845-b6c3-acdca0ac7377",
            "srcDeviceName": "R1",
            "inboundInterface": "Ethernet0/2",
            "mediaName": "123.10.1.0/30",
            "dstDeviceName": "R4",
            "outboundInterface": "Ethernet0/1",
            "nextHopIdList": []
        }
    ],
    "statusCode": 790200,
    "statusDescription": "Success."
}


From the final result, we can see the task is success. And as a clarification, if users get the taskId from previous successfully, that is not means the path is exist during gateway pairs. 


```python
# call logout API

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

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    
    
## References:
> 1) login API:

>https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Login%20API.md<br> 

> 2) get_all_accessible_tenants API:

>https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Get%20All%20Accessible%20Tenants%20API.md<br>

> 3) get_all_accessible_domains API: 

>https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Get%20All%20Accessible%20Domains%20API.md<br>

> 4) specify_a_working_domain API: 

>https://github.com/Gongdai/REST_API_with_Markdown/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Specify%20A%20Working%20Domain%20API.md<br>

> 5) resolve_device_gateway API: 

>https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/Path%20API%20Design/STANDARD_formate_Resolve_Device_Gateway_API_Test.ipynb<br>

> 6) calculate_path API: 

>https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/Path%20API%20Design/STANDARD_formate_Calculate_Path_API_Test.ipynb<br>

> 7) get_patth_result API:

>https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/Path%20API%20Design/STANDARD_formate_Get_Path_Calculation_Result_API_Test.ipynb

> 8) logout API: 

>https://github.com/Gongdai/Netbrain_REST_API_First_Regularization/blob/master/Netbrain_REST_API/API_test/STANDARD_formate_Logout_Test1%20.ipynb


```python

```
