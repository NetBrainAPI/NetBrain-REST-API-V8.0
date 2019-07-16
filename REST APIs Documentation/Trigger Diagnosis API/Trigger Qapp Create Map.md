

## Trigger Qapp Create Map

### Input body parameters

| Name | Type | Description |
|---|---|---|
| map_setting | object | map setting. |
| map_setting.map_create_mode | int | 7: qapp generate a map. |
| map_setting.map_qapp_para | object | map qapp parameter object. |
| map_setting.map_qapp_para.device | string | device name. |
| map_setting.map_qapp_para.thresholdParamters | array | threshold parameters. |
| map_setting.map_qapp_para.thresholdParamters.name | string | threshold parameter name. |
| map_setting.map_qapp_para.thresholdParamters.value | string | threshold value. |
| map_setting.map_qapp_para.thresholdParamters.values | array | an array combined by threshold values. |
| map_setting.map_qapp_para.inputVariableParameters | array | qapp input variable parameters. |
| map_setting.map_qapp_para.inputVariableParameters.name | string | input variable name. |
| map_setting.map_qapp_para.inputVariableParameters.value | string | input variable value. |
| map_setting.map_qapp_para.dataSource.type | int | 1: live<br> 2: current baseline. |
| map_setting.map_qapp_para.frequency | object | run frequency setting. |
| map_setting.frequency.type | int | frequency type:<br> 1: run once.<br> 2: times. |
| map_setting.map_qapp_para.frequency.times | int | times. |
| map_setting.map_qapp_para.frequency.interval | object | cycle interval. |
| map_setting.map_qapp_para.frequency.interval.unit | int | time unit:<br> 1: hour.<br> 2:min.<br> 3: sec. |
| map_setting.map_qapp_para.frequency.interval.duration | float | duration value. |

***Example***


```python
task_parameter = {
    'basic_setting': {
        'user': 'admin',
        'device': 'BJ*POP',
        'stub_name': 'qapp-create-map-stub',
        'triggered_by': user
    },
    'map_setting': {
        'map_create_mode': 7,
        'map_qapp_para': {
            'thresholdParamters': [
                {
                    'name': 'Input Errors Occurred',
                    'value': '800'
                }
            ],
            'inputVariableParameters':[
                    {
                        'name':'VLAN',
                        'value':'100'
                    }
            ]         
        }
    }
}
```

## Full Example:

***Step1: Create a stub for Qapp trigger map***<br>
Please go to your **System Automation Task Manager**, click the tag **API Stub Manager** then select "Add Stub":<br>
<img src="trigger by Qapp\system_automation_manager.png" /><br>
<img src="trigger by Qapp\add_new_stub.png" /><br>

***Step2: Modify the just created stub***<br>
1) Give a reasonable name for your stub.<br>
2) Select the "Use Qapp to Create a Map" in the draw down list.<br>
>>a) After you select "Use Qapp to Create a Map", there would be a new dialog jump up then select the Qapp wich you are going to trigger map.<br>
>> <img src="trigger by Qapp\select_Qapp_to_trigger_map.png" /><br>

3) Never forget to save the stub.<br>
<img src="trigger by Qapp\modify_stub.png" /><br>

***Step3: Use the following python script to calling API***<br>


```python
# import python modules
import requests
import json
import time
import urllib3
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import pprint

# Global Variables
nb_url = "https://ie80.netbraintech.com"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'} 
TenantName = "Initial Tenant"
DomainName = "SNOW_Integration"
username = "APITestGL"
password = "Test123"
source_device = "10.10.7.253"
destination_device = "172.24.101.2"
```


```python
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

    e21c2a14-786c-4031-8a7d-5f17e6285dd9
    


```python
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

    1ebcb6e7-b3de-0983-2048-ae12a2219b56
    


```python
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

    2140d612-a261-4c19-a62e-08579d5c73a3
    


```python
API_BODY = {
            'domain_setting':
            {
                'tenant_id': tenantId,
                'domain_id': domainId
            },
            'basic_setting':
            {
                'user': 'APITestGL',
                'device': 'GW2Lab',
                'stub_name': 'stub1',
                'triggered_by':"Gongdailiu",
           }, 
           'map_setting':{
               'map_create_mode':7,
                'map_qapp_para': {
                    'device':'GW2Lab',
                    'dataSource':{
                        'type':1
                    },
                    'frequency':{
                        'type':1
                    },
                    'thresholdParamters': [
                        {
                            'name': 'Alert1',
                            'value': 'test'
                        },
                    ],
                    'input_variable_parameters':[
                        {
                            'name':'interface',
                            'value':'FastEthernet1/0/2'
                        },
                    ]
                }
           }
}

def TriggerTask(API_BODY):

    # Trigger  API url
    API_URL = r"/ServicesAPI/API/V1/Triggers/Run"
    # Trigger API payload
    print(headers)
    api_full_url = nb_url + API_URL
    api_result = requests.post(api_full_url, data=json.dumps(API_BODY), headers=headers, verify=False)
    if api_result.status_code == 200:
        return api_result.json()
    else:
        return api_result.json()
    
trigger_res = TriggerTask(API_BODY)
trigger_res
```

    {'Content-Type': 'application/json', 'Accept': 'application/json', 'Token': 'e21c2a14-786c-4031-8a7d-5f17e6285dd9'}
    




    {'mapId': '4d78c778-0e96-4a6d-9bf4-1fd1533ce046',
     'mapName': 'stub1-20190716141758',
     'mapType': 1,
     'mapUrl': 'map.html?t=1ebcb6e7-b3de-0983-2048-ae12a2219b56&d=2140d612-a261-4c19-a62e-08579d5c73a3&id=4d78c778-0e96-4a6d-9bf4-1fd1533ce046&maptype=1&rba=713b2263-48a0-4948-bb94-13e2a46cebeb',
     'taskId': 'ffaef497-6f2f-434a-8270-1af08826468f'}




```python
fullURL = str(nb_url + "/" + trigger_res["mapUrl"])
fullURL
```




    'https://ie80.netbraintech.com/map.html?t=1ebcb6e7-b3de-0983-2048-ae12a2219b56&d=2140d612-a261-4c19-a62e-08579d5c73a3&id=4d78c778-0e96-4a6d-9bf4-1fd1533ce046&maptype=1&rba=713b2263-48a0-4948-bb94-13e2a46cebeb'




```python
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

    except Exception as e:
        return (str(e))

res = logout(nb_url, token, headers)
res
```




    {'statusCode': 790200, 'statusDescription': 'Success.'}



**Then open the link you have got from the In[6] you will get the result:**
<img src="trigger by Qapp\result.png" /><br>


```python

```
