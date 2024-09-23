## Kubernetes/k8s 入门与进阶实践（46讲）--- 徐伟亮

### 1.k8s的简介（第一章 k8s快速入门）

#### 1.1k8s是什么

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920155807.png)![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920155911.png)

#### 1.2为什么需要k8s

​	应用扩展：垂直扩缩容---服务器节点扩缩容、水平扩缩容---副本控制器扩缩容、弹性伸缩---自动扩缩容副本、服务发现和负载均衡---流量调度、存储编排---自动挂载存储类系统、自动部署和回滚---版本滚动更新及版本回滚、自我修复---故障转移、密钥与配置管理---密码的统一管理与使用

![3](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920160948.png)

#### 1.3k8s不是什么

k8s本身无需构建部署代码、不限制应用程序类型、无应用服务级别的内置服务、不要求日志、记录和监视、通过yaml实现用户需求无需配置语言工具或系统。

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920162237.png)

#### 1.4 k8s集群角色(k8s集群角色的划分)

k8s集群建立在多个物理机或虚拟机资源上，将物理机或虚拟机的资源抽象出来，组织成一个统一管理的平台。master称为控制平面，node称为数据平面。

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920163801.png)

##### registry镜像仓库

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920165329.png)

##### client客户端

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920165400.png)

### 2.Kubernetes集群组件（k8s集群组件讲解）

k8s集群是由master和node节点组成的，其中master和node是由多个组件组成的。

#### 2.1 master 节点组件及功能

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920171133.png)

#### 2.2 node节点组件及功能 

kubelet管理pod容器的生命周期，容器的启动、停止、销毁、重建都是由kubelet管理的。kubelet通过watch机制监听apiserver，kubelet接收到pod绑定到当前节点的通知，kubelet根据apiserver下发的pod.spec规范（容器名称、容器镜像、容器存储、镜像仓库获取镜像等等），通过cni网络插件获取ip地址，调用docker容器引擎运行对应的容器。

kubeproxy通过watch机制监听apiserver，通过Service和Endpoint来维护本地的iptables路由规则,应用到所有的node节点。

apiserver为service自动分配负载均衡ip地址（虚拟vip）

endpoint通过特定的方法获取到pod容器的ip地址集合（Endpoints Controller会watch Service以及pod的变化，维护对应的Endpoint信息）



![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920210606.png)

#### 2.3 ADD-ons附件组件（Kubernetes集群组件-Add-ons）

##### 2.3.1 cordns

service负载均衡与service域名映射，cordns通过watch机制监听apiserver，当service ip发生变化，cordns更新service ip与service域名映射关系

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920221825.png)

##### 2.3.2 network

网络插件CNI（flannel）根据网段信息，自动将不同的IP地址网段范围信息分配给不同节点。集群如果没有网络插件是无法实现跨主机通信的。

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920221230.png)

**pod之间跨主机通信的路由走向：**

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920221415.png)

##### 2.3.3 Dashboard

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240920221926.png)

### 4.k8s资源与对象（第二章 k8s资源与对象入门）

#### 4.1.k8s资源---pod

k8s集群中的所有ip地址都是由网络插件随机分配的

#### 4.1 pod

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/808d519305a8806e067d2eec386e244.png)

#### 4.2 k8s资源---deployment

deployment控制rs，rs控制pod副本。

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921094245.png)

#### 4.3 k8s资源---service

为了实现service作为负载均衡转发流量到后端指定的pod上，在创建pod时，给pod打上标签，创建service时，使用selector标签选择器指定选择对应标签的pod，这就实现了使用标签绑定service和一组pod的关联关系,pod加入负载均衡中。通过kube-proxy实现负载均衡的规则转发。

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921094827.png)

#### 4.4 k8s资源---namespace

![be1b3b50b692f694f672c6d6f3a2a31](C:/Users/lamp3/Documents/WeChat%20Files/wxid_5708307082921/FileStorage/Temp/be1b3b50b692f694f672c6d6f3a2a31.png)

##### 4.4.1 查看k8s默认资源归属的命名空间

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921104328.png)

##### 4.4.2 常用的命令

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921104456.png)

注：进入pod容器测试

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921105035.png)

### 5.k8s资源与对象

理解：对象指的是资源被定义的期望状态、被定义的规范。pod容器名称、pod副本、pod的重启策略、升级策略等等、pod容器的cpu、内存限制、pod运行所在的节点等等。

#### 5.1 什么是对象

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921110110.png)

#### 5.2 对象规范与状态



![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/266f2aed25cf3aa3abf4cedcb88b94b.png)

#### 5.3 理解对象

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921110805.png)

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921111017.png)

