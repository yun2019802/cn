# 计费概述
## 实例收费标准
云数据库 RDS 实例的收费标准依据实例的规格和存储空间两个维度，并且不同的实例类型收费标准也是不一样的，详情请查看
- [MySQL 价格总览](./Price-Overview/MySQL-Price.md)
- [Percona 价格总览](./Price-Overview/Percona-Price.md)
- [MariaDB 价格总览](./Price-Overview/MariaDB-Price.md)
- [PostgreSQL 价格总览](./Price-Overview/PostgreSQL-Price.md)
- [SQL Server 价格总览](./Price-Overview/SQL-Server-Price.md)


## 实例到期/欠费说明
以下是云数据库 RDS 实例欠费或者到期的保留政策。

|计费类型|实例到期/欠费处理逻辑|
|---|---|
|包年包月|实例到期后，会被标记为到期状态，数据库服务不可用，如 7 天内未续费，则 7 天后会被删除，同时实例对应的备份文件也会被删除，实例数据不能恢复。|
|按配置|实例欠费后，会被标记为欠费状态，数据库服务不可用，如 7 天后仍欠费，则会被删除，同时实例对应的备份文件也会被删除，实例数据不能恢复。|

云数据库 RDS 实例在到期或欠费后会有消息提醒，请注意查收，及时续费，以免数据被删除无法找回。
计费与通知相关信息，详情请查看：
- [预付费计费说明](../../../Finance/Billing/Billing-method/Prepay.md)
- [后付费计费说明](../../../Finance/Billing/Billing-method/Postpay.md) 
