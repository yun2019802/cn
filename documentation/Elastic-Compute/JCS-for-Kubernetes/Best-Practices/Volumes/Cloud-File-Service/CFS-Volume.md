
# 使用京东云文件服务定义Kubernetes集群nfs类型的Volume

  Kubernetes集群提供了Volume的抽象，可以支持Pod中的容器共享文件和文件持久化存储的场景；NFS Volume允许将现有的 NFS（网络文件系统）共享挂载到集群的容器中，当Pod被删除时，NFS Volume中的文件内容被保留，并且NFS可以被多个源同时挂载，这就意味着NFS可以在Kubernetes集群的Pod之间提供数据共享。

  京东云文件服务（Cloud File Service）是一种高可靠、可扩展、可共享访问的全托管分布式文件系统。支持标准的NFSv4.0和NFSv4.1协议，提供全托管的服务，您无需修改应用，通过标准的文件系统挂载步骤即可实现与Kubernetes集群的无缝集成。更多详情参考[京东云文件服务](https://docs.jdcloud.com/cn/cloud-file-service/product-overview)产品文档。
  
  本文将提供在Kubernetes集群中以NFS Volume方式挂载京东云文件服务的操作步骤和应用示例。
  
## 一、创建CFS文件存储

1. 您需要首先创建一个[CFS文件存储](https://docs.jdcloud.com/cn/cloud-file-service/creating-file-system)，建议您在Kubernetes集群的Node子网中创建文件存储；

2. 文件存储支持通过挂载目标的IP地址挂载文件存储，您可以在文件存储详情页查询挂载目标的IP地址，详情参考[文件存储信息](https://docs.jdcloud.com/cn/cloud-file-service/file-system-detail)。

## 二、Pod通过NFS Volume方式使用CFS文件存储
    
1. 新建一个Pod，将第一部分创建的CFS文件存储通过NFS Volume方式挂载到Pod，并在CFS目录下写入文件。Pod YAML文件说明如下：
```
kind: Pod
apiVersion: v1
metadata:
  name: write-data-to-cfs
spec:
  volumes:
  - name: cfs
    nfs:
      path: /           #请使用挂载目标支持的目录替换，默认挂载到根目录
      server: 172.XX.XX.15          #请使用文件存储的挂载目标IP地址替换
      readOnly: false # default false
  containers:
  - args:
    - /bin/sh
    - -c
    - 'while true; do ts=`date +%s`; echo "${ts} hello world" >> /mnt/cfs-write/hello.log; sleep 1; done'
    image: busybox
    imagePullPolicy: Always
    name: busybox
    volumeMounts:
    - name: cfs
      mountPath: /mnt/cfs-write       #您可以根据项目情况修改CFS的挂载目录
  restartPolicy: Never
```     
**参数说明：**

* 上述YAML文件将CFS挂载目标的"/"目录挂载到Pod的/mnt/cfs-write目录；
* 您可以执行如下命令下载实例Yaml文件：

`
wget https://kubernetes.s3.cn-north-1.jdcloud-oss.com/CFS/write-data-to-cfs.yml
`

* 创建Pod前，请根据文件存储信息修改Yaml文件中对应参数值。

2. 使用Kubectl客户端[连接到Kubernetes集群](https://docs.jdcloud.com/cn/jcs-for-kubernetes/connect-to-cluster)后，执行如下命令创建Pod：

```
kubectl create -f write-data-to-cfs.yml       # 文件名称write-data-to-cfs.yml可使用本地目录保存的文件替换
```

3. 执行如下命令，确定Pod运行状态
```
kubectl get pod 
NAME                                          READY   STATUS                       RESTARTS   AGE
write-data-to-cfs                             1/1     Running                      0          7d
```

4. 执行如下命令进入Pod，查看/mnt/cfs-write/hello.log文件中保存的内容：
```
Kubectl exec -it write-data-to-cfs -- /bin/sh
/ # cat /mnt/cfs-write/hello.log

```
5. 执行如下命令删除Pod

```
kubectl delete pod write-data-to-cfs
pod "write-data-to-cfs" deleted
```
6. 重新创建一个Pod，将第一部分创建的CFS文件存储通过NFS Volume方式挂载到Pod，并验证CFS目录下的文件内容。Pod Yaml文件说明如下：
```
kind: Pod
apiVersion: v1
metadata:
  name: read-data-from-cfs
spec:
  volumes:
  - name: cfs
    nfs:
      path: /           #请使用挂载目标支持的目录替换，默认挂载到根目录
      server: 172.XX.XX.15          #请使用文件存储的挂载目标IP地址替换
      readOnly: false # default false
  containers:
  - args:
    - /bin/sh
    - -c
    - 'while true; do ls -l /mnt/cfs-read/; sleep 2; done'
    image: busybox
    imagePullPolicy: Always
    name: busybox
    volumeMounts:
    - name: cfs
      mountPath: /mnt/cfs-read       #您可以根据项目情况修改CFS的挂载目录
  restartPolicy: Never
```     
**参数说明：**

* 上述YAML文件将CFS挂载目标的"/"目录挂载到Pod的/mnt/cfs-read目录；
* 您可以执行如下命令下载实例Yaml文件：

`
wget https://kubernetes.s3.cn-north-1.jdcloud-oss.com/CFS/read-data-from-cfs.yml
`

* 创建Pod前，请根据文件存储信息修改Yaml文件中对应参数值。

7. 创建Pod，并验证Pod处于运行状态时，执行如下命令进入Pod，查看/mnt/cfs-read目录下的文件内容
```
Kubectl exec -it read-data-from-cfs -- /bin/sh
/ # cat /mnt/cfs-read/hello.log

```
8. 通过第7步的验证即可发现，名称为write-data-to-cfs的Pod在CFS中写入的文件hello.log被持久化保存到CFS文件存储，并且可以被名称为read-data-from-cfs的Pod共享。

9. 执行如下命令删除Pod

```
kubectl delete pod read-data-from-cfs
pod "read-data-from-cfs" deleted
```