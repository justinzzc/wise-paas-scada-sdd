# GO Web Framework Survey

* 預計採用的web framework & releatd tool
    * [Gin](https://github.com/gin-gonic/gin)
        * 輕量化、只提供基本功能, 像是HTTP Routing功能、專案本身不會太肥
        * 效能快 ( [Radix Tree](http://en.wikipedia.org/wiki/Radix_tree) algorithm)
        * 資源多, star是目前go web framwork最多
        * gin+vue / gin+angular / gin+react 案例多
    * [GORM](https://github.com/jinzhu/gorm)
        * MySQL/PostgreSQL/Sqlite3/SQL Server
        * 支援postgresql多schema機制, DB→SCHEMA→ TABLE結構
        * 支援Transactions/Composite Primary Key/SQL Builder
        * 目前GO ORM 框架stat數最多的一套
        * gin+gorm case多
    * Migration
        * **計畫自己寫flyway go介面, 跟目前我們用的node.js版類似**
        * 因為目前看到的go migrate 工具都沒有支援checksum機制
        * 用同一套migrate tool,經驗可以沿用
    * swagger
        * 目前survey到的都是把code裡面的註解轉成swagger, 還沒找到像loopback那種把method的參數轉成doc的工具
* 討論
    * Dashboard Team目前使用的是Macaron (因為[Grafana](http://grafana.org/)使用這套)
        * 跟Gin很類似, 也是提供核心的一些web module, 但star數差了10倍, gin兩萬, macaron兩千, 擔心資源不足

