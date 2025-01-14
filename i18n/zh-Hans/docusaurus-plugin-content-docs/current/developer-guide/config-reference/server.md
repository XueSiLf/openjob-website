---
sidebar_position: 1
---

# Server

## 环境变量

通过环境变量配置参数，一般用于 Docker 和 Kubernetes 部署。

| 参数名称    |                                                                                                      默认值 | 描述  |
|:----------|---------------------------------------------------------------------------------------------------------:|:--:|
| OJ_ADMIN_USER_PWD_SALT | ... | 加密盐 |
| OJ_DS_DRIVER_CLASS | `com.mysql.cj.jdbc.Driver`| 数据库驱动，默认MYSQL |
| OJ_DS_URL | `jdbc:mysql://127.0.0.1:3306/openjob?...` | 数据库 URL 地址 |
| OJ_DS_USERNAME | root | 数据库账号 |
| OJ_DS_PASSWORD | 123456 | 数据库密码 |
| OJ_DS_HK_MINI_IDLE | 1 | 连接池最小空闲数 |
| OJ_DS_HK_MAX_POOL_SIZE | 10| 连接池最大数 |
| OJ_DS_HK_IDLE_TIMEOUT | 60000 | 连接池空闲超时 |
| OJ_DS_HK_POOL_NAME | openjob | 连接池名称 |
| OJ_FW_LOCATIONS | `classpath:db/migration/mysql` | 表结构迁移文件，默认MYSQL |
| OJ_FW_TABLE | `migration_version` | 表结构迁移历史记录表名 |
| OJ_LOG_STORAGE_SELECTOR | mysql | 日志存储类型，默认 MYSQL |
| OJ_LOG_STORAGE_H2_USER | root| H2 日志存储，数据库账号  |
| OJ_LOG_STORAGE_H2_PASSWORD | 123456 | H2 日志存储，数据库密码 |
| OJ_LOG_STORAGE_H2_URL | `jdbc:h2:mem:openjob;AUTO_RECONNECT=TRUE...`| H2 日志存储，数据库 URL |
| OJ_LOG_STORAGE_MYSQL_USER | root| Mysql 日志存储，数据库账号 |
| OJ_LOG_STORAGE_MYSQL_PASSWORD | 123456 | Mysql 日志存储，数据库密码 |
| OJ_LOG_STORAGE_MYSQL_URL | jdbc:mysql://127.0.0.1:3306/openjob?... | Mysql 日志存储，数据库 URL |
| OJ_SCHEDULER_DELAY_ENABLE | false | 延时任务，开启状态，默认 false |
| OJ_REDIS_HOST | `127.0.0.1` | Redis 地址 |
| OJ_REDIS_PASSWORD | - | Redis 密码，默认空 |
| OJ_REDIS_DB  | 0 | Redis db |
| SPRING_JPA_SHOW_SQL  | false | SQL 打印状态，默认关闭 |

:::tip
1. 容器启动参数是通过环境变量方式配置
2. 数据库驱动暂时只支持 Mysql，后续会支持 PostgreSQL/Oracle
3. 日志存储暂时支持H2/MYSQL，后续会支持 elasticsearch/tidb/MongoDB
4. 延时任务是可选项，但是开启延时任务时，必须配置 Redis 否则无法使用。
5. 还有部分其它参数未通过环境变量方式，如有场景需要修改，可以通过文件挂载方式实现。
:::

## 配置文件

通过 `application.properties` 方式配置参数，一般用于直接部署或容器挂载配置文件。

```shell
server.port=${SERVER_PORT:8080}
### admin config
# user passwd hash salt
openjob.admin.user.passwd-salt=${OJ_ADMIN_USER_PWD_SALT:...}
### spring config
spring.jackson.serialization.FAIL_ON_EMPTY_BEANS=false
spring.datasource.driver-class-name=${OJ_DS_DRIVER_CLASS:com.mysql.cj.jdbc.Driver}
spring.datasource.url=${OJ_DS_URL:jdbc:mysql://127.0.0.1:3306/openjob?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai}
spring.datasource.username=${OJ_DS_USERNAME:root}
spring.datasource.password=${OJ_DS_PASSWORD:123456}
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
spring.datasource.hikari.minimum-idle=${OJ_DS_HK_MINI_IDLE:1}
spring.datasource.hikari.maximum-pool-size=${OJ_DS_HK_MAX_POOL_SIZE:10}
spring.datasource.hikari.idle-timeout=${OJ_DS_HK_IDLE_TIMEOUT:60000}
spring.datasource.hikari.pool-name=${OJ_DS_HK_POOL_NAME:openjob}
# fixed warn for "spring.jpa.open-in-view is enabled by default"
spring.jpa.open-in-view=false
spring.flyway.enabled=true
spring.flyway.clean-disabled=true
spring.flyway.locations=${OJ_FW_LOCATIONS:classpath:db/migration/mysql}
spring.flyway.baseline-on-migrate=true
spring.flyway.table=${OJ_FW_TABLE:migration_version}
spring.flyway.baseline-version=0
spring.flyway.encoding=UTF-8
spring.flyway.validate-on-migrate=false
openjob.log.storage.selector=${OJ_LOG_STORAGE_SELECTOR:mysql}
openjob.log.storage.h2.properties.user=${OJ_LOG_STORAGE_H2_USER:root}
openjob.log.storage.h2.properties.password=${OJ_LOG_STORAGE_H2_PASSWORD:123456}
openjob.log.storage.h2.properties.url=${OJ_LOG_STORAGE_H2_URL:jdbc:h2:mem:openjob;AUTO_RECONNECT=TRUE;MODE=MySQL;DB_CLOSE_DELAY=-1;DATABASE_TO_UPPER=false;WRITE_DELAY=0;}
openjob.log.storage.mysql.properties.user=${OJ_LOG_STORAGE_MYSQL_USER:root}
openjob.log.storage.mysql.properties.password=${OJ_LOG_STORAGE_MYSQL_PASSWORD:123456}
openjob.log.storage.mysql.properties.url=${OJ_LOG_STORAGE_MYSQL_URL:jdbc:mysql://127.0.0.1:3306/openjob?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai}
openjob.scheduler.delay.enable=${OJ_SCHEDULER_DELAY_ENABLE:false}
spring.redis.host=${OJ_REDIS_HOST:127.0.0.1}
spring.redis.password=${OJ_REDIS_PASSWORD:}
spring.redis.database=${OJ_REDIS_DB:0}
spring.redis.client-type=lettuce
spring.redis.lettuce.pool.max-active=32
spring.redis.lettuce.pool.max-idle=8
spring.redis.lettuce.pool.max-wait=1000
spring.redis.lettuce.pool.time-between-eviction-runs=60s
spring.jpa.show-sql=${SPRING_JPA_SHOW_SQL:false}
```

:::tip
配置参数与环境变量完全一致
:::