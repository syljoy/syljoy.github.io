# GAN训练难题

大多数情况下，$P_G$和$P_{data}$是没有重叠（overlapped）的。



### 没有overlap：


$$
D_{KL}(P||Q) = \int_x p(x){\rm log} \frac{p(x)}{q(x)} = \sum_{x=0}p(x){\rm log} \frac{p(x)}{q(x)} = { + \infty }
$$

$$
D_{KL}(Q||P) = { + \infty }
$$

$$
D_{JS}(P||Q)=...={\rm log}{2}
$$

- 没有重叠，没有梯度信息（梯度消失）。
- JS divergence is log2 if two distributions do not overlap. 如果两个分布不重叠，则JS散度为log 2。
- 没有重叠，判别器D就可以很好地区分出来real data与fake data。



**WGAN的loss可以解决上述问题。**







---

<div style="display:none;">

### 参考
1. 

</div> 

