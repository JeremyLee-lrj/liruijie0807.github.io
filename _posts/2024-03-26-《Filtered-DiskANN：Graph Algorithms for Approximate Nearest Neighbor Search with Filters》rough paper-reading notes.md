---
layout: post
title:  "《Filtered-DiskANN：Graph Algorithms for Approximate Nearest Neighbor Search with Filters》rough paper-reading notes"
date:   2024-03-26 14:21:02 +0800
categories: jekyll update
---

**序号**：

**文献背景**：WWW2023(CCF-A)

**研究方法**：Filtered-DiskANN

**存在问题**：ANNS已经在检索和推荐系统领域无处不在，因此高效的filtered ANNS变得至关重要。Filtered ANNS需要在查询最近邻向量的同时满足一些属性限制，例如日期，价格等等。目前关于结合label metadata和vector data构建索引的研究很少，因此，目前的索引在面对filtered ANNS时表现出high search latency或者low recall，不能满足网络交互的需求。

**解决方案**：提出了两个算法以支持faster and more accurate filtered ANNS queries: one with streaming support, and another based on batch construction。

**创新点**：

——构建了graph-structured index，不仅基于vector data还基于label set。

——生成的索引能够支持thousands of filters，并且与unfiltered ANNS所消耗的资源几乎一致。

——生成的索引能够像DiskANN一样存储在SSD中

**下一步/不足之处**：

——支持超过几千的过滤集以及支持complex SQL-like filter expression仍然是challenging open problems 

**数据集**：

![image-20240327232931311](C:\Users\bangsun\AppData\Roaming\Typora\typora-user-images\image-20240327232931311.png)