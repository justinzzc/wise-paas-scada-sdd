## 2. Directory Tree
```
│
├ common
│   ├ models
│   │   ├ alarm.js
│   │   ├ alarm.json
│   │   ├ const.js
│   │   ├ device.js
│   │   ├ device.json
│   │   ├ hist-data.js
│   │   ├ hist-data.json 
│   │   ├ project.js
│   │   ├ project.json
│   │   ├ real-data.js
│   │   ├ real-data.json
│   │   ├ role-info.js
│   │   ├ role-info.json
│   │   ├ scada.js
│   │   ├ scada.json
│   │   ├ scope-info.js
│   │   ├ scope-info.json
│   │   ├ status.js
│   │   ├ status.json
│   │   ├ tag.js
│   │   ├ tag.json
│   │   ├ token.js
│   │   ├ token.json
│   │   ├ user-info.js
│   │   └ user-info.json
│   └ utils
│       ├ error.js
│       └ utils.js
├ scripts
│   ├ gen_man.bat
│   └ gen_yml.bat
├ server
│   ├ explorer
│   │   ├ bwclient.ico
│   │   ├ index.html
│   │   ├ index.yml
│   │   ├ swagger-ui-bundle.js
│   │   ├ swagger-ui-bundle.js.map
│   │   ├ swagger-ui-standalone-preset.js
│   │   ├ swagger-ui-standalone-preset.js.map
│   │   ├ swagger-ui.css
│   │   ├ swagger-ui.css.map
│   │   ├ swagger-ui.js
│   │   └ swagger-ui.js.map
│   ├ models
│   │   ├ analog-tag.js
│   │   ├ analog-tag.json
│   │   ├ bind-scada-instance.js
│   │   ├ bind-scada-instance.json
│   │   ├ count.js
│   │   ├ count.json
│   │   ├ device-count.js
│   │   ├ device-count.json
│   │   ├ device-list.js
│   │   ├ device-list.json
│   │   ├ device-status.js
│   │   ├ device-status.json
│   │   ├ device-update-instance.js
│   │   ├ device-update-instance.json
│   │   ├ discrete-tag.js
│   │   ├ discrete-tag.json
│   │   ├ hist-data-log.js
│   │   ├ hist-data-log.json
│   │   ├ hist-raw-data.js
│   │   ├ hist-raw-data.json
│   │   ├ project-count.js
│   │   ├ project-count.json
│   │   ├ project-update-instance.js
│   │   ├ project-update-instance.json
│   │   ├ role-insert-instance.js
│   │   ├ role-insert-instance.json
│   │   ├ role-scope.js	
│   │   ├ role-scope.json
│   │   ├ role-update-instance.js
│   │   ├ role-update-instance.json
│   │   ├ scada-count.js
│   │   ├ scada-count.json
│   │   ├ scada-insert-instance.js
│   │   ├ scada-insert-instance.json
│   │   ├ scada-instance.js
│   │   ├ scada-instance.json
│   │   ├ scada-list.js
│   │   ├ scada-list.json
│   │   ├ scada-status.js
│   │   ├ scada-status.json
│   │   ├ scada-update-instance.js
│   │   ├ scada-update-instance.json
│   │   ├ sync-res.js
│   │   ├ sync-res.json
│   │   ├ sys-param.js
│   │   ├ sys-param.json
│   │   ├ tag-count.js
│   │   ├ tag-count.json
│   │   ├ tag-list.js
│   │   ├ tag-list.json
│   │   ├ tag-req.js
│   │   ├ tag-req.json
│   │   ├ tag-update-instance.js
│   │   ├ tag-update-instance.json
│   │   ├ tag-value.js
│   │   ├ tag-value.json
│   │   ├ text-tag.js
│   │   ├ text-tag.json	
│   │   ├ user-allow-device.js
│   │   ├ user-allow-device.json
│   │   ├ user-count.js
│   │   ├ user-count.json
│   │   ├ user-insert-instance.js
│   │   ├ user-insert-instance.json
│   │   ├ user-instance.js
│   │   ├ user-instance.json
│   │   ├ user-scope.js
│   │   ├ user-scope.json
│   │   ├ user-update-instance.js
│   │   ├ user-update-instance.json
│   │   ├ value.js
│   │   └ value.json
│   ├ component-config.json
│   ├ config.json
│   ├ datasources.development.json
│   ├ datasources.json
│   ├ datasources.production.js
│   ├ middleware.development.json
│   ├ middleware.json
│   ├ model-config.json
│   └ server.js
├ test
│   ├ deviceTest.js
│   ├ realdataTest.js
│   ├ roleTest.js
│   ├ scadaTest.js
│   ├ tagTest.js
│   └ userTest.js
├ .babelrc
├ .cfignore
├ .gitignore
├ CHANGELOG.md
├ Procfile
├ README.md
├ config.json
├ gulpfile.js
└ package.json
```

