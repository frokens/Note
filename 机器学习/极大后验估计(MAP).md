#### MAP公式：
- 先由贝叶斯公式推出 $P(\theta|X)$ ，且 由于$P(X)$与$\theta$ 无关
	- 所以:$$P(\theta \mid D) \propto P(D \mid \theta)\,P(\theta)
$$
- $$\theta_{MAP}=\arg\max_\theta P(\theta|X) =\arg\max_\theta P(D \mid \theta)\,P(\theta)$$
#### 极大后验估计原理：
与[[极大似然估计]]在想法本质上一致，都是求使得概率最大化的$\theta$ , 然后直接带入假设的模型, 但是转换视角，将概率变为在样本$X$为条件$\theta$ 为变量的最大可能参数$\theta$

>[!info]	
>在$X$条件下求最可能的参数$\theta$(*MAP*)  **VS**  求最符合$X$ 分布的参数$\theta$ (*MLE*)
	
需要提前假设 $\theta$ 的先验分布$P(\theta)$ 用于通过贝叶斯公式计算$P(\theta|X)$ 

> [!理解]
> 后验分布是我们对未知事物在看到证据后更新了的“信念状态”。它包含了关于 $\theta$ 的所有信息——最可能的值（如后验均值或众数）以及围绕这些值的不确定性（如后验方差）。