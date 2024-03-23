---
layout: post
title:  "《DPTree Differential Indexing for Persistent Memory》rough paper-reading notes"
date:   2024-03-20 04:21:02 +0800
categories: paper reading notes
---

### 《DPTree: Differential Indexing for Persistent Memory》论文粗读笔记

**序号**：

**文献背景**：VLDB2019

**研究方法**：A new persistent index called DPTree

**存在问题**：PM存在读写不对称的问题，写性能比较差

**解决方案**：提出了一种两级索引，分为Buffer Tree和Base Tree，前者负责处理insert/delete/update等操作，后者负责处理query操作。

##### 创新点：

——在Buffer Tree中提出了一个write-optimized adaptive PM log

——在Base Tree中提出了一个coarse-grained versioning技术，能在提供并发读的同时保证crash consistency

——基于versioning technique提出了一个in-place crash-consistent merge algorithm

——提出了一个hashing-based的PM leaf node设计，从而在保持range scan的同时减少PM的读操作

**下一步工作/不足之处**：

Query the log directly without DRAM buffer, for example, structure the PM log as a persistent hash table for both buffering and durability, to reduce the complexity of the design.

**数据集**:

无特定数据集，使用Micro-benchmark和YCSB测试。



