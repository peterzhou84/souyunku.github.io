---
layout: post
title: 分布式和集群区别？什么是云计算平台？分布式的应用场景？
categories: Distributed
description:  分布式和集群区别？什么是云计算平台？分布式的应用场景？
keywords: Distributed
---

分布式是指将一个业务拆分不同的子业务，分布在不同的机器上执行，集群是指多台服务器集中在一起，实现同一业务，可以视为一台计算机，一个云计算平台，就是通过一套软件系统把分布式部署的资源集中调度使用。要应对大并发，要实现高可用，既需要分布式，也离不开集群。

# 分布式和集群区别？

## 分布式

**分布式**：是指将一个业务拆分不同的子业务，分布在不同的机器上执行。

常用的分布式就是在负载均衡服务器后加一堆web服务器，然后在上面搞一个缓存服务器来保存临时状态，后面共享一个数据库，

如图所示:

![ ][1]

这种环境下真正进行分布式的只是web server而已，并且web server之间没有任何联系，所以结构和实现都非常简单。

## 集群

**集群**：是指多台服务器集中在一起，实现同一业务，可以视为一台计算机。

多台服务器组成的一组计算机，作为一个整体存在，向用户提供一组网络资源，这些单个的服务器就是集群的节点。

## 两个特点

**可扩展性**：集群中的服务节点，可以动态的添加机器，从而增加集群的处理能力。

**高可用性**：如果集群某个节点发生故障，这台节点上面运行的服务，可以被其他服务节点接管，从而增强集群的高可用性。

## 集群分类

常用的集群分类

**1.高可用集群**(High Availability Cluster)

高可用集群,普通两节点双机热备，多节点HA集群。

**2.负载均衡集群**(Load Balance Cluster) 

常用的有 Nginx 把请求分发给后端的不同web服务器，还有就是数据库集群，负载均衡就是，为了保证服务器的高可用，高并发。  

**3.科学计算集群**(High Performance Computing Cluster)  

简称HPC集群。这类集群致力于提供单个计算机所不能提供的强大的计算能力。  

## 两大能力

**负载均衡**：负载均衡能把任务比较均衡地分布到集群环境下的计算和网络资源。

**集群容错**：当我们的系统中用到集群环境,因为各种原因在集群调用失败时，集群容错起到关键性的作用。

例如 Dubbo 的集群容错：

**Failover Cluster**  

失败自动切换，当出现失败，重试其它服务器，通常用于读操作，但重试会带来更长延迟。  

**Failfast Cluster**  

快速失败，只发起一次调用，失败立即报错，通常用于非幂等性的写操作，比如新增记录。  

**Failback Cluster**  

失败自动恢复，后台记录失败请求，定时重发，通常用于消息通知操作。
  
**Forking Cluster**  

并行调用多个服务器，只要一个成功即返回，通常用于实时性要求较高的读操作，但需要浪费更多服务资源。  

## 简单总结

分布式，从狭义上理解，也与集群差不多，但是它的组织比较松散，不像集群，有一定组织性，一台服务器宕了，其他的服务器可以顶上来。

分布式的每一个节点，都完成不同的业务，一个节点宕了，这个业务就不可访问了。

**1. 分布式是指将一个业务拆分不同的子业务，分布在不同的机器上执行。** 

**2. 集群是指多台服务器集中在一起，实现同一业务，可以视为一台计算机。** 

分布式的每一个节点，都可以用来做集群。而集群不一定就是分布式了。  

# 什么是云计算平台？

一个云计算平台，就是通过一套软件系统把分布式部署的资源集中调度使用。要应对大并发，要实现高可用，既需要分布式，也离不开集群。

比如负载均衡，如果只是一台服务器，这台宕机了就完蛋了。

分布式的难点，就是很多机器做存在依赖关系的不同活儿，这些活儿需要的资源、时间区别可能很大，某些机器还可能罢工，要怎么样才能协调好，做到效率最高，消耗最少，不出错。

