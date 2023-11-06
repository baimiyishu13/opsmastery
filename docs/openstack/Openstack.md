# 📒 Doc

> Openstack

提供了云计算和开放基础设施的强大功能，



Openstack：网路

当涉及到无力网路时，传统上拥有交换机、路由器和其他网路设置。

当谈到Openstack的网路时候，这些概念并没有那么的不同，仍然拥有相同的组件，但是这里区别在于组件的虚拟化版本，软件定义的网路。



默认情况下创建的所有内容都是私有的，包括对象存储、计算实例。

安全组：ssh时登陆服务器的方式，如果攻击者能够闯入这些服务器将造成巨大威胁，确保至少被少量锁定。

安全组：控制能够访问的实例的内容，视为一种形式的防火墙



构建私有网路：

1. 创建一个网路
2. 创建子网（实际设置网路IP信息）

 与无力网路没有任何区别，总是需要一个路由器，满足要求

3. 创建一个路由器（名称、外部网路-运营商网路（视为下一跳））
   1. 创建  interfaces 匹配步骤一网路
   2. 网关IP

 

 