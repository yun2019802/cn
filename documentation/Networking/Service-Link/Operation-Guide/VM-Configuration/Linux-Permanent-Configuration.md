# Linux系统永久配置

本教程以CentOS 6.8操作系统为例，介绍如何在云主机内配置弹性网卡（该方法永久保存弹性网卡配置，云主机重启后依然生效）。

**注意：中括号中的内容为用户自行填写内容。**

## 操作步骤
步骤1：在京东云控制台将弹性网卡绑定至目标云主机。

步骤2：通过SSH方式远程登录至目标云主机。

步骤3：执行以下命令查询已挂载的弹性网卡名称。

	# ifconfig -a

步骤4：执行以下命令进入网卡配置文件目录。

	# cd /etc/sysconfig/network-scripts

步骤5：执行以下命令创建弹性网卡配置文件。

	# touch ifcfg-[device name]

步骤6：执行以下命令启用弹性网卡配置文件编辑。

	# vi ifcfg-[device name]

步骤7：在弹性网卡配置文件中覆盖填写一下内容。

	DEVICE=[device name]
	NM_CONTROLLED=yes
	ONBOOT=yes
	IPADDR=[primary ip]
	NETMASK=[netmask]

步骤8：执行以下命令重启网络服务以使配置生效。

	# service network restart

