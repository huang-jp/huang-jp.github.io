---
title: 大数据学习路线（完整详细版）
---
大数据学习路线

java(Java se,javaweb)
Linux(shell,高并发架构,lucene,solr)
Hadoop(Hadoop,HDFS,Mapreduce,yarn,hive,hbase,sqoop,zookeeper,flume)
机器学习(R,mahout)
Storm(Storm,kafka,redis)
Spark(scala,spark,spark core,spark sql,spark streaming,spark mllib,spark graphx)
Python(python,spark python) 
云计算平台(docker,kvm,openstack)

名词解释

一、Linux
lucene： 全文检索引擎的架构
solr： 基于lucene的全文搜索服务器，实现了可配置、可扩展并对查询性能进行了优化，并且提供了一个完善的功能管理界面。

二、Hadoop
HDFS： 分布式存储系统，包含NameNode，DataNode。NameNode：元数据，DataNode。DataNode：存数数据。
yarn： 可以理解为MapReduce的协调机制，本质就是Hadoop的处理分析机制，分为ResourceManager NodeManager。
MapReduce： 软件框架，编写程序。
Hive： 数据仓库 可以用SQL查询，可以运行Map/Reduce程序。用来计算趋势或者网站日志，不应用于实时查询，需要很长时间返回结果。
HBase： 数据库。非常适合用来做大数据的实时查询。Facebook用Hbase存储消息数据并进行消息实时的分析
ZooKeeper： 针对大型分布式的可靠性协调系统。Hadoop的分布式同步等靠Zookeeper实现，例如多个NameNode，active standby切换。
Sqoop： 数据库相互转移，关系型数据库和HDFS相互转移
Mahout： 可扩展的机器学习和数据挖掘库。用来做推荐挖掘，聚集，分类，频繁项集挖掘。
Chukwa： 开源收集系统，监视大型分布式系统，建立在HDFS和Map/Reduce框架之上。显示、监视、分析结果。
Ambari： 用于配置、管理和监视Hadoop集群，基于Web，界面友好。

三、Cloudera
Cloudera Manager： 管理 监控 诊断 集成
Cloudera CDH：(Cloudera's Distribution，including Apache Hadoop) Cloudera对Hadoop做了相应的改变，发行版本称为CDH。
Cloudera Flume： 日志收集系统，支持在日志系统中定制各类数据发送方，用来收集数据。
Cloudera Impala： 对存储在Apache Hadoop的HDFS，HBase的数据提供直接查询互动的SQL。
Cloudera hue： web管理器，包括hue ui，hui server，hui db。hue提供所有CDH组件的shell界面的接口，可以在hue编写mr。

四、机器学习/R
R： 用于统计分析、绘图的语言和操作环境，目前有Hadoop-R
mahout： 提供可扩展的机器学习领域经典算法的实现，包括聚类、分类、推荐过滤、频繁子项挖掘等，且可通过Hadoop扩展到云中。

五、storm
Storm： 分布式，容错的实时流式计算系统，可以用作实时分析，在线机器学习，信息流处理，连续性计算，分布式RPC，实时处理消息并更新数据库。
Kafka： 高吞吐量的分布式发布订阅消息系统，可以处理消费者规模的网站中的所有动作流数据（浏览，搜索等）。相对Hadoop的日志数据和离线分析，可以实现实时处理。目前通过Hadoop的并行加载机制来统一线上和离线的消息处理
Redis： 由c语言编写，支持网络、可基于内存亦可持久化的日志型、key-value型数据库。

六、Spark
Scala： 一种类似java的完全面向对象的编程语言。
jblas： 一个快速的线性代数库（JAVA）。基于BLAS与LAPACK，矩阵计算实际的行业标准，并使用先进的基础设施等所有的计算程序的ATLAS艺术的实现，使其非常快。
Spark： Spark是在Scala语言中实现的类似于Hadoop MapReduce的通用并行框架，除了Hadoop MapReduce所具有的优点，但不同于MapReduce的是job中间输出结果可以保存在内存中，从而不需要读写HDFS，因此Spark能更好的适用于数据挖掘与机器学习等需要迭代的MapReduce算法。可以和Hadoop文件系统并行运作，用过Mesos的第三方集群框架可以支持此行为。
Spark SQL： 作为Apache Spark大数据框架的一部分,可用于结构化数据处理并可以执行类似SQL的Spark数据查询
Spark Streaming： 一种构建在Spark上的实时计算框架，扩展了Spark处理大数据流式数据的能力。
Spark MLlib： MLlib是Spark是常用的机器学习算法的实现库，目前(2014.05)支持二元分类，回归，聚类以及协同过滤。同时也包括一个底层的梯度下降优化基础算法。MLlib以来jblas线性代数库，jblas本身以来远程的Fortran程序。
Spark GraphX： GraphX是Spark中用于图和图并行计算的API，可以在Spark之上提供一站式数据解决方案，可以方便且高效地完成图计算的一整套流水作业。
Fortran： 最早出现的计算机高级程序设计语言，广泛应用于科学和工程计算领域。
BLAS： 基础线性代数子程序库，拥有大量已经编写好的关于线性代数运算的程序。
LAPACK： 著名的公开软件，包含了求解科学与工程计算中最常见的数值线性代数问题，如求解线性方程组、线性最小二乘问题、特征值问题和奇异值问题等。
ATLAS： BLAS线性算法库的优化版本。
Spark Python： Spark是由scala语言编写的，但是为了推广和兼容，提供了java和python接口。

七、Python
Python: 一种面向对象的、解释型计算机程序设计语言。

八、云计算平台
Docker： 开源的应用容器引擎
kvm： (Keyboard Video Mouse)
openstack：  开源的云计算管理平台项目