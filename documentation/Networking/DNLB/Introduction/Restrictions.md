# 限制说明
您可以快速创建并使用分布网络负载均衡，但使用中有部分约束条件需要注意。

| 资源	| 限制	| 例外申请方式 |
| :- | :- | :- |
|单地域下DNLB实例个数	|100个	|工单|
|一个DNLB下的监听器个数	|50个	|无|
|一个DNLB下的后端服务个数	|50个	|无|
|一个DNLB下的虚拟服务器组个数	|50个|	无|
|一个虚拟服务器组内的实例个数	|1000个|	无|
## 相关参考
- [产品概述](../Introduction/Product-Overview.md)
- [产品定价](../Pricing/Billing-Overview.md)
- [创建DNLB实例](../Operation-Guide/Create-DNLB-Instance.md)
- [创建虚拟服务器组](../Operation-Guide/TargetGroup-Management.md)
- [配置监听策略](../Operation-Guide/Listener-Management.md)
- [管理后端服务与查看服务实例健康状态](../Operation-Guide/Backend-Management.md)
