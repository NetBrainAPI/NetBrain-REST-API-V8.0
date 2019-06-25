
## Trigger input parameter for verify application node

### Input parameters:

| Name | Type | Description |
|---|---|---|
|verify_application_settings |array	|verify application node setting|
|verify_application_settings.deviceNames| array	|all seed device name|
|verify_application_settings.topApp| int|	how many path aam we will select when search by devices|
|verify_application_settings.topPathEachApp |int |how many path in aam we will select when search by devices|
|verify_application_settings.dataSource object|	data |source seetting|
|verify_application_settings.dataSource.type| int|	1 live,2 base line|
|verify_application_settings.shareAlert |object	|alert and email setting|
|verify_application_settings.shareAlert.alertUsers |array|	alert user setting|
|verify_application_settings.shareAlert.alertUsers.userId |string|	alert user id|
|verify_application_settings.shareAlert.alertUsers.userName |string|	alert user name|
|verify_application_settings.shareAlert.sendEmailTo |array	|email address|
|verify_application_settings.isLiveUseBaseLineConfig |string	|set a value indicate use base line|
|verify_application_settings.advancedOption |object	|advance setting|
|verify_application_settings.advancedOption.debugMode | bool	|The debug mode of trigger API|
|verify_application_settings.advancedOption.calcWhenDeniedByACL| bool	| Whether to keep calculate when the process been denied by ACL|
|verify_application_settings.advancedOption.calcWhenDeniedByPolicy |bool	|Whether to keep calculate when the process been denied by policy|
|verify_application_settings.advancedOption.calcL3ActivePath| bool	||
|verify_application_settings.advancedOption.useCommandsWithArguments| bool	|Whether to use the commands with arguments inside.|
|verify_application_settings.advancedOption.enablePathFixup	|bool	|Whether to enable the path fixup feature.|

***Example：***


```python
task_parameter = {
    'basic_setting': {
        'user': 'admin',
        'device': 'BJ*POP',
        'stub_name': 'aam-stub',
        'triggered_by': user
    },
    'map_setting': {     
        'map_create_mode': 0,
        'map_device_para':{},
    },   
    'verify_application_settings': [{
        'deviceNames':['BJ-R1','BJ-R2'],
        'topApp':1
    }]
}
```
