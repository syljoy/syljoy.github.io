---
layout: post
title: KL Divergence
date: 2020-07-20
tags: 机器学习 
---


# KL Divergence



所谓KL散度，是指当某分布q(x)被用于近似p(x)时的信息损失。记作：$D_{KL}(P||Q)$，读作：KL散度从Q到P（KL Divergence from Q to P）。KL散度又称为相对熵（*relative entropy*）。

$D_{KL}(P||Q)=\int_x{p(x){\rm log}\frac{p(x)}{q(x)}} {\rm d}x = \sum_{i=1}^N{p(x_i) \cdot ({\rm log}p(x_i) - {\rm log}q(x_i))} = E[{\rm log}p(x_i) - {\rm log}q(x_i)]$

q(x)能在多大程度上表达p(x)所包含的信息，KL散度越大，表达效果越差。

![KL Divergence](/images/posts/2020-07-20-KL Divergence/KL_Divergence.png)

### 性质：

1. $D_{KL}(P||Q) \ge 0,   \forall{P,Q}$

2. $D_{KL}(P||Q) = 0, 当P和Q一样的时候$
3. $D_{KL}(P||Q) \ne D_{KL}(Q||P)$


 ### 最小化

给定P找Q来最小化KL散度。

1. $Q^{*(from)} = \mathop{\arg\max}\limits_{Q} D_{KL}(P||Q)$
   - $Q^{*(from)}$ ：在P有高概率(high probability)的地方，Q也会得到高的位置
2. $Q^{*(to)} = \mathop{\arg\max}\limits_{Q} D_{KL}(Q||P)$
   - $Q^{*(to)}$ ：在P有低概率(low probability)的地方，Q也会得到低的位置

![image-20200726100238889](/images/posts/2020-07-20-KL Divergence/image-20200726100238889.png)

如上图所示为：使用一个高斯分布Q近似两个高斯分布混合的P。

第一个图：在P的高点，可能由于是两个极大值点，而使得Q在而两大极值点中间取得极大值。

第二个图：在P的极小值点处，Q也取极小值。

---



### 参考
1. https://zhuanlan.zhihu.com/p/95687720
2. https://www.bilibili.com/video/av71868743