##### 5.3.1 解析yaml文件内容

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921111504.png)

#### 5.4 对象必须字段

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921111724.png)

#### 5.5 如何创建对象 

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921111809.png)

### 6.k8s资源实践（第三章 k8s资源与对象入门）

pod、deployment、service等资源的命名name可随意变更，但标签和标签选择器必须一致。

#### 6.1 部署应用（pod）

```

# deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-dp
  namespace: default
spec:
  replicas: 3           # deploy需要管理的Pod副本为3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:            # 创建的Pod的标签是什么
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:1.15.12
```

##### 6.2访问应用（service）

```
apiVersion: v1
kind: Service
metadata:
  name: app-service     # 这个名称会被CoreDNS解析为对应的域名
  namespace: default
spec:
  selector:             # Service的后端是哪些Pod，通过Selector选择对应的Pod
    run: app

  ports:
  - name: http
    port: 8080          # 负载均衡的端口   User--> ServiceIP+ServicePort --> PodIP+PodPort
    targetPort: 80      # 容器对外的端口
```

##### 6.2.1 service通过标签选择器关联一组pod

```
[root@k8s-master01 ~]# kubectl describe svc app-service
Name:                     app-service
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 run=app
Type:                     ClusterIP
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.96.153.204
IPs:                      10.96.153.204
Port:                     http  8080/TCP
TargetPort:               80/TCP
Endpoints:                172.16.85.216:80,172.16.58.205:80,172.16.58.206:80
Session Affinity:         None
Internal Traffic Policy:  Cluster
Events:                   <none>

```

##### 6.3 扩展多副本

##### 6.3.1 在线扩容方式扩容副本数

```
[root@k8s-master01 ~]# kubectl get deploy
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
app-deploy   3/3     3            3           13m

[root@k8s-master01 ~]# kubectl edit deployments.apps app-deploy（修改如下图显示）
[root@k8s-master01 ~]# kubectl get deploy
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
app-deploy   5/5     5            5           14m
```

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921152714.png)

##### 6.3.2 yaml文件方式扩容副本数

##### 6.3.3 命令行的方式扩容副本数

```
[root@k8s-master01 ~]# kubectl scale deployment app-deploy --replicas=5

```

##### 6.3.4 编辑yaml文件修改

测试环境可以直接从创建的yaml文件中修改内容，但生产环境不建议直接修改yaml文件。避免改错不同时间不同版本的内容。建议导出yaml文件再修改

```shell
[root@k8s-master01 ~]# kubectl get deploy app-deploy -o yaml > app-deploy_bak.yaml
[root@k8s-master01 ~]# kubectl apply -f app-deploy_bak.yaml

```

##### 6.4滚动更新

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921160359.png)

##### 6.4.1 使用命令行更新镜像

```shell
[root@k8s-master01 ~]# kubectl set image deployment app-deploy *=nginx:1.15.12 --record
Flag --record has been deprecated, --record will be removed in the future
deployment.apps/app-deploy image updated
[root@k8s-master01 ~]# kubectl get pod
NAME                          READY   STATUS        RESTARTS   AGE
app-deploy-65756544db-bnfjn   1/1     Running       0          22s
app-deploy-65756544db-vkztr   1/1     Running       0          20s
app-deploy-65756544db-vqzdc   1/1     Running       0          19s
app-deploy-7675d7c8cd-55p59   0/1     Terminating   0          9m43s
app-deploy-7675d7c8cd-9xtvb   0/1     Terminating   0          9m43s
app-deploy-7675d7c8cd-kgrq6   0/1     Terminating   0          9m43s
set                           1/1     Running       0          93m

```

##### 6.4.2 查看滚动更新情况

```
[root@k8s-master01 ~]# kubectl describe pod app-deploy-65756544db-vqzdc -n default
....................
.................
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  13m    deployment-controller  Scaled up replica set app-deploy-7675d7c8cd to 3
  Normal  ScalingReplicaSet  4m27s  deployment-controller  Scaled up replica set app-deploy-65756544db to 1
  Normal  ScalingReplicaSet  4m26s  deployment-controller  Scaled down replica set app-deploy-7675d7c8cd to 2 from 3
  Normal  ScalingReplicaSet  4m25s  deployment-controller  Scaled up replica set app-deploy-65756544db to 2 from 1
  Normal  ScalingReplicaSet  4m24s  deployment-controller  Scaled down replica set app-deploy-7675d7c8cd to 1 from 2
  Normal  ScalingReplicaSet  4m24s  deployment-controller  Scaled up replica set app-deploy-65756544db to 3 from 2
  Normal  ScalingReplicaSet  4m23s  deployment-controller  Scaled down replica set app-deploy-7675d7c8cd to 0 from 1
[root@k8s-master01 ~]#
```

