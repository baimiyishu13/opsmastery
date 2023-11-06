# 自我介绍

​		谢谢您邀请我参加这次面试，我的名字是马乐乐，我拥有计算机科学与技术的学位，并且修读了虚拟化/云计算的课程

​		我所做的对我职业生涯真正有效的一件事就是能够将网路工程师转为运维工程师，所以我在转为运维之前，我在网路工程上学习和实习了投入了很多，我喜欢它也做的很好，也参加了一些网路比赛，获得华为、国家计算机网络的认证资质。

​		但是后来转向了一个新的职业，或者只是对它充满热情，但这就是我最近两年一直在做的事情。从事运维工程师近两年，当我在运维部门工作， 我已经能够学习如何做好运维工作，我还为我的同事和我的经理生成自动化任务完成的无聊重复性任务，管理/部署k8s集群、在k8s上发布业务、负责k8s集群的监控等等。并且获得了kubernetes CKA/CKS等认证，这些也是我的技能。

​		虽然我喜欢在目前的职位上工作，但我觉得经过这一年的工作，已经在这家公司取得了很大的成就。我被这个职位所吸引，在IT领域众所周知的一家声誉极佳的公司，我想这是一个非常不错的职位，我有相关技能有经验，因此我看到的是一个快速发展的组织，也是我真正能够做出积极贡献的地方。这就是我为什么来这里接受面试，		再次谢谢你邀请。

我相信你也想和雄心勃勃的人一起工作，这就是我为什么想和你一起工作，

---

离职原因：

​		虽然我喜欢在目前的职位上工作，但我觉得经过这一年的工作，已经在这家公司取得了很大的成就，所以当我在boss上看到你们的招聘信息后，我想这是一个非常不错的职位，我有相关技能有经验，这是一家声誉极佳的公司，也是我真正能够做出积极贡献的地方。

---











# 🔭 prometheus

普罗米修斯面试题及答案

**prometheus 服务发现有哪些**

```
静态配置：Prometheus支持静态配置，其中你手动指定要监控的目标
动态配置：Prometheus还支持动态服务发现，其中它可以自动发现并监控新添加的目标，而无需手动配置。这通常通过诸如Consul、Kubernetes
Consul服务发现：如果有经验，可以提到使用Consul进行服务发现。Consul是一种用于服务注册和发现的工具，Prometheus可以与Consul集成，以实现动态服务发现。
Kubernetes服务发现：如果你在Kubernetes环境中工作过，可以讨论如何使用Prometheus的Kubernetes服务发现插件来发现和监控Kubernetes中的Pod和服务。
Zookeeper服务发现：Zookeeper是另一种服务发现工具，Prometheus可以与之集成。
```



1. ##### 普罗米修斯是什么？

```
免费开源的时事监控软件，将获取到的指标转意后，通过HTTP PUSH记录在TSDB时间序列数据库中，提供的API接口或者UI查询接口通过PromQL查询指标。
```

1. ##### Prometheus监控的架构是怎样的？

```
1.prometheus service : retrieval、TSDB、HTTP server
2.Exporter
3.pushgateway
4.service discover
5.prometheus altermanage
6.UI - grafanna
```

1. ##### 我们如何使用 Prometheus 管理和监控生产中的 Spring boot 应用程序？

```
安装Exporter、配置Prometheus拉取指标，通过Grafana展示
```

1. ##### 普罗米修斯有什么特点？

```
1.原生提供的Exporter
2.完美的集成Grafana、很好的展示、同时有大量的模版
3.可以通过PromQL查询指标
4.不依赖分布式存储，可以直接使用本服务器磁盘
5.时间序列数据库,建议是本地高性能磁盘（还可以与远程存储系统集成）
```

1. ##### 普罗米修斯有哪些组件？

```
1.prometheus service : retrieval、TSDB、HTTP server
2.Exporter
3.pushgateway
4.service discover
5.prometheus altermanage
6.UI - grafanna
```

