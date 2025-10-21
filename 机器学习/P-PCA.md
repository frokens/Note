> [!info]
> 从生成模型的角度,假设复杂的高斯分布模型,本质上是由一个简单的先验线性无关模型,由线性变换和噪声叠加产生

> [!different]
> 相较于[[主成分分析PCA]], P-PCA 不仅仅给出样本在低维度的点估计，还给出其在低维度中完整的概率分布，考虑了噪声的存在，当 $\sigma^2 \rightarrow 0$ 时，P-PCA等价于传统的PCA, 但是它还可以用于去噪，捕捉更真实的潜在变量。 

模型假设
$$
\begin{align}X\in \mathbb{R}^p,~Z\in\mathbb{R}^q,&~q\lt p\\\begin{cases}Z&\sim N(0,I_q)\\X&=WZ + \mu +\epsilon \\ \epsilon &\sim N(0,\sigma^2I_p)\end{cases}\end{align}
$$

其中噪声 $\epsilon$ 是各项同性的（所有维度的方差相同，协方差为0），用于简化模型，便于计算

通过[[多维高斯分布边缘概率分布&联合概率分布]]中的方法去得出
推断 $P(Z|X)$

