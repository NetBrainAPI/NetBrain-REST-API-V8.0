
## Trigger DVT input parameters in runbook

### Input parameters:

| Name | Type | Description |
|---|---|---|
|dataview_template_settings |array	|data view template node setting|
|dataview_template_settings.deviceVariables |array|	device variable setting|
|dataview_template_settings.deviceVariables.deviceName |string|	device name|
|dataview_template_settings.deviceVariables.values |array|	device variable|
|dataview_template_settings.deviceVariables.values.alias |string	|device variable name|
|dataview_template_settings.deviceVariables.values.value |string|	device variable value|
|dataview_template_settings.templateVariables |array	|template variable setting|
|dataview_template_settings.templateVariables.alias |string	|variable name|
|dataview_template_settings.templateVariables.value |string	|variable value|
|dataview_template_settings.dataSource |object	|data source seetting|
|dataview_template_settings.dataSource.type |int	|1: live.<br> 2: base line.<br> 3: range.<br> 4: around.|
|dataview_template_settings.dataSource.recent |time	|set recent, valid when Type is Live|
|dataview_template_settings.dataSource.range |time	|Gets or sets range, valid when Type is Range|
|dataview_template_settings.dataSource.around |time	|Gets or sets around, valid when Type is Around|
|dataview_template_settings.frequency |object	|run frequency setting|
|dataview_template_settings.frequency.type |int	|frequency type|
|dataview_template_settings.frequency.times |int	|times|
|dataview_template_settings.frequency.interval |object	|cycle interval|
|dataview_template_settings.frequency.interval.unit |int	|time unit|
|dataview_template_settings.frequency.interval.duration |float|	duration value|

***Example:***


```python
task_parameter = {
    'basic_setting': {
        'user': 'admin',
        'device': 'BJ*POP',
        'stub_name': 'dvt-stub',      
        'triggered_by': user
    },
    'map_setting': {       
        'map_create_mode': 0,
        'map_device_para':{},
    },
    'dataview_template_settings': [{
        'deviceVariables': [{
            'deviceName': 'BJ*POP',
            'values': [{
                'alias': 'd1',
                'value': 'Fast'
            }]
        }],
        'templateVariables': [{
            'alias': 't1',
            'value': '2'
        }],
        'dataSource': {
            'type': 1
        },
        'frequency': {
            'type': 1
        }
    }]
}
```
