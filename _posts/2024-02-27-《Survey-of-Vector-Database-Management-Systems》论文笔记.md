---
layout: post
title:  "《Survey of Vector Database Management Systems》论文笔记"
date:   2024-02-27 14:21:02 +0800
categories: Paper Notes
---

### 

##### 摘要

 	在过去的大约5年内，已经出现了超过20种商用的向量数据库管理系统。然而，基于向量的检索已经被学术界研究了几十年，相似度检索的研究也有半个多世纪的历史。而算法数据密集型应用的出现，尤其是大模型的出现推动了理论算法落地到实际系统，而这就需要能够存储海量非结构化数据的存储结构以及可靠、安全、快速可扩展的查询处理能力。因此有很多新的数据管理技术和系统应运而生，然而目前在这方面没有很全面详尽的综述。

​	首先指出在向量数据管理方面的五个主要障碍，分别为：语义相似度的模糊性，向量数据海量的规模，进行相似度比较带来的巨大花费，缺乏对索引的自然划分，以及高效处理混合查询的难度。而跨越这些障碍需要新的技术和方法来处理几个问题：查询处理，存储和索引，查询优化与执行。对于查询处理，已经有一些广为人知的相似度查询衡量距离标准和查询类型。对于存储和索引，已经存在包含向量压缩（量化），基于随机、机器学习、“导航”的分区索引。对于查询优化和执行，列举了用于混合查询的新运算符，以及一些技术：查询计划的枚举，查询计划的选择，硬件加速执行过程。这些技术构成了各种各样的向量数据库管理系统，可分为专用的向量数据库和扩展了向量检索能力的现有系统。最后概括了几个研究方向的挑战并且指出未来工作的方向。



##### 1 Introduction

大模型的兴起以及现如今不断增长的非结构化数据需要向量数据库的支持。向量数据库的结构可以被分为下图所示的结构，包含Storage Manager和Query Processor。其中Storage Manager又可以分为Search Indexes查询的索引和Vector Storage向量的存储，Query Processor可以分为Queries查询的种类，Operators，Query Optimizer和Query Executor。

![image-20240304170622392](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240304170622392.png)

##### 2 Query Processing

Query Processing需要考虑how to specify the search criteria和how to execute search queries。对于how to specify the search criteria，需要考虑的包括：similarity scores, query types。而对于how to execute search queries，需要考虑index-supported operators。

常见的similarity score包括以下几种：

汉明距离

![image-20240304205835217](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240304205835217.png)

内积

![image-20240304205935624](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240304205935624.png)

余弦相似度

![image-20240304210116156](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240304210116156.png)

明可夫斯基距离

![image-20240304210320219](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240304210320219.png)

马哈拉诺比斯距离

![image-20240304210920333](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240304210920333.png)

除此之外还有Aggregate Score，可用于多向量检索。

值得注意的是，至今没有一个严格的划分依据，各种similarity score适用于哪些应用场景，大多数时候，人们是基于经验去选择某一种similarity score。很多向量数据库系统往往把选取哪一种similarity score的权限交给用户，用户在使用向量数据库的时候，可以根据自己的想法选择similarity score。	

query type可以分为以下几类：

![image-20240304221555416](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240304221555416.png)

ANN：Approximate Nearest Neighbors

![image-20240304222839184](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240304222839184.png)

Range Queries

![image-20240304223033791](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240304223033791.png)

有些向量数据库在查询的时候还支持变量：Predicated, Batched, Multi-Vector

Predicated Search Queries是在查询的时候增加一个表达式作为限制，实现hybrid search

![image-20240304223503641](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240304223503641.png)

Batched Queries是指大量的查询同时提供给系统处理，适合于硬件加速的查询情况。

Multi-Vector是指在某个实物的向量和查询的向量可能由多个向量组成，而不是单个向量，具体包含：

* MQSF：multi-query single-feature，指的是查询由多个向量组成，实体由一个向量组成。

* MQMF：multi-query multi-feature，指的是查询和实体都是由多个向量组成。

* SQMF: single-query multi-feature，指的是查询由单个向量组成，而实体由多个向量组成。



##### 3 Indexing

给定一个查询向量，对于一次暴力的向量检索，需要对向量数据库中的每一个向量进行距离计算，当数据量变得非常大的时候，查询的时间花费将是不可接受的。而索引可以减少每次检索向量的范围，从而减少检索的时间。由于向量数据不像其他的经典的数据类型一样容易排序进而进行分类，所以需要一些新的技术，如Partitioning Techniques和Storage Techniques。而Partitioning Techniques可以分为：

*  Randomization：随机化探索独立事件之间的概率，让索引更好的区分真正的相似向量

* Learned Partitioning：基于机器学习的partition，可以是supervised也可以是unsupervised

* Navigable Partitioning：相比于将每个向量划分在某个固定的partition中，navigable partitioning是让不同的向量之间能够基于相似度遍历

Storage Techniques可以分为：

* Quantization：量化可以用来让向量降维，从而减少存储的消耗。
* Disk-Resident Design：磁盘的存储设计

对于索引而言，一个索引使用的技术往往是多个技术的结合。常见的索引可以根据他们的结构，划分为Table-based indexes, tree-based indexes和graph-based indexes。

Table-based indexes可以主要分为randomization和learned partitions。

![image-20240305134235413](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240305134235413.png)

亦称为LSH(Locality Sensitive Hashing)位置敏感哈希以及L2H(Learning to Hash)基于机器学习的哈希。

不论是那种类型的哈希索引,都需要大量的存储空间,而为了减少空间的消耗,Quantization量化技术应运而生.

Tree-based index,常见的基于树的索引有以下几种:

![image-20240305181048613](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240305181048613.png)

Graph-based index,基于图结构的索引,有以下一些常见的:

![image-20240305182320447](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240305182320447.png)

##### 4 Query Optimization and Execution

查询优化的目的是选择最优的查询计划以减少时间的消耗从而实现高效的查询。

* 对于包含表达式的hybrid search,需要使用专门的hybrid operators。而根据利用表达式过滤和查询的先后顺序,可以分为"pre-filtering"先进行表达式过滤再查询, "post-filtering"先查询再进行表达式的过滤,"single-stage filtering"在查询的过程中进行过滤.

* Plan Enumeration计划枚举节省了在线枚举计划的开销,可分为Predefined和Automatic两种类型

* Plan Selection计划选择可以分为Rule based和cost based
  
    Rule based是基于规则来选择执行的计划

![image-20240305210919544](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240305210919544.png)

​	Cost based是基于cost model来预测各个执行计划的成本,从而选取一个成本最低的计划

Query Execution查询执行

* Hardware Acceleration
  * CPU Cache
  * SIMD
  * GPU
* Distributed Search
* Out-Of-Place Updates 由于更新索引需要时间开销,会影响整体的查询效率。在更新索引的同时提高效率的常用方法如下:
  * Replicas: 构建多副本
  * Log-Structured Merge Tree: 日志结构合并树

##### 5 Current System

目前市场上存在的向量数据库可以主要分为两大类:Native专为向量数据设计的向量数据库和Extended在原先的数据库上扩展了向量的存储和检索功能

常见的向量数据库如下表

![image-20240305221300163](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240305221300163.png)

##### 6 Benchmarks

ANN算法在线benchmark网址:[ANN benchmark](http://ann-benchmarks.com)

##### 7 Challenges and Open Problems

* Similarity Score Selection
* Operator Design
* Incremental Search
* Multi-Vector Search
* Security and Privacy

