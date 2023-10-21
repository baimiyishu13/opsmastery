## 📒doc



### 👀 What kafka？

Apache Kafka 是一个开源分布式事件流平台

[官网](https://kafka.apache.org/#:~:text=Apache%20Kafka%20is%20an%20open,%2C%20and%20mission%2Dcritical%20applications.)

用于高性能数据管道、流分析、数据集成和关键任务应用程序。





# 🧪 deploy

## 🧱 二进制部署kafka集群（企业级）

### 1. 主机规划

| ***\*IP\****  | ***\*broker id\**** | ***\*版本\****   |
| ------------- | ------------------- | ---------------- |
| 10.232.23.173 | 0                   | kafka_2.12-3.5.1 |
| 10.232.23.143 | 1                   |                  |
| 10.232.23.158 | 2                   |                  |

### 2. 安装 Java 8+

- Kafka二进制包下载地址 [Download](https://kafka.apache.org/downloads)
- jdk-8u371-linux-x64.tar.gz [Download](https://javadl.oracle.com/webapps/download/AutoDL?BundleId=248219_ce59cff5c23f4e2eaf4e778a117d4c5b)

> **注意：环境必须安装 Java 8+。**

登录harbor机器,使用ansible拷贝到kafka节点

```sh
- name: cp file
  hosts: test2
  become: true
  tasks:
    - name: Copy files
      copy:
        src: "/home/swdys/soft/{{ item }}"
        dest: "/home/wlznhpt/"
      loop:
        - jdk-8u371-linux-x64.tar.gz
        - kafka_2.12-3.5.1.tgz
```

- 解压：`jdk-8u371-linux-x64.tar.gz`

```
mkdir /usr/java
tar -zxf jdk-8u371-linux-x64.tar.gz -C  /usr/java
```

- 编辑 `/etc/profile` 配置文件，文件末尾添加如下配置：

```sh
export JAVA_HOME=/usr/java/jdk1.8.0_371 #java解压目录
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
export PATH=$PATH:${JAVA_PATH}
```

- 让配置生效 `source /etc/profile`
- 查看是否安装成功：`java -version`

### 3. 配置KAFKA

下载 Kafka 版本并解压:

```sh
tar -xzf kafka_2.13-3.5.1.tgz
```

编辑 `config/zookeeper.properties`配置文件：

```sh
dataDir=/kafka/zookeeper/data
dataLogDir=/kafka/zookeeper/log

clientPort=2181

# maxClientCnxns=0

#为zk的基本时间单元，毫秒
tickTime=2000
#Leader-Follower初始通信时限 tickTime*10
initLimit=10
#Leader-Follower同步通信时限 tickTime*5
syncLimit=5

# admin.enableServer=false
# admin.serverPort=8080

#设置broker Id的服务地址
server.0=10.232.23.173:2888:3888
server.1=10.232.23.143:2888:3888
server.2=10.232.23.158:2888:3888
```

- 创建好对于目录和文件：

```sh
 mkdir /kafka/zookeeper/data -p
 mkdir /kafka/zookeeper/log -p
 echo "0" > /kafka/zookeeper/data/myid # zk集群id 注意：zk集群id 每个节点唯一
```

编辑 `kafka `配置文件 `config/server.properties`

```sh
broker.id=0 # 需要修改为集群唯一
listeners=PLAINTEXT://10.232.23.173:9092 # 填节点地址
zookeeper.connect=10.232.23.173:2181,10.232.23.143:2181,10.232.23.158:2181 # 集群节点IP地址
```

### 4. 启动KAFKA

运行以下命令以便以正确的顺序启动所有服务：

```sh
# Start the ZooKeeper service
$ bin/zookeeper-server-start.sh -daemon config/zookeeper.properties; echo $?
# Start the Kafka broker service
$ bin/kafka-server-start.sh -daemon config/server.properties; echo $?
```

所有服务成功启动后，将拥有一个正在运行并可供使用的基本 Kafka 环境。

测试：

```sh
$ bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092

$ bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
Topic: quickstart-events        TopicId: NPmZHyhbR9y00wMglMH2sg PartitionCount: 1       ReplicationFactor: 1    Configs:
    Topic: quickstart-events Partition: 0    Leader: 0   Replicas: 0 Isr: 0

$ bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
This is my first event
This is my second event

$ bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
This is my first event
This is my second event

$ bin/kafka-topics.sh --delete --topic quickstart-events --bootstrap-server localhost:9092
```