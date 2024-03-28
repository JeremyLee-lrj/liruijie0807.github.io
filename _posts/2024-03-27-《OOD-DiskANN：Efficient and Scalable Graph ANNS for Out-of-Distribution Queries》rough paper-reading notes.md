---
layout: post
title:  "《OOD-DiskANN：Efficient and Scalable Graph ANNS for Out-of-Distribution Queries》论文粗读笔记"
date:   2024-01-22 14:21:02 +0800
categories: jekyll update
---



**序号**：

**文献背景**：arXiv

**研究方法**：OOD-DiskANN

**存在问题**：data dependent索引比data-agnostic索引性能表现更好。而当查询数据属于a different distribution的时候，例如索引存的是image embeddings，查询是textual embeddings，data dependent的性能优势就失去了。

**解决方案**：提出了OOD-DiskANN，在构建索引的过程中使用了OOD queries的small sample。

**创新点**：

——提出了RobustVamana，在DiskANN的Vamana图构造算法进行了优化，提高了OOD查询的检索效率。

——为了解决现有的PQ算法给OOD查询带来的large distortion in distance estimates，提出了Accurate PQ，提升了OOD查询的性能。

——提出了ParallelGorder，一种parallel and scalable graph reordering algorithm，减少了SSD的I/O

**下一步/不足之处**：

**数据集**：

Yandex Text-to-Image-1B(200 dimensions)

Turing Text-to-Image(1024 dimensions)

Web Ads(64 dimensions)
