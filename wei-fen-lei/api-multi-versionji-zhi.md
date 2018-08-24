# API Multi Version

* swagger上一律使用最新版
  * 因為不同版本的reqeust body / response format都會不一樣
* POST 
* 缺點
  * 因為API的驗證很多都是用loopback model validate，如果有多個版本並存，連帶的model也可能不能重用，需要為了相容舊的版本而手動產生很多model
  * 例如我新增group的model是GroupCreateInstance, 但假設新板的model有修改，這時就要去新增一份GroupCreateInstance\_V的model檔，
  * 而且多版本間的API 介面如果有差異的話，就得用hard code的方式新增很多的api route, 像GET就是
  * 主因是不同版本的api, 除了介面不同外, 可能連相依的模組/檔案/model等等都不同
    * 這樣同一份專案裡就會很肥, 而且命名上會很複雜,這樣命名會變成用\_V1這種方式來區分版本

## Solution 1

* 在整個Model定義路由規則

* ```
  "http": {
      "path": ":apiVersion/Groups"
    },
  ```
* 在API 介面帶入version
* ```
  Group.remoteMethod('listAllGroups', {
      http: { path: '/', verb: 'get' },
      accepts: [
        {
          arg: 'version', type: 'object', description: 'API version eg. v1, v2, etc.',
          http: function (context) {
            return { apiVersion: context.req.apiVersion || 'v1.0' };
          }
        },
        { arg: 'req', type: 'object', 'http': { source: 'req' } },
        { arg: 'groupId', type: 'string', required: false, description: 'groupId' },
        { arg: 'name', type: 'string', description: 'name of group' },
        { arg: 'count', type: 'number', default: 1000, description: 'Data retrived. limit: 1000' },
        { arg: 'index', type: 'number', default: 1, description: 'Starting Index' },
        { arg: 'sortby', type: 'string', description: 'Sort by the specified property' },
        { arg: 'desc', type: 'boolean', default: false, description: 'order by desc or not' }],
      returns: { arg: 'data', type: 'GroupList', root: true },
      description: 'List/find groups'
    });
  ```
* remote method function會像這樣
* ```
   Group.listAllGroups = async (version, req, groupId = null, name = null, count = 1000, index = 1, sortby = 'name', desc = false, cb) => {
      switch (version.apiVersion) {
        case 'v1':
          // version specific API method
          break;
        case 'v2':
          // version specific API method
          break;
        default:
          cb(null, false);
      }
  ```
* ### 缺點

  * 當API兩版本的介面\(要傳的參數\)有差異時，此方法就不能用了，此方式的前提是介面一樣，只有API內部的邏輯有不同時適用
  * 因為API的驗證是用loopback model validate，如果版本間對於某個model的定義不同，那勢必要拆檔案，這樣一來此方法也不能用了

    * ex.  以POST /Groups 為例\(新建Groups\)，POST Request body會轉成GroupCreateInstance

      ```
      Group.remoteMethod('createGroup', {
          http: { path: '/', verb: 'post' },
          accepts: [
            { arg: 'req', type: 'object', 'http': { source: 'req' } },
            { arg: 'data', type: 'GroupCreateInstance', http: { source: 'body' }, required: true, description: 'name: name of group. description: group description. type: 1(email), 2(line). config.host: host of smtp server. config.port: port of smtp server. config.secure: smtp secure transport. config.method: TLS or SSL, required when config.secure is true. config.username: smtp username. config.password: smtp password. config.senderEmail: smtp MAIL FROM. config.emailSubject: subject of email. sendList[{firstName}] (option): firstName of send target. sendList[{lastName}] (option): lastName of send target. sendList[{target}]: email address or line token, based on the type.' }
          ],
          returns: { arg: 'data', type: 'object', root: true },
          description: 'Create new group'
        });
      ```

      ```
      {
        "name": "GroupCreateInstance",
        "base": "Model",
        "strict": true,
        "properties": {
          "name": {
            "type": "string",
            "required": true
          },
          "description": {
            "type": "string",
            "required": false
          },
          "type": {
            "type": "number",
            "required": true
          },
          "config": {
            "type": "GroupConfigInstance"
          },
          "sendList": {
            "type": [
              "GroupSendObject"
            ]
          }
        },
        "validations": [],
        "relations": {},
        "acls": [],
        "methods": {}
      }
      ```

    * 但假設在下一個版本時，GroupCreateInstance裡的name改成groupName, 這樣的話就需要新增一個GroupCreateInstanceV2的model\(json檔\)，而且remoteMethod的路由定義也不能用同一個了，因為該定義需要指定reqeust body會轉成那一種model

  * 假設兩版本間的API介面、用到的model都一樣，但api裡用到不同的node module，這樣一來專案就會越來越肥, dependency越來越多
* ### 優點

  * 架構較為乾淨

  * api endpoint不用分別建立



