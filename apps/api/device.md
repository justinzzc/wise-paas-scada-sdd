### 3.2 Device
#### 3.2.1 Models
##### 3.2.1.1 Device {#model_Device}
- Data Source: device_list
- Properties: 

|Name|Data Type|Primary Key|Column Name|Description|
|:--:|:-------:|:-:|:--------:|:---------:|
|ScadaId|String|V|scada_id||
|DeviceId|String|V|device_id||
|DeviceName|String||device_name||
|DeviceType|Number||devcie_type||
|DeviceIP|String||device_ip||
|DeviceDesc|String||device_description||
|ComportNbr|Number||comport_nbr|||

##### 3.2.1.2 DeviceCount {#model_DeviceCount}
- Properties: 

|Name|Data Type|Description|
|:--:|:-------:|:--------:|
|List|Array|[Device](#model_Device) Object|
|TotalCount|Number|The total count instances of the model from the data source matched by where filter|
|Index|Number|Starting Index|
|Count|Number|Data retrived|

##### 3.2.1.2 DeviceList {#model_DeviceList}
- Properties: 

|Name|Data Type|Description|
|:--:|:-------:|:--------:|
|DeviceId|String|Device Id|
|DeviceName|String|Device Name|

##### 3.2.1.2 DeviceUpdateInstance {#model_DeviceUpdateInstance}
- Properties: 

|Name|Data Type|Description|
|:--:|:-------:|:--------:|
|DevicePort|Number|Device port|
|DeviceIP|String|Device IP|
|DeviceDesc|String|Device description|
- Validation:
 - **DevicePort**: 0 < DevicePort < 65535
 - **DeviceIP**: X.X.X.X, 其中 X < 256
 - **DeviceDesc**: length < 256

#### 3.2.2 Functions
##### 3.2.2.1 listAllDeviceByScadaId {#function_listAllDeviceByScadaId}
- Purpose: List all device information in the specified SCADA
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|scadaId|String|V||Scada Id|
|count|Integer||1000|Data retrived. limit: 1000|
|index|Integer||1|Starting Index|
|deviceName|String|||Filter Scada Name|
|deviceDesc|String|||Filter Scada Description|
|sortby|String|||Sort by the specified property|
|order|String||DESC|ascending (ASC) or descending (DESC) only|
- Output: 
 - 200 [DeviceCount](#model_DeviceCount) Object
 - 400 Input invalid
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. 利用 [formatRegexpFilter](#function_formatRegexpFilter) 產生 filter.where
 3. 檢查 index 和 count
 4. 利用 [formatSortBy](#function_formatSortBy) 產生 filter.order
 5. 呼叫 [countList](#function_countList) query Device model

##### 3.2.2.2 listAllDevice {#function_listAllDevice}
- Purpose: List all device information
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|count|Integer||1000|Data retrived. limit: 1000|
|index|Integer||1|Starting Index|
|deviceName|String|||Filter Scada Name|
|deviceDesc|String|||Filter Scada Description|
|sortby|String|||Sort by the specified property|
|order|String||DESC|ascending (ASC) or descending (DESC) only|
- Output: 
 - 200 [DeviceCount](#model_DeviceCount)
 - 400 Input invalid
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
- Logical description:
1. 呼叫 [validToken](#function_validToken) 檢查 token
2. 利用 [formatRegexpFilter](#function_formatRegexpFilter) 產生 filter.where
3. 檢查 index 和 count
4. 利用 [formatSortBy](#function_formatSortBy) 產生 filter.order
5. 呼叫 [countList](#function_countList) query Device model


##### 3.2.2.3 listAllDeviceNameByScada {#function_listAllDeviceNameByScada}
- Purpose: List all device Id and device name in the specified SCADA
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|scadaId|String|V||Scada Id|
- Output: 
 - 200 Array of [DeviceList](#model_DeviceList)
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. Query Device model 中屬於 scadaId 的 DeviceId 和 DeviceName 欄位

##### 3.2.2.4 listDeviceByDeviceId {#function_listDeviceByDeviceId}
- Purpose: 更新特定 deviceId 的 Device 資訊
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|scadaId|String|V||Scada Id|
|deviceId|String|V||Device Id|

- Output: 
 - 200 Boolean. return true, if update successfully.
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. Query Device model with scadaId and deviceId

##### 3.1.2.4 updateDevice {#function_updateDevice}
- Purpose: 更新特定 deviceId 的 Device 資訊
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|scadaId|String|V||Scada Id|
|obj|[DeviceUpdataInstance](#model_DeviceUpdataInstance)|V||object of update properties|
- Output: 
 - 200 Boolean. return true, if update successfully.
 - 400 Input invalid
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
- Logical description:
 1. 呼叫 [validToken](#function_validToken) 檢查 token
 2. 檢查 [DeviceUpdataInstance](#model_DeviceUpdataInstance) 是否合法
 3. 將可更新 properties 整理 update obj
 4. 檢查 ScadaId 是否存在
 5. 呼叫 [_startUpdateTransaction](#function__startUpdateTransaction) 更新scada

##### 3.1.2.4 _startUpdateTransaction {#function__startUpdateTransaction}
- Purpose: 使用 transaction 處理 update device 和 deviceManager
- Input:

|Name|Data Type|Necessary|Default|Description|
|:--:|:-------:|:-------:|:-----:|:---------:|
|req|Object|V||request Object|
|scadaId|String|V||Scada Id|
|obj|[ScadaUpdataInstance](#model_ScadaUpdataInstance)|V||object of update properties|
- Output: 
 - 200 Boolean. return true, if update successfully.
 - 400 Input invalid
 - 401 No Authorization or token format error
 - 404 Result not found
 - 500 Interval error
- Logical description:
 1. 整理傳給 deviceManager 的 params
   - params[ScadaId] = {Device: {DeviceId: device}}
 2. 開啟 transaction
 3. 更新符合 where 條件的 Device 並呼叫deviceManager.addModifiecConfigRecord