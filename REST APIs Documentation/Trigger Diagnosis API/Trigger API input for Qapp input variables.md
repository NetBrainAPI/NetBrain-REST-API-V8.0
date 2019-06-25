
## Trigger API input for Qapp input variables

### Input parameters:

| Name | Type | Description |
|---|---|---|
|qapp_settings	|object	|qapp  node settings.|
|qapp_settings.threshold_paramters	|array	|threshold  parameters.|
|qapp_settings.threshold_paramters.name	|string	|threshold parameter name.|
|qapp_settings.threshold_paramters.value	|string	|threshold value.|
|qapp_settings.threshold_paramters.values	|array	|threshold values.|
|qapp_settings.input_variable_parameters	|array	|qapp input variable parameters.|
|qapp_settings.input_variable_parameters.name	|string	|input variable name.|
|qapp_settings.input_variable_parameters.value	|string	|input variable value.|
|qapp_settings.dataSource |object	|data source seetting.|
|qapp_settings.dataSource.type |int	|1 live,2 base line,3 range, 4 around.|
|qapp_settings.dataSource.recent |time	|set recent, valid when Type is Live.|
|qapp_settings.dataSource.range |time	|Gets or sets range, valid when Type is Range.|
|qapp_settings.dataSource.around |time	|Gets or sets around, valid when Type is Around.|
|qapp_settings.frequency |object	|run frequency setting.|
|qapp_settings.frequency.type |int|	frequency type.|
|qapp_settings.frequency.times |int	|times.|
|qapp_settings.frequency.interval |object	|cycle interval.|
|qapp_settings.frequency.interval.unit |int	|time unit.|
|qapp_settings.frequency.interval.duration |float	|duration value.|

***Example:***


```python
task_parameter = {
    'basic_setting': {
        'user': 'admin',
        'device': 'BJ*POP',
        'stub_name':'qapp_stub',
        'triggered_by': user
    },
    'map_setting': {
        'map_create_mode': 0,
        'map_device_para':{},
    },
    'qapp_settings':[{
                'thresholdParamters': [
                    {
                        'name': 'High CPU Utilization - Error',
                        'value': '800'
                    }
                ],
                'inputVariableParameters':[
                    {
                        'name':'VLAN',
                        'value':'100'
                    }
                ]
    }]
}
```
