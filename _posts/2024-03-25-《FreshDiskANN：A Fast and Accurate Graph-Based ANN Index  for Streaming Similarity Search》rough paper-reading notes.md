---
layout: post
title:  "《FreshDiskANN：A Fast and Accurate Graph-Based ANN Index  for Streaming Similarity Search》论文粗读笔记"
date:   2024-03-25 14:21:02 +0800
categories: jekyll update
---

**序号**：

**文献背景**：arXiv

**研究方法**：FreshDiskANN

**存在问题**：ANNS是信息检索的基础构件，而且基于图的索引是state of the art并且被广泛用于工业界。但是目前ANNS领域内的图索引是静态的，无法处理数据发生实时变化的场景。为了克服这一弊端，目前工业界尝试定期重构索引以处理数据的实时更新，但这会带来巨大的开销。

**解决方案**：首次提出了用于ANNS并且能够反映出数据集实时更新的graph-based index，并且不影响检索的性能。

**创新点**：

——设计了FreshVamana，第一个支持插入和删除的graph-based index

——为了实现可伸缩性，将大部分的索引存在SSD上，内存中只存少量的最近更新的索引。

——设计了一个新颖的two-pass StreamingMerge算法，能够以一种非常高效的方式将in-memory index合并到SSD-resident index

——设计了FreshDiskANN，包含long-term SSD-resident index和short-term in-memory index。其中short-term memory汇总近期的更新，并且在用户unknown的情况下定期的通过StreamingMerge过程将short-term index整合到long-term memory，降低系统的开销。

**下一步/不足之处**：

**数据集**：

SIFT1M

GIST1M

DEEP1M

SIFT1B

