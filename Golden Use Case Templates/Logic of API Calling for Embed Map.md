
# Logic of API Calling for Embeded Map
Many service providers and large enterprises benefit from their powerful web portals which offer seamless integrations with various well-established network management systems. NetBrain Embedded Map is a technology offered by NetBrain, which integrates a highly dynamic and easily deployable network map into client web portal by providing them with a set of pre-defined iframes and API endpoints.

User can choose either of the following ways to consume NetBrain Embedded Map technology:<br>
>▪ View site maps, device group maps, and maps in the Public folder.<br>
▪ View path maps by calculating paths.

**Workflow**
><br>1. Modify the web.config file of your NetBrain Web Server to allow the iframe to render NetBrain web page.<br>2. Reference the Web Server script library in the portal.<br>3. Initialize NetBrain instance.<br>4. Construct the drop-down menu for Tenant.<br>5. Construct the drop-down menu for Domain.<br>6. Construct the drop-down menu for Site/Device Group/Public Map File.<br>7. Browse and open a specific map.<br>8. Construct the text box and drop-down menu for path options.<br>9. Calculate a path and view the path result.

## Web server configuration and UI preparation 
Please check the detail information in [reference](https://github.com/Gongdai/REST_API_with_Markdown/blob/master/Golden%20Use%20Case%20Templates/v3_NetBrain_Embedded_Map_Quick_Start_Guide.pdf)

## Relative REST API callings
a. Get Authentication Token and Specify Working Domain.
>1. [Login](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Login%20API.md) <br>
>2. [Get all accessible tenants](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Get%20All%20Accessible%20Tenants%20API.md) <br>
>3. [Get all accessible domains of a tenant](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Get%20All%20Accessible%20Domains%20API.md) <br>
>4. [Specify a working domain](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Specify%20A%20Working%20Domain%20API.md)

b. Get Specified Network Information and Calculate Path. 
>4. [Get child sites of a specific site](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Site%20Management/Get%20Child%20Site%20API.md) <br>
>5. [Get device group list](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Devices%20Management/Get%20Device%20Group%20API.md) <br>
>6. [Get the gateway information of a device](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Path%20Management/Resolve%20Device%20Gateway%20API.md) <br>
>7. [Calculate a Path](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Path%20Management/Calculate%20Path%20API.md) <br>
>8. [Get path calculation status]() <br>
>9. [Get path calculation result](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Path%20Management/Get%20Path%20Calculation%20Result%20API.md) <br>
>10. [Get file list](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Get%20file%20list%20API.md)

c. Finish All Callings and Logout Netbrain 
>11. [Stop a path](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Path%20Management/Stop%20A%20Path%20API.md)
>12. [Logout](https://github.com/NetBrainAPI/North-Bound-API/blob/master/REST%20APIs%20Documentation/Authentication%20and%20Authorization/Logout%20API.md)

### More further details required, please check the detail information in [reference](https://github.com/NetBrainAPI/North-Bound-API/blob/master/Golden%20Use%20Case%20Templates/v3_NetBrain_Embedded_Map_Quick_Start_Guide.pdf)
