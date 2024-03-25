---
layout: post
title:  "《DILI：A Distribution-Driven Learned Index》论文粗读笔记"
date:   2024-03-23 04:21:02 +0800
categories: paper reading notes
---



#### 《DILI：A Distribution-Driven Learned Index》论文粗读笔记

**序号**：

**文献背景**：VLDB2023

**研究方法**：DILI

**存在问题**：近年来learned index作为一种新兴的索引正在取代traditional index的地位，Recursive Model Index(RMI)是常见的一种learned index。然而RMI存在如下问题：（1）the layout of RMI, 即the number of stages和the number of models at each stage必须要在模型创建之前就确定，不够灵活，不适合particular key distributions。（2）RMI不支持key insertions and deletions。

**解决方案**：提出了DILI，a distribution-driven learned index。

**创新点**：

——设计了一个distribution-driven learned index DILI。

——设计了一个distribution-driven BU-Tree 作为DILI的node layout reference，并且在进行specific cost analyse，设计了一个基于BU-Tree构造DILI的算法。

——在DILI的叶子节点上提出了local optimization，以此避免local search并且提升查询表现。

——为key insertion和deletion设计了高效的算法，能保证search performance。

**下一步/不足之处**：

**数据集**：

- four real datasets:

  - [FB](https://doi.org/10.7910/DVN/JGVF9A/Y54SI9):  contains 200M Facebook user ids.
  - [WikiTS](https://doi.org/10.7910/DVN/JGVF9A/SVN8PI): contains 200M unique request timestamps(integers) of log entries of the WiKipedia web-site.
  - [OSM](https://www.dropbox.com/s/j1d4ufn4fyb4po2/osm_cellids_800M_uint64.zst?dl=1): contains 800M ids of OpenStreetMap cells.
  - [Books](https://www.dropbox.com/s/y2u3nbanbnbmg7n/books_800M_uint64.zst?dl=1): contains 800M ids of books in Amazon.

- one synthetic dataset:

  - Logn: contains 200M unique values sampled from a heavy-tail log-normal distribution with following constraint
    $$
    \mu = 1, \sigma = 0
    $$

    $$
    
    $$

    