##### 6.4.3 查看历史更新版本

```shell
[root@k8s-master01 ~]# kubectl get replicasets.apps -o wide
NAME                    DESIRED   CURRENT   READY   AGE   CONTAINERS      IMAGES          SELECTOR
app-deploy-65756544db   3         3         3       26m   app-container   nginx:1.15.12   pod-template-hash=65756544db,run=app
app-deploy-7675d7c8cd   0         0         0       35m   app-container   nginx:1.14      pod-template-hash=7675d7c8cd,run=app 
#已删除的旧版本

```

##### 6.4.4 回滚历史版本记录(rollout)

执行历史命令即可

```shell
[root@k8s-master01 ~]# kubectl rollout history deployment
deployment.apps/app-deploy
REVISION  CHANGE-CAUSE
2         kubectl set image deployment app-deploy *=nginx:1.15.12 --record=true
3         kubectl set image deployment app-deploy *=nginx:1.14 --record=true

```

##### 6.4.5 回滚到指定的版本(undo)

```
[root@k8s-master01 ~]# kubectl rollout undo deployment app-deploy --to-revision=2
deployment.apps/app-deploy rolled back
[root@k8s-master01 ~]#

```

### 7.k8s资源pod（第四章 k8s核心资源）

#### 7.1 pod基本概述

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921182618.png)



#### 7.1.1 为什么需要pod

所有的应用打包到同一个镜像中，镜像会非常的臃肿且业务之间的耦合度过高。这样做并不理想。pod的出现解决了生产场景的这些问题，例如：

1）不同的命名空间资源是隔离的，端口资源不再被限制，不同的命名空间下的应用服务可拥有相同的端口。

2）pod可以部署适配各类云平台的应用，通过该应用调用各类云平台接口获取数据再传送到pod中的agent。

3）pod的方式部署，保持着各应用的部署方式和原有的服务逻辑，共享相同的网络命名空间和文件系统命名空间实现同时满足强依赖的容器的紧密关联。

4）能够做到将不同团队开发的镜像部署在同一个pod中组成一个微服务，各自相互协同对外提供服务，

**业务服务需要收集日志**

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921185441.png)

**提供ssh、ftp访问容器数据的能力**

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921184148.png)

**适配不同Iaas平台的环境**

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921184308.png)

**提供软件发行版仓库**

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921190338.png)

#### 7.2 pod共享网络

mysql传统服务中优先启动，tomcat后启动。在pod容器中所有的应用都通过pause容器实现网络共享，直接使用localhost可相互通信。解决的传统部署的服务中谁先启动谁后启动的问题。

![image-20240921190834130](C:/Users/lamp3/AppData/Roaming/Typora/typora-user-images/image-20240921190834130.png)

#### 7.3 pod的共享存储

random-app容器将日期写入/apps目录，random-app容器/apps目录挂载到vlumes中， nginx容器 /usr/share/nginx/html目录挂载到vlumes中。两个容器的目录实现数据共享。

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921191427.png)

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921191427.png)

##### 7.3.1 共享存储yaml解析

**创建pod共享存储yaml文件**

```shell
apiVersion: v1
kind: Pod
metadata:
  name: pod-mutil
spec:
  volumes:                                                      # 申明Volumes共享卷
  - name: webpage                                       # 卷名称
    emptyDir: {}                                        # 卷类型

  containers:
  - name: random-app                            # 产生内容写入 /apps/index.html 文件中
    image: busybox
    command: ['/bin/sh','-c','echo "web-$(date +%F)" >> /apps/index.html &&  sleep 30']
    volumeMounts:
    - name: webpage
      mountPath: /apps

  - name: nginx-app                                     # 第二个容器读取第一个容器产生的内容，对外提供访问
    image: nginx
    volumeMounts:
    - name: webpage
      mountPath: /usr/share/nginx/html
```

**yaml解析**

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921191651.png)

### 8.pod管理方式

#### 8.1 自主式管理pod(静态pod)

pod被删除不会自我修复，被驱逐也不会自动重建，部署多个副本需要人为创建，多个pod一一对应多个yaml，人为监控删除，人为手动修复重建。极其繁琐，不适用生产环境。

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921193450.png)

##### 8.1.1 干跑导出一个静态pod yaml文件

```shell
[root@k8s-master01 ~]# kubectl run pod nginx --image=nginx --dry-run=client -o yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod
  name: pod
spec:
  containers:
  - args:
    - nginx
    image: nginx
    name: pod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```



