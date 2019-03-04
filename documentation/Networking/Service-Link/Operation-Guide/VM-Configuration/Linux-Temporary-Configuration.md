# Linux系统临时配置

本教程以CentOS 6.8操作系统为例，介绍如何在云主机内配置弹性网卡（该方法并不永久保存弹性网卡配置，云主机重启后失效）。

**注意：中括号中的内容为用户自行填写内容。**

## 操作步骤
步骤1：在京东云控制台将弹性网卡绑定至目标云主机。

步骤2：通过SSH方式远程登录至目标云主机。

步骤3：执行以下命令查询已挂载的弹性网卡名称。

	# ifconfig -a

步骤4：执行以下命令启用弹性网卡。

	# ifconfig [device name] up

步骤5：执行以下命令配置弹性网卡主IP。

	# ifconfig [device name] [primary ip] netmask [netmask] broadcast [broadcast ip]

步骤6：执行以下命令配置弹性网卡辅助IP。

	# ifconfig [device name]:[secondary ip sequence number] [secondary ip] netmask [netmask] broadcast [broadcast ip]


