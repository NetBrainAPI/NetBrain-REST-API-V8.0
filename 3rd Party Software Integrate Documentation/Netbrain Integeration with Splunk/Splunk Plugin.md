
# V8.0 Splunk Plugin


```python
import urllib
import urllib.parse as up
import re
from xml.dom import minidom
import json
import requests
import warnings
import pythonutil
warnings.filterwarnings("ignore")

def extract_param(param):
    # The NetBrain initial parameters with customized fields.
    if isinstance(param, str):
        param = json.loads(param)  
    #username, password, endpoint are build-in keywords in initial param.
    username = ''
    password = ''
    endpoint = ''
    #callParam is customized fields.
    api_params = {}
    apiServerId = ''
    servInfo = {}
    if 'apiServerId' in param:
        apiServerId = param['apiServerId']
        servInfo = pythonutil.GetApiServerInfo(apiServerId)
        username = servInfo['username']
        password = servInfo['password']
        endpoint = servInfo['endpoint']
        api_params = param['api_params']
    else:
        username = param["username"]
        password = param["password"]
        endpoint = param["endpoint"]
        api_params = param['api_params']
    return (endpoint, username, password, api_params)
 
            
def post_searchId(rtn_params): #, sessionkey
    endpoint, username, password, api_params  = extract_param(rtn_params)
    headers = {}
  
    searchquery = str(api_params["searchquery"])
    if not searchquery.startswith('search'):
            searchquery = 'search ' + searchquery
    
    try:
        body = {'search' : searchquery}  
        searchURL = endpoint + str(api_params["post_searchID_uri"])#headers = headers
        searchJob = requests.post(searchURL, auth = (username, password), data = body, verify=False).content
        sid = minidom.parseString(searchJob).getElementsByTagName('sid')[0].childNodes[0].nodeValue
        return str(sid)
    
    except Exception as e:
            return "Error! - " + str(e)
            
def get_data(rtn_params, sid):
    
    endpoint, username, password, api_params  = extract_param(rtn_params)
    
    if "Error" in sid:
        return str(sid)
    else:
        Get_Search_Status_URL = endpoint + str(api_params["get_searchStatus_uri"]) %sid
        services_search_results_str = str(api_params["get_data_uri"]) %sid
      
    try:
        isnotdone = True
        while isnotdone:

            searchStatus = requests.get(Get_Search_Status_URL, auth = (username, password), verify = False).content.decode('utf-8')
            isdonestatus = re.compile('isDone">(0|1)')
            isdonestatus = isdonestatus.search(searchStatus).groups()[0]

            if (isdonestatus == '1'):
                isnotdone = False
                full = endpoint + services_search_results_str
                searchStatus = requests.get(full, auth = (username, password), verify = False).content
        
        parsed = json.loads(searchStatus)["results"]
        res = json.dumps(parsed)
        return res        
                
    except Exception as e:
            return "Error! - " + str(e)
            
def _test(param):
    #if isinstance(params, str):
    test_param = json.loads(param) 
    test_param['api_params'] = {'post_searchID_uri' : '/services/search/jobs', 'searchquery' : 'sourcetype=iis'}
    result = str(post_searchId(test_param))
    #"isFailed" and "msg" key fileds are the required.
    rtn = {"isFailed":False, "msg":result}
    return rtn
```
