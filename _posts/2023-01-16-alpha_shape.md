---
title: Alpha-shape
categories: Computer
image: 
---


## 簡介

給定點集合S，目標是找尋點集合的邊界。若找的到一半徑alpha，使得點落在多個圓的邊界上，不會滾入S集合內部，則落在邊界上的點則為邊界"alpha-shape"，
當alpha很小時，每個點都在邊界上，反之邊界則為一個convex hull。




source: 
- 計算幾何：https://dsa.cs.tsinghua.edu.cn/~deng/cg/cgaa/cgaa.3rd-edn.cn.pdf
- Bowyer-Watson：https://en.wikipedia.org/wiki/Bowyer–Watson_algorithm
- Delaunay:https://zhuanlan.zhihu.com/p/459884570
- Delaunay:https://zhuanlan.zhihu.com/p/42331420
- Alpha-shape:https://en.wikipedia.org/wiki/Alpha_shape