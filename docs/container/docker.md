

### 入门案例

[入门指南](https://docs.docker.com/get-started/overview/)









### 场景

---

##### 什么是docker？

 docker是一个开源的容器化平台，用于管理容器的生命周期 , 是对程序的代码、依赖项、文件、环境变量等进行打包，使能够将应用程序与基础设施分离，以便您可以快速交付软件

##### 容器和虚拟机有何不同

Docker 是轻量级的沙盒，在其中运行的只是应用，虚拟机里面还有额外的系统。

1. 跨平台而言：docker 更强
2. 性能上面，vm将使用更多的内存和CPU，性能损耗大
3. 自动化：vm需要手动安装所有东西，docker只需要一个命令
4. 稳定性上，vm不同系统存在差异，docker的稳定性更好
5. 但是在安全性上，vm更好

##### Docker常用命令

docker 【info、version、build、save、load、run、create、loag、exec、pull、push、rm、rmi、ps、login、images】

##### Docker容器有几种状态





### dockerfile

+ FROM
+ LABLE
+ RUN
+ COPY、ADD
+ CMD
+ EXPOSE
+ WORKERDIR
+ USER
+ VOLUME
+ ENTPYPOINT



网路

1. 桥接：bridge
2. host
3. none







项目：

k8s部署项目 - 九天AI平台 （九天平台是干什么的）

+ 项目功能简介
+ 技术架构
+ 此项目涉及到什么



项目中的技术

1. 微服务架构

2. 基础设施：K8s（kubeadm）+ keeplive/harporx + kubesphere 

公共服务：

2. harbor
3. prometheus
4. 负载均衡器：nginx
5. nacos

微服务网关：api-gateway

中间件：

1. mysql
2. redis
3. nacos
4. ES
5. kafka



配置文件和数据库文件怎么管理的 - nacos 配置管理/服务注册



项目流水线：

前期准备：

1. git
2. harbor
3. k8s集群的kubeconfig

后端服务：

1. helm
2. 自研的devops平台、手动发布
3. dockerfile



我在此项目中使用的技术：（重点 ）

+ harbor、kubeadm、keeplive、harporxy、kubesphere、prometheus、grafana、

整个集群部署、监控部署、kubesphere部署、业务发布、EFK、mysql、GPU exporter、ES exporter

项目中我的到经验



简历格式

1. 个人信息

2. 个人优势：我在项目中使用笔记熟的技术linux、shell、docker、k8s、ansible、prometheus、（不要写闭源产品）

3. 技术栈

4. 司历 

   1. 司龄（不要写 那年到那年）
   2. 职责
   3. 项目（业务、技术、我在想吗当中使用的技术部分、我在想吗中总结的经验）

   



​	交付工作本身就是一项非常有成就感的工作，改进事物，优化，使事情更快，自动化任务等等。从一名网络工程师，在一次机会中在接触到了这份工作，使用容器对应用程序部署，操作镜像等，或者围绕docker和k8s交付，我注意到这些内容对我来说比做网络更有趣，但是我相信意义远不止于此，看的越远就越学无止境。