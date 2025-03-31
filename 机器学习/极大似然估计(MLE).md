#### MLE公式：
- $$\theta_{MLE}=\arg\max_{\theta}\log{P(X|\theta)}=\arg\max_{\theta}L(\theta)$$
- $\log$用于简化运算，对于$$P(X|\theta)=\prod_{i=1}^{N} p(x_i|\theta)\rightarrow\log{P(X|\theta)}=\sum_{i=1}^{N}\log{p(x_i|\theta)}$$
#### 极大似然原理：
在随机试验中，许多事件都有可能发生，概率大的事件发生的概率也大。若只进行一次试验，事件 A 发生了，则我们有理由认为 A 比其他事件发生的概率都大。

>**模型已定，参数未知**
>把样本$X$带入模型，$\theta$ 作为变量，去求使得$P(X|\theta)$概率最大的$\theta$
