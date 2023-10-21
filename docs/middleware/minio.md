



https://min.io/docs/minio/linux/operations/install-deploy-manage/deploy-minio-multi-node-multi-drive.html

## 部署MinIO



#### 📒 先绝条件

1. 关闭网络、防火墙



#### 主机名

创建服务器池时， MinIO*需要*使用扩展符号`{x...y}`来表示一系列连续的 MinIO 主机。MinIO 支持使用一系列连续的主机名*或*IP 地址来表示部署中的每个进程。



分布式部署

> MinIO 不支持分布式部署的非连续主机名或 IP 地址。

172.32.165.80

172.32.165.81

172.32.165.82



#### 💽 存储

MinIO 强烈建议使用带有 XFS 格式磁盘的直连JBOD 阵列，以获得最佳性能。

以使用扩展符号指定整个驱动器范围 `/mnt/disk{1...4}`。

如果要使用每个驱动器上的特定子文件夹，请将其指定为`/mnt/disk{1...4}/minio`。





#### 时间同步



### 部署分布式MinIO



#### 安装 MinIO 二进制文件

```
wget https://dl.min.io/server/minio/release/linux-amd64/minio
chmod +x minio
sudo mv minio /usr/local/bin/
```



#### 创建`systemd`服务文件

以下[systemd](https://www.freedesktop.org/wiki/Software/systemd/)服务文件安装到`/usr/lib/systemd/system/minio.service`. 对于二进制安装

/usr/lib/systemd/system/minio.service

```sh
[Unit]
Description=MinIO
Documentation=https://min.io/docs/minio/linux/index.html
Wants=network-online.target
After=network-online.target
AssertFileIsExecutable=/usr/local/bin/minio

[Service]
WorkingDirectory=/usr/local

User=minio-user
Group=minio-user
ProtectProc=invisible

EnvironmentFile=-/etc/default/minio
ExecStartPre=/bin/bash -c "if [ -z \"${MINIO_VOLUMES}\" ]; then echo \"Variable MINIO_VOLUMES not set in /etc/default/minio\"; exit 1; fi"
ExecStart=/usr/local/bin/minio server $MINIO_OPTS $MINIO_VOLUMES

# MinIO RELEASE.2023-05-04T21-44-30Z adds support for Type=notify (https://www.freedesktop.org/software/systemd/man/systemd.service.html#Type=)
# This may improve systemctl setups where other services use `After=minio.server`
# Uncomment the line to enable the functionality
# Type=notify

# Let systemd restart this service always
Restart=always

# Specifies the maximum file descriptor number that can be opened by this process
LimitNOFILE=65536

# Specifies the maximum number of threads this process can create
TasksMax=infinity

# Disable timeout logic and wait until process is stopped
TimeoutStopSec=infinity
SendSIGKILL=no

[Install]
WantedBy=multi-user.target

# Built for ${project.name}-${project.version} (${project.name})
```

默认情况下，该`minio.service`文件作为`minio-user`用户和组运行。`groupadd`您可以使用和命令创建用户和组`useradd` 。

```sh
groupadd -r minio-user
useradd -M -r -g minio-user minio-user

#单驱动
chown minio-user:minio-user /mnt/data
#多驱动
chown minio-user:minio-user /mnt/data1 /mnt/data2
```

#### 创建服务环境文件

创建一个环境文件`/etc/default/minio`

示例:

```
172.32.165.80 minio-01.example.net
172.32.165.81 minio-02.example.net
172.32.165.82 minio-03.example.net
```

所有主机都有1个驱动器

```
/mnt/data1
```

该部署有一个运行的负载均衡器，`https://minio.example.net` 用于管理所有 三 台 MinIO 主机之间的连接。

```sh
MINIO_VOLUMES="http://minio{1...3}.example.com:9000/mnt/data1"
MINIO_VOLUMES="http://minio{1...2}.example.com:9000/mnt/data{1...2}"
MINIO_OPTS="--console-address :9001"

# Set the root username. This user has unrestricted permissions to
# perform S3 and administrative API operations on any resource in the
# deployment.
#
# Defer to your organizations requirements for superadmin user name.

MINIO_ROOT_USER=minioadmin

# Set the root password
#
# Use a long, random, unique string that meets your organizations
# requirements for passwords.

MINIO_ROOT_PASSWORD=minioadmin

# Set to the URL of the load balancer for the MinIO deployment
# This value *must* match across all MinIO servers. If you do
# not have a load balancer, set this value to to any *one* of the
# MinIO hosts in the deployment as a temporary measure.
MINIO_SERVER_URL="http://minio.example.net:9000"
```









---

## 📔Docs

### 🎯对象存储要点

三种主要存储类型 - 文件、块和对象、核心存储概念、每种存储类型的成本注意事项以及关键用

它与 Amazon S3 兼容，使其成为在 Kubernetes 上部署存储基础设施的绝佳选择。





##### 1）块存储基础知识 

块：将数据视为 固定块的大小，并且每个文件分布在多个块中，可以调整块的大小满足存储内容的需求。

如果正在使用某个类型的 数据库，其中该数据库的输出的标准块大小可能是16KB、但是标准磁盘的大小是4，可以通过匹配并且相同硬件上获得更高的吞吐量来优化它。

块不需要存储在一起，可以进行排序，以提供最佳的性能，通常是通过一些软件，而不是自己安排这些块。都是在幕后完成的。 

然而，处理元数据的能力有限，因此可以获得文件名和其他的信息，但这里实际上没有任何的可搜索的元数据，这在以后变得很重要。

它具有很强的一致性，并且在块级别具有高度结构化，这意味着如果正在写入单个目标设备，并且该设备没有任何问题，则数据本身不太可能被损坏，如果丢失单个存储设备，显然该数据将会丢失。

但块存储本身具有很强的弹性，块存储的检索方式是，使用`ISCSI` 、`光纤` 、串行、SATA 等常见方法作为块索引，然后重新组装为编写的任何文件

 

##### 2） 文件存储基础知识 

文件作为一个整体存储，并以它们存储在文件夹文件路径中的原始格式进行访问。

例如：使用驱动器，文件的路径等，存储了有限的元数据。

+ 创建、修改日期
+ 文件大小
+ 文件属性、和其他的一些属性（例如权限）

但是同样，仅根据元数据并不能完全搜索它

文件存储通常位于块存储和对象存储之上，例如使用SMB、NFS等，因此会将这些附加的协议视为开销。  



##### 3） 对象存储基础知识

 在对象存储，文件存储在带有元数据、对象ID和附加属性的分布式分片上。

因此：

+ 谁对该文件有什么操作权限（打开、下载、修改等）注入此类的事情
+ 可以附加无限的元素，这将允许更高级的搜索功能

minio中可以附加有限的元数据，许多S3 或者对象存储的提供商确实限制了可以添加到文件的元数据量。

文件无法锁定，但是可以打开版本控制保持数据的完整性。

无限可扩展性的概念： 只需要不断的添加后端，无论位于前端的哪个位置，就可以处理所有这些问题，远离最终用户，对于最终的用户来说，你几乎没有进行任何修改或必须执行任何其他操作，后端只需要使用常见的硬件来扩展集群，扩展到需要的范围。

使用通用的 HTTP 协议通过 API 进行访问，可以使用它的地方，它可以从客户端、服务器、晕节点、以及类似的东西。应用程序存储和所有这些东西使用。

因此它是一个非常强大的工具，也是非常有效存储大量数据的好方法。



### ⚠️影响存储性能因素

注意一些可能影响存储性能的因素很重要

1. 目标磁盘速度
2. 硬件规格
3. 网路速度
4. 协议开销
5. 协议可扩展性
6. 介质搜索能力的限制

大多数存储的速度和底层硬件存储介质允许的速度一样快

+ 从硬件角度：只能允许以底层存储实际允许的速度运行

因此：还需要注意，通过利用磁盘集群来提高性能，在磁盘集群中，将多个磁盘进行组合在一起，以获得所有吞吐量的总和，意味着只需要添加多硬件即可提高重写速度。使用某种方法将其捆绑在一起，对于用户来说看起来就像一个更大的磁盘。

 如果不直接连接到系统， 可能会有一些限制，但通常是直接连接时，非常快速的链接到应用程序或者尝试访问磁盘的任何内容

+ 如果涉及网络流量：网路链接速度将成为一个考虑因素

例如，有一个存储阵列，它的速度可以达到每秒 10GB、但是

有一个网路长度只有每秒 1千KB 不匹配，无法实现全部的吞吐量。



块存储

+ 通常需要在其之上应用文件系统

 	作为原始存储，然后该文件系统将具有该文件系统的其他特性（NTFS、XFS、ZFS之类）



文件存储：



























