## 发起部署

在“应用”页，进入到“部署组”分页，可见上一步中新建完成的部署组。

![Alt text](https://github.com/jdcloudcom/cn/blob/edit/image/CodeDeploy/starting9.png)

对于单个部署组，可支持的操作有：发起部署、部署历史、编辑、删除。

点击“发起部署”按钮，进入到“新建部署”页，有以下选项：

- 部署描述：非必须，为本次部署的描述信息
- 部署来源：支持多种部署来源，请选择URL上传
- URL地址：请填写http://ftp.cuhk.edu.hk/pub/packages/apache.org/tomcat/tomcat-7/v7.0.92/bin/apache-tomcat-7.0.92.tar.gz
- 文件类型：请选择.tar.gz
- 部署操作命令：请选择“输入部署操作命令”，使用“表单”填写功能，填写具体操作命令，如下：      

![Alt text](https://github.com/jdcloudcom/cn/blob/edit/image/CodeDeploy/starting17.png)

启动脚本：/home/apache-tomcat-7.0.92/bin/startup.sh

填写部署任务信息后，请点击“发起部署”，将触发部署操作，跳转到“部署详情”页。