##### 8.1.2 创建一个自主式pod

**查看资源与对象所需字段**

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921195704.png)

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921195715.png)

**最精简的静态pod**

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921195916.png)

**删除与驱逐pod pod不会重建**

这里不展示实验，以熟悉命令为主

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921200203.png)

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921200136.png)

#### 8.2 控制器管理pod

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921200452.png)

##### 8.3 创建一个控制器管理的pod(动态pod)

```shell
[root@k8s-master01 ~]# vim 05-pod-deployment.yaml
# deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-dp
  namespace: default
spec:
  replicas: 3           # deploy需要管理的Pod副本为3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:            # 创建的Pod的标签是什么 
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:1.16
```

**删除和驱逐pod pod自动重建**

这里不展示实验，以熟悉命令为主

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921200203.png)

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921200136.png)

### 9.pod运行阶段与状态

#### 9.1 创建一个精简的静态pod

```shell
apiVersion: v1
kind: Pod
metadata:
  name: jingjianpod
spec:
  containers:
  - name: jingjiannginx
    image: nginx
    imagePullPolicy: IfNotPresent
    ports:
      - containerPort: 80

```

#### 9.2 pod运行阶段

pending: pod为被调度或正在下载镜像

runing: 至少有一个容器启动，或处于启动或重启状态

successded: 所有容器以0状态码退出，且不会做任何操作

failed: 至少有一个容器处于非0状态码退出，pod会尝试重启并再次运行，重启策略不设置默认是会自动重启。

unkown： 无法获取pod状态，通常是与pod所在的主机失联

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921201933.png)

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921202215.png)

#### 9.2.1 describe查看pod状态详情

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921202854.png)

#### 9.2.2 condition描述的是status的具体原因

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921203123.png)

#### 9.3 容器运行阶段

结合pod和容器运行阶段存在关联关系，结合来看，更容易定位具体问题。

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921205613.png) 

#### 9.3.1 describe结合查看容器和pod的状态定位具体问题

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921210452.png)

**1) pod处于pending 容器处于waiting状态**

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921210724.png)

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921210755.png)

**2）pod运行成功 容器启动失败**

原因是pod成功之后，容器没起来，则容器的状态显示为waiting。容器命令执行错误

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921211433.png)

#### 9.3.2 总结pod和容器的状态分析

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240921212734.png)

### 10.pod运行应用对应字段 

#### 10.1 容器镜像拉取策略

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/db536a252bad26677e85c25a3175e92.png)

#### 10.2 创建镜像拉取yaml

01-pod-imagepull-policy

```
apiVersion: v1
kind: Pod
metadata:
  name: pod-image-policy
spec:
  containers:
  - name: pod-image
    image: nginx
    imagePullPolicy: Never
```

#### 10.3 获取私有仓库镜像

**使用私有仓库镜像创建pod---yaml文件 **

01-pod-imagepull-policy

```shell
apiVersion: v1
kind: Pod
metadata:
  name: pod-image-secret
spec:
  containers:
  - name: pod-image-secret
    image: registry.cn-huhehaote.aliyuncs.com/oldxu3957/nginxdemo:latest #私有镜像仓库
    imagePullPolicy: Always
 #未配置私有仓库的用户身份验证，镜像拉取失败   
```

**创建secret存放私有仓库的账号和密码**

创配置用户名、密码及url

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922111041.png)

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922111421.png)

**pod资源清单传入secrets资源**

```shell
apiVersion: v1
kind: Pod
metadata:
  name: pod-image-secret
spec:
  imagePullSecrets:
  - name: aliyun

  containers:
  - name: pod-image-secret
    image: registry.cn-huhehaote.aliyuncs.com/oldxu3957/nginxdemo:latest
    imagePullPolicy: Always
```

#### 10.4 自定义容器环境变量

**pod注入环境变量**

03-pod-mysql-env.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: pod-mysql-env
spec:
  containers:
  - name: mysql
    image: mysql:5.7.37
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: "oldxu3957"
    - name: MYSQL_DATABASE
      value: "k8s"

```

#### 10.5 自定义容器命令与参数

**适用场景：**

例如，redis主容器加入指定redis备节点的命令，redis备节点容器启动后自动加入redis集群。

例如，容器无法启动甚至查不到该容器时，可以尝试启动容器时增加sleep命令，延长启动时间，可查询更多排错信息。

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922131646.png)

注：command和args都属于覆盖容器的默认命令以达到我们期望执行的命令

##### 10.5.1 pod中定义容器sleep命令

04-pod-busybox-command.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: pod-busybox-command             # Pod名称
spec:
  containers:
  - name: busy       # 容器名称
    image: busybox
   # command: [ "/bin/sh", "-c" , "sleep 999999" ]
    command:
    - "/bin/sh"
    - "-c"
    - "sleep 909999"

```

