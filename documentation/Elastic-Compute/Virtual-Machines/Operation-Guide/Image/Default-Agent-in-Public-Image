# 重要系统组件
官方镜像中默认安装了下述系统组件，以和对应服务或产品配合提供完备的产品功能和安全监控。建议不要进行卸载或禁止开机运行，否则会导致部分功能缺失。
受系统升级和组件演进的客观因素影响，早期官方镜像中可能未安装以下组件，建议您核查当前系统的安装情况后逐一完成安装。

| 组件名称    | 相关进程名称    | 主要功能     | 不安装有何影响    |
| --- | --- | --- | --- |
|   JCS-Agent  | JCS-Agent <br> MonitorPlugin-‘版本号’  <br>  UpgradePlugin-‘版本号’  | 通用核心组件，配合元数据服务提供密码密钥注入、自定义脚本注入、监控数据上报等功能    |  无法通过京东云控制台或openAPI设置密码、密钥、自定义用户数据，无法获得部分云主机监控数据   |
| Ifrit    |  ifrit-agent <br> ifrit-supervise   |   通用部署插件，实现JCS-Agent的自动升级	  |  无法获得自动升级JCS-Agent的能力，如后续希望使用基于JCS-Agent开发的新功能，需要手动升级   |
|  Jcloudhids   |jcloudhids <br> jcloudhidsupdate    | 安全核心组件，提供安全防护能力    | 无法通过“主机安全”产品控制台获得关于云主机的安全隐患及异常行为监测。    |
| Jdog-Monitor |	jdog-monitor.'版本号'<br>jdog-watchdog<br>jdog-kunlunmirror| 安全辅助插件，实现Jcloudhids的自动升级（目前仅安装于Linux系统）|无法获得自动升级Jcloudhids的能力，如后续希望使用基于Jcloudhids开发的新功能，需要手动升级|

