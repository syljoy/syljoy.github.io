---
layout: post
title: JS Divergence
date: 2020-07-21
tags: 机器学习
---

# JS Divergence

JS散度(Jensen-Shannon Divergence)，是基于KL散度的变体，解决了KL散度非对称的问题。一般地，JS 散度是对称的，其取值是 0 到 1 之间。

$$
D_{JS}(P||Q)=\frac{1}{2}D_{KL}(P||\frac{P+Q}{2}) + \frac{1}{2}D_{KL}(Q||\frac{P+Q}{2})
$$

![JS_Divergence](/images/posts/2020-07-21-JS Divergence/JS_Divergence.png)

KL 散度和 JS 散度度量的时候有一个问题：

如果两个分配 P,Q 离得很远，完全没有重叠的时候，那么 KL 散度值是没有意义的，而 JS 散度值是一个常数，这就意味这一点的梯度为 0。



### JS散度在GAN的应用

$$
\begin{equation}
\begin{aligned}
\mathop{\min}\limits_{G} \mathop{\max}\limits_{D}L(D,G) &= \mathbb{E}_{x\sim p_r(x)}[logD(x)] + \mathbb{E}_{z\sim p_z(z)}[log(1-D(G(z))] \\ &=\mathbb{E}_{x\sim p_r(x)}[logD(x)] + \mathbb{E}_{x\sim p_g(x)}[log(1-D(x)]\\ &=\int_x{p_r(x){\rm log}D(x) + p_g(x){\rm log}(1-D(x))} {\rm d}x
\end{aligned}
\end{equation}
$$

1. 固定生成器G，训练判别器D，最大化$V(G,D)$：

$$
  V(G,D) = \int_x{p_r(x){\rm log}D(x) + p_g(x){\rm log}(1-D(x))} {\rm d}x
$$

  - 将$p_r(x)$和$p_g(x)$看做两个常数A和B，$D(x)$看做$\tilde{x}$，得：

    $$
    f(\tilde{x})=A{\rm log}\tilde{x} + B{\rm log}(1-\tilde{x})
    $$

  - 然后$f(\tilde{x})$对$\tilde{x}$进行求导，并令导数等于0，得(判别器的最佳值)：

    $$
    D^*(x)=\tilde{x}^*=\frac{A}{A+B}=\frac{p_r(x)}{p_r(x)+p_g(x)}\in[0, 1]
    $$

2. 固定判别器D，训练生成G，最小化$V(G,D^*)$

  - 根据$D_{JS}$的公式，计算：
    $$
    \begin{equation}
    \begin{aligned}
    D_{JS}(p_r||p_g)&=\frac{1}{2}D_{KL}(p_r||\frac{p_r+p_g}{2}) + \frac{1}{2}D_{KL}(p_g||\frac{p_r+p_g}{2})\\&=\frac{1}{2}({\rm log}2 + \int_x{p_r(x){\rm log}\frac{p_r(x)}{p_r(x)+p_g(x)}}) + \frac{1}{2}({\rm log}2 + \int_x{p_g(x){\rm log}\frac{p_g(x)}{p_r(x)+p_g(x)}}) \\ &= \frac{1}{2}({\rm log}4 + \int_x{p_r(x){\rm log}\frac{p_r(x)}{p_r(x)+p_g(x)}}) \\ &= \frac{1}{2}({\rm log}4+L(G, D^*))
    \end{aligned}
    \end{equation}
    $$

  - 最小化$V(G,D^*)$

    $$
    V (G,D^*) = 2D_{JS}(p_r||p_g) - 2 {\rm log}2 \\
    \because D_{JS}(p_r||p_g) \ge 0, 当且仅当p_r=p_g时，D_{JS}(p_r||p_g)取0 \\ \therefore
    $$

> 理论上最后$p_g=p_r$，使得损失最小。此时判别器D的输出概率为$D^*=\frac{1}{2}$。

---

<div style="display:none;">

### 参考
1. https://hyper.ai/wiki/4274

</div> 