**进入容器查看执行的命令**

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922132735.png)

##### 10.5.2 k8s中运行redis主从复制

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922133507.png)

**主redis yaml文件**

 05-pod-redis-leader-command.yaml

```
apiVersion: v1
kind: Pod
metadata:
  name: pod-redis-leader-command
spec:
  containers:
  - name: redis-leader
    image: redis
    ports:
    - containerPort: 6379


```

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922134056.png)

**从redis yaml文件**

  06-pod-redis-slave-command.yaml

````
apiVersion: v1
kind: Pod
metadata:
  name: pod-redis-slave-command
spec:
  containers:
  - name: redis-slave
    image: redis
    command: ["redis-server"]           # 主命令
    args:                               # 选项 
    - --port 6380
    - --slaveof 192.168.3.58 6379
    ports:
    - name: redis-port
      containerPort: 6380
    - name: redis-cluster
      containerPort: 16380
````

**客户端测试连接redis主从**

```shell
1.安装redis客户端
[root@k8s-master01 ~]# yum install redis

2.连接redis主节点
[root@k8s-master01 ~]# redis-cli -h 172.16.58.221
172.16.58.221:6379>

3.连接redis备节点
[root@k8s-master01 ~]# redis-cli -h 172.16.58.222 -p 6380
172.16.58.222:6380>

4.测试主从同步
[root@k8s-master01 ~]# redis-cli -h 172.16.58.221
172.16.58.221:6379> set xiaoxiao hello
OK
172.16.58.221:6379>
[root@k8s-master01 ~]# redis-cli -h 172.16.58.222 -p 6380
172.16.58.222:6380> get xiaoxiao
"hello"
172.16.58.222:6380>

```

#### 10.6 自定义容器端口

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922145243.png)

**从redis yaml文件**

  06-pod-redis-slave-command.yaml

```shell
apiVersion: v1
kind: Pod
metadata:
  name: pod-redis-slave-command
spec:
  containers:
  - name: redis-slave
    image: redis
    command: ["redis-server"]          
    args:                            
    - --port 6380
    - --slaveof 192.168.3.58 6379
    ports:
    - name: redis-port #端口名称
      containerPort: 6380 #对外暴露的端口
    - name: redis-cluster #端口名称
      containerPort: 16380 #对外暴露的端口
```

### 11. pod的重启策略

**重启策略是针对POD的重启不是针对containerd!!!**

pod重启可修复容器启动异常的问题，但如果容器中的命令是错误的，导致容器无法启动，那么pod的重启策略会出现无休止重启，除了消耗大量资源外并无任何意义。![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922150021.png)

```shell
注意：所谓的重置未初始状态值得是最初按指数增加延迟重建和启动的时间间隔。
```



#### 11.1 always

##### 11.1.1 编写一个pod的yaml文件

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922151004.png)

##### 11.1.2 正常停止容器nginx进程或kill掉节点上的容器进程，容器都会重启一次，pod又恢复正常状态。

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922151354.png)

#### 11.2 never

##### 11.2.1  编写pod的yaml文件

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922210353.png)

##### 11.2.2 无论正常或异常停止容器应用，容器不会重启应用

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922210500.png)

#### 11.3 onfailure

##### 11.3.1 编写pod的yaml文件

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922210616.png)

##### 11.3.2 模拟正常重启和异常重启

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922210806.png)

### 12.POD生命周期

#### 12.1 什么是生命周期

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922211543.png)

#### 12.2 生命周期流程

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922211552.png)

##### 12.3 生命周期总结

![](https://raw.githubusercontent.com/zoowemama1930/DMimagest/main/20240922212525.png)

### 13.init container

在主要的容器之前启动的一个人容器，重点是用来做初始化动作的。

#### 13.1 基本概念

#### 13.2 应用场景

#### 13.3 场景实践-1

1.编写yaml,适用初始化容器对mysql端口进行检查，如果存活则运行pod,否则一直重启尝试

```
apiVersion: v1
kind: Pod
metadata:
  name: pod-check-mysql
spec:
  restartPolicy: Always

  # 初始化容器
  initContainers:
  - name: check-mysql
    image: oldxu3957/tools
    command: ['/bin/bash', '-c', 'nc -z 10.0.0.206 3306']

  # 主容器
  containers:
  - name: webapps
    image: tomcat
    ports:
    - containerPort: 8080

```

