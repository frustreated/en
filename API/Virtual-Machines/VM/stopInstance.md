# stopInstance


## Description
When stop a single VM, only the VM in <b>running</b> status can be stopped, and the VM can only be stopped without any ongoing task


## Request method
POST

## Request address
https://vm.jdcloud-api.com/v1/regions/{regionId}/instances/{instanceId}:stopInstance

|Name|Type|Required or not|Default value|Description|
|---|---|---|---|---|
|**instanceId**|String|True||VM ID|
|**regionId**|String|True||Region ID|

## Request parameter
None


## Return parameter
|Name|Type|Description|
|---|---|---|



## Return code
|Return code|Description|
|---|---|
|**400**|Invalid parameter|
|**401**|Authentication failed|
|**404**|Not found  |
|**503**|Service unavailable|
|**200**|OK|
|**500**|Internal server error|