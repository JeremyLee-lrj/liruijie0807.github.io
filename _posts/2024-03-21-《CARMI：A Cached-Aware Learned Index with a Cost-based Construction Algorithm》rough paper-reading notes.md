---
layout: post
title:  "《CARMI：A Cached-Aware Learned Index with a Cost-based Construction Algorithm》粗读笔记"
date:   2024-03-21 04:21:02 +0800
categories: jekyll update
---



##### 《CARMI：A Cached-Aware Learned Index with a Cost-based Construction Algorithm》粗读笔记

**序号**：

**文献背景**：VLDB2022

**研究方法**：A cache-aware learned index

**存在问题**：在最近的研究中，learned index的实验效果相对于传统的index比较好，但是现有的learned indexes在synthetic和real-world的数据集上的表现出现了差距。

**解决方案**：这篇论文首先指出了忽视data partitioning的重要性是出现上述问题的主要原因。于是，直接使用data partitioning技术来构建索引，并且提出了一种效率高且可更新的cache-aware RMI框架，CARMI。

**创新点**：

——首先提出了entropy作为衡量在learned index中tree node的data partitioning的有效性标准，并且提出了一个新颖的cost model。

——然后基于提出的cost model，CARMI可以在各种各样的数据集和工作负载下通过hybrid construction algorithm自动选择tree structures和model types，不需要人工干预。

——最后，由于内存访问会限制RMI的performance，因此CARMI使用了cache-aware设计，利用CPU cache有效降低对内存的访问。

**下一步/不足之处**：

——在目前的CARMI中，inner node和leaf node都是存放在内存中的，下一步工作考虑放在Disk或者NVM上

——目前CARMI只支持数值类型的数据，暂时不能支持其余类型，例如string，因为目前很难找到适合string类型的数据分布模型。

**数据集**：

Four synthetic datasets

YCSB dataset

OSMC dataset

Facebook dataset