# 分布式的应用场景？

平时接触到的分布式系统有很多种，比如分布式文件系统，分布式数据库，分布式WebService，分布式计算等等，面向的情景不同，但分布式的思路是否是一样的呢?

## 1.简单的例子

假设我们有一台服务器，它可以承担1百万/秒的请求，这个请求可以的是通过http访问网页，通过tcp下载文件，jdbc执行sql，RPC调用接口…，现在我们有一条数据的请求是2百万/秒，很显然服务器hold不住了，会各种拒绝访问，甚至崩溃，宕机，怎么办呢。

一台机器解决不了的问题，那就两台。所以我们加一台机器，每台承担1百万。如果请求继续增加呢，两台解决不了的问题，那就三台呗。

这种方式我们称之为**水平扩展**。如何实现请求的平均分配便是**负载均衡了**。

另一个栗子，我们现在有两个数据请求，数据1 90万，数据2 80万，上面那台机器也hold不住，我们加一台机器来负载均衡一下，每台机器处理45万数据1和40万数据2，但是平分太麻烦，不如一台处理数据1，一台处理数据2，同样能解决问题，这种方式我们称之为**垂直拆分**。

**水平扩展和垂直拆分**是分布式架构的两种思路，但并不是一个二选一的问题，更多的是兼并合用。下面介绍一个实际的场景。这也是许多互联网的公司架构思路。

## 2.实际的例子

我此时所在的公司的计算机系统很庞大，自然是一个整的分布式系统，为了方便组织管理，公司将整个技术部按业务和平台拆分为部门，订单的，会员的，商家的等等，每个部门有自己的web服务器集群，数据库服务器集群，通过同一个网站访问的链接可能来自于不同的服务器和数据库，对网站及底层对数据库的访问被分配到了不同的服务器集群,这个便是典型的按业务做的**垂直拆分**，每个部门的服务器在hold不住时，会有弹性的扩展，这便是**水平扩展**。

在数据库层，有些表非常大，数据量在亿级，如果只是纯粹的水平的扩展并不一定最好，如果对表进行拆分，比如可以按用户id进行水平拆表，通过对id取模的方式，将用户划分到多张表中，同时这些表也可以处在不同的服务器。按业务的垂直拆库和按用户**水平拆表**是分布式数据库中通用的解决方案。

比如 Mycat 开源分布式数据库中间件 [http://www.mycat.io/](http://www.mycat.io/)

## 3.分布式一致性

分布式系统中，解决了负载均衡的问题后，另外一个问题就是数据的一致性了，这个就需要通过同步来保障。根据不同的场景和需求，同步的方式也是有选择的。

在分布式文件系统中，比如商品页面的图片，如果进行了修改，同步要求并不高，就算有数秒甚至数分钟的延迟都是可以接受的，因为一般不会产生损失性的影响，因此可以简单的通过文件修改的时间戳，隔一定时间扫描同步一次，可以牺牲一致性来提高效率。

但银行中的分布式数据库就不一样了，一丁点不同步就是无法接受的，甚至可以通过加锁等牺牲性能的方式来保障完全的一致。

在一致性算法中paxos算法是公认的最好的算法，Chubby、ZooKeeper 中Paxos是它保证一致性的核心。这个算法比较难懂，我目前也没弄懂，这里就不深入了。

[1]: http://www.ymq.io/images/2018/Distributed/transaction-cluster/1.png

# Contact

 - 作者：鹏磊  
 - 出处：[http://www.ymq.io/2018/01/23/Distributed-cluster/](http://www.ymq.io/2018/01/23/Distributed-cluster/)  
 - Email：[admin@souyunku.com](admin@souyunku.com)  
 - 版权归作者所有，转载请注明出处
 - Wechat：关注公众号，搜云库，专注于开发技术的研究与知识分享
 
![关注公众号-搜云库](http://www.ymq.io/images/souyunku.png "搜云库")