- `server/model-config.json`定義出所有的 model 以及此 model 是否為 public(外部是否可對 model 進行調用與操作)
- 每個 model 由一個 js 和一個 json 組成, 其中 json 定義 model 的 properties, js 則是定義 model 的驗證, 邏輯, 和對外的 interface
- 在`common/models`下的 model 皆為 public model, 主要用來提供 API url 的操作
- 在`server/models`下的 model 皆為 private mode, 主要用來 document 顯示, 以及 input 的驗證檢查

- model 的 js 檔中架構為 (以`common/models/project.js`為例)

```
'use strict';
/* import node modules */
var ScadaDBManager = require('scada-dbmanager');

module.exports = function (Project) {
  Utils.disableAllMethodsBut(Project, []);

  /* model properies 的驗證　*/
  Project.validatesPresenceOf('projectId');
  Project.validatesLengthOf('projectId', {max: 32, message: {max: 'too long'}});
  Project.validatesLengthOf('description', {max: 256, message: {max: 'too long'}, allowNull: true});
  Project.validate('projectId', checkSpecial, {message: 'is invalid. contain invalid characters.'});

  /* 驗證 function */
  function checkSpecial (err) {
    if (Utils.hasSpecial(this.projectId)) {
      err();
    }
  }

  /* function */
  Project.createProject = function (req, projectObj, cb) {
    app.models.Token.validScope(req, Const.Scope.EditConfig, function (err, token) {
      if (err) {
        return cb(err);
      }
      let userName = jwt.decode(token, { complete: true }).payload.username;
      /* 呼叫 model 驗證 */
      projectObj.isValid(function (valid) {
        if (!valid) {            
          /* 回傳Error Object */
          return cb(Err.paramsInvalidWithlbValidation(projectObj.errors));
        }
        ProjectDao.checkProjectRightByUserName(userName).then((res) => {
          if (res.find((raw) => raw.projectId === projectObj.projectId)) {
            return cb(Err.paramsIsDuplicate('projectId'));
          }
          return UserDao.getUserByName(userName).then((user) => {
            return ScadaDBManager.conn().transaction().then((trans) => {
              ProjectDao.insertProject(projectObj, trans).then((project) => {
                return UserAllowDeviceDao.insertAccessRight([{userId: user.userId, projectId: project.projectId}], trans).then((result) => {
                  trans.commit();
                  /* 回傳 response true */
                  return cb(null, true);
                });
              }).catch((err) => {
                trans.rollback();
                return cb(Err.internalError(err.message));
              });
            });
          });
        }).catch((err) => {
          return cb(Err.internalError(err.message));
        });
      });
    });
  };

  /* 對外 API (remote method) */
  Project.remoteMethod('createProject', {
    http: { path: '/', verb: 'POST' },
     /* api request */
    accepts: 
      { arg: 'req', type: 'object', 'http': { source: 'req' } },
      { arg: 'data', type: 'Project', required: true, http: { source: 'body' }, description: 'project instance' }
    ],
     /* api response */
    returns: { arg: '', type: 'boolean', root: true },
    description: 'Create a new project'
  });
};

```