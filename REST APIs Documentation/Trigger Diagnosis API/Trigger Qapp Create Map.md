
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


```python

```