1. ##### Prometheus 使用什么数据库？

```
时间序列数据库 TSDB
```

2. Prometheus 表达式语言中有哪些不同的 PromQL 数据类型？

```
1.仪表盘：可上下变化，入CPU、内存、网路延迟等
2.计数器：只增不减：请求数、错误数等等
3.直方图：响应大小、请求持续时长
4.摘要 summary
```

1. ##### 如何根据直方图或摘要计算过去 5 分钟的平均请求持续时间？

```
rate(**[5m])
```

1. ##### Prometheus 中默认的数据保留期限是多少？

```
默认保留15天
```

1. ##### 如何检查我的 Prometheus 状态？

```
curl http://localhost:9090
```

1. ##### 如何触发 Prometheus 警报？

```sh
从 Prometheus 服务器接收警报并通过电子邮件、Slack 或其他方式将它们发送给最终用户。
下是设置 Prometheus 警报的步骤：

配置并设置 AlertManager。
配置 Prometheus 的配置文件以允许其与 AlertManager 通信。
在Prometheus服务器配置中，定义警报规则。
在 AlertManager 中，创建警报机制以通过 Slack 和电子邮件发送警报。
```

1. ##### 什么是普罗米修斯导出器？

```
exporter：获取目标信息，将指标暴露在 默认的 /metrics接口下
```



# 🎯 日志

采集方法：

1. 使用daemonset 在每个节点部署 日志记录代理收集这些日志
2. 专门为容器注入一个记录日志的（Sidecar）-Fluentd
3. 将日志直接从应用程序中推送到日志记录后端 EFK，缺点：比较重，优点比较成熟
4. PLG：  grafana loki promtail - 使用logql查询



重点1：EFK



重点2： PLG 

在 Loki 架构中有以下几个概念：

- Grafana：相当于 EFK 中的 Kibana ，用于 UI 的展示。
- Loki：相当于 EFK 中的 ElasticSearch ，用于存储日志和处理查询。
- Promtail：相当于 EFK 中的 Filebeat/Fluentd ，用于采集日志并将其发送给 Loki 。
- LogQL：Loki 提供的日志查询语言，类似 Prometheus 的 PromQL，而且 Loki 支持 LogQL 查询直接转换为 Prometheus 指标。

日志数据可以存储在 `本地文件系统` / `S3`



# CNI插件

Flannel：采用UDP Overlay、主要是2层（vxlan）

Calico：3层方案，不需要Overlay，使用BGP路由

Cilium：高性能网路

有数十种可用于 Kubernetes 的 CNI，但它们的功能、规模和性能差异很大。其中许多依赖于传统技术（iptables），该技术无法处理 Kubernetes 环境的规模和变动，从而导致延迟增加和吞吐量降低。大多数 CNI 也仅提供对 L3/L4 Kubernetes 网络策略的支持，但除此之外几乎没有其他支持。许多云提供商都有自己的自定义 CNI



Cilium 的数据平面使用 eBPF 进行高效的负载平衡和增量更新

而不必担心网络成为瓶颈。

Cilium 可以通过 BGP 吸引流量，并利用 XDP 和 eBPF 加速流量

份工作做过一些效率改进的工作，pipeline，k8s改造，有断断续续的学习容器相关内容；日常k8s升级；



它管理网络和应用程序协议层的连接，以更高的效率处理 IP、TCP、UDP、HTTP、Kafka、gRPC 和 DNS 等协议。

虽然 Ingress API 支持基于路径和主机规则的基本路由，但它缺乏对高级路由功能的支持，仅支持 HTTP 和 HTTPS 流量，





它支持流量拆分、标头修改和 URL 重写等功能

Hubble 可以轻松地自动发现 Kubernetes 集群中 L3/L4 和 L7 级别的服务依赖关系









例如，可能很难确定 DNS 是否正常工作，或者应用程序是否由于策略相关问题而失败



您可以给真正想

我的表现将根据什么















