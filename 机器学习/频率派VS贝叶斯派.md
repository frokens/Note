### 数据作为概率模型
$X$ : data 
$$X = (x_1,x_2,\dots,x_n)^T_{n\times p}=
\begin{bmatrix}
x_{11} & x_{12}  & \cdots   & x_{1m}   \\
x_{21} & x_{22}  & \cdots   & x_{2m}  \\
\vdots & \vdots  & \ddots   & \vdots  \\
x_{n1} & x_{n2}  & \cdots\  & x_{nm}  \\
\end{bmatrix}_{N \times P}
$$
$\theta$ : parameter
$x\sim{p(x|\theta)}$

### 频率派VS贝叶斯派比较
#### 频率派：
- $\theta$ : 未知的常量
- $x$ : 随机变量
- 估计 $\theta$ 方法![[极大似然估计(MLE)]]
发展出 --> 统计机器学习（优化问题）：
- 提出模型
- Loss function
- 算法优化（牛顿法，梯度下降）



### 贝叶斯派：
- $\theta$ : 随机变量
- $\theta\sim{p(\theta)}$ : 先验概率
- $$P(\theta|X)=\frac{P(X|\theta) \cdot P(\theta)}{\int_\theta P(X|\theta)P(\theta)\,dx}$$
	- 这里$P(\theta~|X)$ 为后验概率(posterior), $P(X|\theta)$为似然(likelihood), $P(\theta)$ 为 先验概率(prior)

1. 估计$\theta$ 的方法![[极大后验估计(MAP)]]
2. 贝叶斯估计![[贝叶斯估计]]
发展出-->概率图模型(积分问题)：
- 数值积分（蒙托卡门）