* [JCS-Agent](Default-Agent-in-Public-Image#JCS-Agent)
* [Ifrit](Default-Agent-in-Public-Image#Ifrit)
* [Jcloudhids](Default-Agent-in-Public-Image#Jcloudhids)
* [Jdog-Monitor](Default-Agent-in-Public-Image#Jdog-Monitor)



<div id="JSC-Agent"></div>

## JCS-Agent
### 组件介绍
JCS-Agent是京东云自研的云主机核心组件，可提供诸如云主机基本信息（密码、密钥）注入、用户数据注入、Windows系统KSM激活、监控数据上报等功能。

2018年8月-12月之间官方镜像陆续升级，完成了JCS-Agent的默认安装，早期官方镜像中存在安装cloud-init和qemu-guest-agent的情况，此类镜像依然具有主机基本信息注入和监控上报功能，但用户数据注入、Windows系统KMS激活及后续新增功能均无法支持，如您当前使用的是早期agent建议您更换为JCS-Agent（如您当前使用的JCS-Agent为低于1.0.728的版本，建议您参照下方操作进行新版本的安装，以便获得Ifrit的自动升级管理功能）。

云市场镜像安装JCS-Agent的情况视镜像发布时间（基于何版本的官方镜像制作）和服务商制作情况，如您依赖JCS-Agent提供的某些特殊功能，请咨询云市场确认镜像内agent情况后再行使用。

### 安装准备
#### 卸载冲突软件
***如您使用的镜像为京东云外环境的导入镜像，且导入前已安装了cloud-init或qemu-guest-agent，请务必在完成卸载后再进行安装！***
如果卸载时提示软件未安装，则说明当前系统未做安装，可不用进行后续的配置文件和日志清理。同时建议卸载完成后运行`ps -ef`查看服务是否已清理。

① Cloudinit卸载清理：
卸载cloud-init：`rpm -e cloud-init `
删除原有配置和保留文件：`rm -rf /etc/conf/cloud/*   rm -rf /var/lib/cloud/* `

② Qemu-Guest-Agent卸载清理：
卸载qemu-guest-agent：`rpm -e qemu-guest-agent`
删除日志：`rm -fr /var/log/qemu-ga`

#### 查看当前软件版本
通过查看监控插件的版本来获悉JCS-Agent版本号。
Linux：`ps -ef|grep MonitorPlugin`
Windows：`wmic process where caption="MonitorPlugin.exe" get caption,commandline /value`

### 安装方式
**Linux：**
1、下载下述安装包和安装脚本，将其下载至同一目录中（比如：/root/jcloud）。
若主机未绑定公网IP，请将链接中的地域参数"cn-north-1"替换成主机所在地域的代码："cn-south-1"(华南-广州)、"cn-east-1"(华东-宿迁)、"cn-east-2"(华东-上海)。
https://bj-jcs-agent-linux.s3.cn-north-1.jdcloud-oss.com/jcloud-jcs-agent-linux-deploy.py
https://bj-jcs-agent-linux.s3.cn-north-1.jdcloud-oss.com/jcloud-jcs-agent-linux.zip
2、在存放安装包和脚本的目录中执行下述命令进行安装
```bash
python jcloud-jcs-agent-linux-deploy.py install
```
3、执行`ps -ef`看到JCSAgentCore、MonitorPlugin和UpgradePlugin三个进程即表示安装成功。安装成功后可以删除安装包和安装脚本。

**Windows:**
1、下载安装包、安装脚本和MD5工具，将其下载至同一目录中（比如： C:\jcloud）
若主机未绑定公网IP，请将链接中的地域参数"cn-north-1"替换成主机所在地域的代码："cn-south-1"(华南-广州)、"cn-east-1"(华东-宿迁)、"cn-east-2"(华东-上海)。
https://bj-jcs-agent-windows.s3.cn-north-1.jdcloud-oss.com/jcloud-jcs-agent-windows-manual.zip
https://bj-jcs-agent-windows.s3.cn-north-1.jdcloud-oss.com/jcloud-jcs-agent-win-deploy.ps1
https://bj-jcs-agent-windows.s3.cn-north-1.jdcloud-oss.com/MD5.exe
2、打开powershll，进入安装包所在的目录（C:\jcloud）执行下述命令进行安装
```
.\jcloud-jcs-agent-win-deploy.ps1 install
```
3、执行`ps`命令看到JCSAgentCore、MonitorPlugin和UpgradePlugin三个进程即表示安装成功。安装成功后可以删除安装包、安装脚本和MD5工具。

<div id="Ifrit"></div>

## Ifrit
### 组件介绍
Ifrit是京东云自研的轻量、通用的部署运维工具，可实现对其所管理组件的部署、升级、卸载等管理操作。Ifrit与JCS-Agent配合工作，实现对JCS-Agent的自动化升级。

官方镜像将在2019年5月-6月期间陆续升级，完成Ifrit的默认安装。云市场镜像安装情况视镜像发布时间（基于何版本的官方镜像制作）和服务商制作情况，具体请咨询云市场。

### 安装方式
**Linux：**
```bash
curl -fsSL http://deploy-code-vpc.jdcloud.com/dl-ifrit-agents/install_jcs | bash
```
**Windows:**
下述指令间区别仅为安装包所在的对象存储地域不同，若主机未绑定公网IP，请在下述指令中，选择主机所在地域的指令执行；若主机绑定公网IP，可任意选择一个执行。
华北-北京：
```powershell
($client = new-object System.Net.WebClient) -and ($client.DownloadFile('http://devops-hb.s3.cn-north-1.jcloudcs.com/ifrit/ifrit-external-v0.01.448.0742c84.20190327195007.exe', 'c:\ifrit.exe')) -or (Start-Process 'c:\ifrit.exe')
```

华东-上海：
```powershell
($client = new-object System.Net.WebClient) -and ($client.DownloadFile('http://devops-hd.s3.cn-east-2.jcloudcs.com/ifrit/ifrit-external-v0.01.448.0742c84.20190327195007.exe', 'c:\ifrit.exe')) -or (Start-Process 'c:\ifrit.exe')
```

华东-宿迁：
```powershell
($client = new-object System.Net.WebClient) -and ($client.DownloadFile('http://devops-sq.s3.cn-east-1.jcloudcs.com/ifrit/ifrit-external-v0.01.448.0742c84.20190327195007.exe', 'c:\ifrit.exe')) -or (Start-Process 'c:\ifrit.exe')
```

华南-广州：
```powershell
($client = new-object System.Net.WebClient) -and ($client.DownloadFile('http://devops.s3.cn-south-1.jcloudcs.com/ifrit/ifrit-external-v0.01.448.0742c84.20190327195007.exe', 'c:\ifrit.exe')) -or (Start-Process 'c:\ifrit.exe')
```

<div id="Jcloudhids"></div>

## Jcloudhids
### 组件介绍
Jcloudhids是京东云提供的主机安全核心组件，是“主机安全”产品所实现的安全监控及防范功能的核心，可提供防暴力破解、异常登陆检测、高危漏洞检测等安全功能。主机安全产品介绍请参考：https://www.jdcloud.com/cn/products/endpoint-security

官方镜像均默认安装了Jcloudhids。云市场镜像安装情况视服务商制作镜像情况，如镜像功能与Jcloudhids功能重复或冲突，不强制要求其安装Jcloudhids，具体请咨询云市场。

### 安装方式
请参考：https://docs.jdcloud.com/cn/endpoint-security/getting-started

<div id="Jdog-Monitor"></div>

## Jdog-Monitor
### 组件介绍
Jdog-Monitor是京东云提供的针对核心安全组件的升级插件，可实现安全相关组件的维护和升级。

### 安装方式
当前仅提供Linux系统的安装方式。
**Linux：**
1、下载安装包：（非华北地域主机请绑定公网IP后下载）
https://iaas-cns-download.oss.cn-north-1.jcloudcs.com/JdogMonitor/jdog-op-agent-master-fbe96b07-0306202642.tar
2、运行以下指令进行安装。
```bash
mkdir -p /usr/local/share/jcloud/jdog-monitor
tar zxvf jdog-op-agent-master-fbe96b07-0306202642.tar -C /usr/local/share/jcloud/jdog-monitor
/usr/local/share/jcloud/jdog-monitor/scripts/jdog_service install
```
