# Database

## Flyway

#### 规范

1. 必须放在 `src/main/resources/db/migration`
2. sql文件命名为 `V<VERSION>__<NAME>.sql`，
   * `<VERSION>` - 版本号，由一个或多个数字构成，数字之间使用点号做为分隔符，例如 `1.0` 、 `1.1` 、`2.0`
   * `<NAME>` - 数据库更新的主要内容
   * `<VERSION>` 和  `<NAME>` 之间用**两个**下划线分隔

最终的例子： `V1.0__init.sql` 、`V1.1__add_system_user.sql` 

flyway在数据库中创建一张表 `schema_version`，记录当前数据库的版本，如果当前版本为 `V1.1`，当 `migration` 目录中有多个sql文件时，会按文件版本从底到高依次执行。

#### 初始化

https://flywaydb.org/getstarted

第1步 生成初始脚本，例如 `V1.0__init.sql`

第2步 运行

```
Flyway flyway = new Flyway();
flyway.setDataSource(
				"jdbc:mysql://127.0.0.1:3306/plus?characterEncoding=UTF-8", "root", "");
flyway.migrate();
```

之后就是维护迁移sql脚本，在执行 `flyway.migrate();`

#### SpringBoot 集成

只要在maven中加入依赖，项目启动时自动运行 `migrate`

```xml
<dependency>
  <groupId>org.flywaydb</groupId>
  <artifactId>flyway-core</artifactId>
</dependency>
```

如果不想开启，设置 `flyway.enabled=false`

#### [已存在的数据库的时候](https://flywaydb.org/documentation/existing) 

第1步 生成初始脚本

```sql
mysqldump -h 10.200.0.103 -u root -p --no-data --compact --skip-opt  plus |egrep -v "(^SET|^/*!)" > src/main/resources/db/migration/V1.0__init.sql
```

第2步 生成 baseline

```
Flyway flyway = new Flyway();
flyway.setDataSource(
				"jdbc:mysql://127.0.0.1:3306/plus?characterEncoding=UTF-8", "root", "");
flyway.baseline();
```

第3步 维护迁移sql脚本，并执行 `flyway.migrate();`