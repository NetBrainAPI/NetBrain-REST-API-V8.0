
## Introduction
### What is NetBrain Single Pane of Glass (SPoG)?
NetBrain integrates with different data sources within an enterprise to use NetBrain map and Qapp for
data correlation, analysis, and troubleshooting.

### How does NetBrain SPoG work?
NetBrain has Python function defined in API Plugin to send HTTP/HTTPS request to 3rd party system to
query 3rd party system data via REST API. End user needs to specify the 3
rd party system REST API for 
certain corresponding data in NetBrain API Parser. NetBrain will then be able to implement the parser in
a Qapp to further process the REST API retrieved data to generate NetBrain Data View on NetBrain
maps.
This documentation uses display ServiceNow Incident table data as NetBrain Data View (DV) as an
example to explain how ServiceNow SPoG is implemented in NetBrain system.


## Integrate with Splunk
In this case, we mainly focus on  calling Splunk REST API and set Splunk searchquery as an input to retreive corresponding data from Splunk, then put the data into NetBrain system with correlation, analysis or troubleshooting. 

First, customer needs to know how to use Splunk search, click [here](https://docs.splunk.com/Documentation/Splunk/7.2.5/Search/GetstartedwithSearch).
After customer confirm that the syntax of searchquery is correct and can retreive the expecting data from Splunk console then we can forward to the next step **Create API Plugin in NetBrain for Splunk**.

### Defining an API Plugin
API Plugin is a component to define a function template for the API Parser and API Server to work with a third-party system.<br>

To define a new API plugin, complete the following steps.<br>

**Tip:** The system provides a built-in Splunk API Plugin and you can refer to it to write your own ones in the API Plugin Manager.

At the beginning, customer needs to know how to creating searches using the Splunk REST API, click [here](https://docs.splunk.com/Documentation/Splunk/7.2.5/RESTTUT/RESTsearches). 

**Step:**<br>
1.[Log into System Management page.](https://www.netbraintech.com/docs/ie71/help/logging-in-system-admin-page.htm)<br>
2.Select the **API Plugin Manager** tab and click **Add**. <br>
<img src="images\add_plugin_manager.png" /><br>
3.Enter a name in the Plugin Name field, such as ServiceNow Incidents.<br>
4.Enter a description of the API plugin in the Description field.<br>
5.Enter the API script in the Script field. <br>
**Tip:** You can click Popup to prompt a larger script interface. Alternatively, you can click Import to import an existing python file directly.<br>
6.Click Save to save the definition.

***Splunk Plugin Python Script:***


```python
# Import all useful python modules.
import urllib
import urllib.parse as up
import re
from xml.dom import minidom
import json
import requests
import warnings
import pythonutil
warnings.filterwarnings("ignore")

# define an extract parameter function for plugin to parsing the input variables value from parser input.
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
 
# First Splunk REST API call, use a POST function with a searchquery as input to create a search ID which will be a required 
# input for next API call.             
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

# Second Splunk REST API call, use a GET function with searchID as input which from previous API call to get the expect data 
# result from Splunk. Actually we totally calling two APIs in this function, one is search status API the other is getting data.
# Before getting the search data, we need to confirm whether the data searching in Splunk is finished. 
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
        
# Set a test function for NetBrain system testing, to confirm that whether the NetBrain front server connect with Splunk 
# REST API server successfully.  
def _test(param):
    #if isinstance(params, str):
    test_param = json.loads(param)
    test_param['api_params'] = {'post_searchID_uri' : '/services/search/jobs', 'searchquery' : 'sourcetype=iis'}
    result = str(post_searchId(test_param))
    #"isFailed" and "msg" key fileds are the required.
    rtn = {"isFailed":False, "msg":result}
    return rtn
```

Customer has to copy the whole python script into NetBrain plugin. After customer save the plugin, next step is navigate to NetBrain **Domain Management** then **API Server Manager** to set the API plugin to corresponding devices. Steps showing below:
<img src="images\api_manage_step.png" /><br>
A) Click the domain name on right up corner of customer NetBrain desktop, click on the **Manage Domain**.<br>
B) After redirection into **Domain Management** page, click on **Operations** and select **API Server Manager**.<br>
C) Find the plugin customer just created, to the right end of current row click the smail arrow and selest **Edit**.<br>
D) Fill in all fields in the new open dailog, click on **Manage Devices**.<br>
> **Note:** customer must input the Splunk account information correctly(username, password and endpoint).<br>

E) In the new open dialog, choose devices to connect to Splunk plugin, click **OK**.<br>
F) Then after jump to the previous dialog, click on the **Test** button. <br>
G) Check the test result.<br>

If the test result is **failed** then customer needs to check the NetBrain frontserver apilog file to confirm what kind of error has been occured.

After test the plugin successfully, next step is build a parser to input the variable and retreive data from Splunk.
To create a NetBrain parser click [here](https://www.netbraintech.com/ftp/IE71/OnlineHelp/creating-an-api-parser.htm).



```python
# Define function to retrieve data first.
'''
For sample
[
    {"name": "$param1"},
    {"name": "$param2"}
]
'''

def BuildParameters(context, device_name, params):
    rtn_params = {}
    rtn_params['api_params'] = {
        'post_searchID_uri' : '/services/search/jobs',
        'get_searchStatus_uri' : '/services/search/jobs/%s/',
        'get_data_uri' : '/services/search/jobs/%s/results?output_mode=json&count=0',
        'searchquery' : 'customer need to input their own search query at here'
                       #example: "sourcetype=iis earliest=-5m@m latest=now() | timechart span=5m count as Number by c_ip"
    }
    return (True, rtn_params)
	
def RetrieveData(rtn_params):
    sid = post_searchId(rtn_params)
    data = get_data(rtn_params, sid)
    return data
```

For the parse data function, because it is rely on the data retrieved from Splunk and the schema of data table that customer want to present in NetBrain. It is a absolute customized function. We don't want to provide sample code at here.

After all these steps have been deployed, it means the integration has been finished 80%. Next step is for customer to build Qapp in NetBrain to processing data from Splunk. To create Qapp please click [here](https://www.netbraintech.com/ftp/DE10/OnlineHelp/HTML.html?create_qapp.htm)


```python

```
