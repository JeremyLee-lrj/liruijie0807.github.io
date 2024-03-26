---
layout: post
title:  "《DiskANN: Fast Accurate Billion-point Nearest Neighbor Search on a Single Node》论文粗读笔记"
date:   2024-03-24 14:21:02 +0800
categories: jekyll update
---



#### 《DiskANN: Fast Accurate Billion-point Nearest Neighbor Search on a Single Node》论文粗读笔记

**序号**：

**文献背景**：NeurIPS2019

**研究方法**：DiskANN

**存在问题**：当前sota的ANNS的索引必须存储在内存中以获得high-recall search，但是这使得搜索expensive,并且limits the size of datasets。

**解决方案**：

——提出了一个graph based indexing and search system called DiskANN，能够index, store and search a billion point database on a single workstation with just 64 GB RAM and an inexpensive solid-state drive(SSD)。

**创新点**：

——提出了Vamana索引结构，具有相比于NSG和HNSW更小的diameter，从而minimize the number of sequential disk reads.

——Vamana索引在面对大规模数据集的时候能够生成small indices for overlapping partitions，并且能够轻易地merge into one index，提供同样的search performance as a single-shot index constructed for the entire dataset。因此，这使得DiskANN能够支持size超过内存容量的dataset。

——在构建DiskANN的时候，Vamana能够结合off-the-shelf的vector compression scheme such as product quantization（PQ）。因此，图索引和原本的向量数据存储在磁盘上，而将压缩后的向量数据存在内存中。

**下一步/不足之处**：

**数据集**：

SIFT1M

GIST1M

DEEP1M

DEEP1B

 ANN_SIFT1B



