# API

---
## 1. Overview

## 2. Director Tree
```
│
├ common
│   ├ models
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
│        ├ db.js
│        ├ error.js
│        └ utils.js
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
│   │   ├ count.js
│   │   ├ count.json
│   │   ├ device-count.js
│   │   ├ device-count.json
│   │   ├ device-list.js
│   │   ├ device-list.json
│   │   ├ device-update-instance.js
│   │   ├ device-update-instance.json
│   │   ├ discrete-tag.js
│   │   ├ discrete-tag.json
│   │   ├ hist-data-log.js
│   │   ├ hist-data-log.json
│   │   ├ hist-raw-data.js
│   │   ├ hist-raw-data.json
│   │   ├ role-insert-instance.js
│   │   ├ role-insert-instance.json
│   │   ├ role-scope.js	
│   │   ├ role-scope.json
│   │   ├ role-update-instance.js
│   │   ├ role-update-instance.json
│   │   ├ scada-count.js
│   │   ├ scada-count.json
│   │   ├ scada-instance.js
│   │   ├ scada-instance.json
│   │   ├ scada-list.js
│   │   ├ scada-list.json
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
## 3. Component-Level Design