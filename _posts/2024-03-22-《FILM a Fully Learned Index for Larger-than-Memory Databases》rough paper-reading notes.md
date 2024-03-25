---
layout: post
title:  "Welcome to Jekyll!"
date:   2024-01-22 14:21:02 +0800
categories: jekyll update
---



#### 《FILM: a Fully Learned Index for Larger-than-Memory Databases》论文粗读笔记

**序号**：

**文献背景**：VLDB2022

**研究方法**：FILM：a Fully Learned Index for Larger-than-Memory Databases

**存在问题**：现代应用正在以不可预知的速度产生大量的分布在各种各样设备上的数据，如位于内存中的数据和位于磁盘中的数据，因此适用于larger-than-memory databases，且支持快速插入和查找的索引技术变得至关重要。

**解决方案**：提出了FILM，a learned tree structure that uses simple approximation models to index data spanning different storage devices.

**创新点**：

——提出了FILM, the first learned approach specifically proposed for data indexing and cold data identification on heterogeneous storage.

——研究了索引更新的过程，提出一种能够在更新索引的过程中dynamically and efficiently handle data swapping between memory and disk with reduced I/O overhead.

——基于learned data pattern设计了一个自适应的LRU structure,能够反映出query workloads的access pattern，在保持lower CPU overhead的同时提供了fine-grained cold data  identification。

——提出了基于FILM的point and range queries算法，以处理在一种或多种类型的storage上的查询。  

**下一步/不足之处**：

把FILM扩展到multi-core systems并且设计更复杂的并发控制策略，以在进行并发更新和查询的过程中提升性能表现。

**数据集**：

**3 real world datasets:**

wiki_ts

books

astro_ra

**2 synthetic datasets:**

synthetic

YCSB
