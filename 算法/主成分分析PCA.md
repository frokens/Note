> [!核心思想]
一组可能线性相关的变量通过正交变幻变为正交线性无关的变量
即对原始特征空间的重构（相关 $\rightarrow$ 无关），从而找到真正有最大贡献的特征
再通过选择贡献（特征值）最大的特征(主成分)的前K个特征，实现对特征的筛选
从而筛去冗余信息（一方面体现在选择特征值前k个特征，另一方面，SVD会压缩到满秩的空间）



具体数学上的实现方法：
1. 最大投影方差（找到一个维度方向，样本的投影之间的方差最大）
2. 最小重构距离（将投影重构回样本原位置的代价最小）

将思想转为最优化问题，从而得出具体的数学公式

### 1. 最大投影方差角度
$u_i$  方向上投影和投影方差的数学表示：
$$
\begin{gather}p_i =\frac{1}{N} x_iu_i^T\\J=\sum_{i=1}^{N}((x_i-\overline{x})^Tu_i)^2 = \frac{1}{N} u_i^T Su_i\end{gather}
$$

为了找到投影，即找到一个最大的投影方差，本质上是一个有约束的优化问题：
$$
\begin{cases}\hat{u} = \arg \max{u_i^TSu_i}\\s.t. u_i^Tu_i = 1\end{cases}
$$

而又约束的最优化问题的求解常用拉格朗日数乘法
![[拉格朗日乘数法]]


对于主成分分析，即解：
$$
\begin{gather}\mathcal{L}(u_i,\lambda) = u_i^TSu_i + \lambda(1-u_i^Tu_i)\\
\frac{\partial \mathcal{L}}{\partial u_i} = 2Su_i -\lambda 2 u_i = 0
\\ Su_i = \lambda u_i
\end{gather}
$$

而在这里，我们会发现 $u_i$ 就是协方差矩阵的的特征向量，而 $\lambda$ 即为特征值

### 2. 最小代价重构角度
PCA本质为坐标系的变换：
	从原先的 $(x_{i1},x_{i2},\cdots,x_{ip})$ 为基的坐标系变换为以 $(u_1,u_2,\cdots,u_{q})$ 的新的坐标系下,这里
	$p>q$ 代表降维
而 $x$ 先转为 $u_i$ 坐标系后再转回原型的坐标系下的 $\hat{x}$ （重构）数学表达如下：
$$
 \hat{x_i} = \sum_{k=1}^{q}(x_i^Tu_k)u_k
$$
括号内为算数量积，即在 $u_i$ 上的长度，而再乘上 $u_k$ 为转为原先的坐标系
（这里简化了中心化的内容）

所以最小重构代价为：
$$
J = \frac{1}{N}\sum_{i=1}^N\|x_i - \hat{x_i}\|^2
$$

由于 $p<q$ ，前 $p$ 维度直接减去：
$$
\begin{align}J &= \frac{1}{N} \sum_{i=1}^N\sum_{k=q+1}^p{\|(x_i^Tu_k)u_k\|^2} \\&= \frac{1}{N}\sum_{i=1}^N\sum_{k=q+1}^p{(x_i^Tu_k)^2} 
\qquad \text{范数即求长度，不保留坐标}\\ &=\sum_{i=1}^N\sum_{k=q+1}^p{\frac{1}{N}((x_i-\overline{x})^Tu_k)^2} \qquad \text{加入考虑中心化} \\ & =\sum^P_{k=q+1}{u_k^TSu_k}\end{align}
$$
这里等价于从投影方差角度的最优化问题
### 求坐标矩阵
所有要求最佳主成分，即求 $Su_i = \lambda u_i$

对 $P\times P$ 的 S 进行 特征值分解：
$$
S = GKG^T
$$

所以要得到 最终的坐标矩阵，需要的步骤为：
1. 中心化数据：$HX$
2. 乘上主成分：$HXG$


而这又可以从两个角度进行
1. 直接计算出 G
2. 从数据矩阵X奇异值分解出发：
		$HX = U\Sigma V^T$

##### 1. S 主成分分析
$$
S_{P\times P}= X^THX = X^TH^THX = V\Sigma^2 V^T
$$
在这里 V 就是 G ,我们得到了主成分G, 接下来，直接 $HXV$ 得到坐标
而V通过[[奇异值分解SVD]]得到
##### 2. T 主坐标分析 （$PC_oA$)
通过对T特征分解直接得到坐标矩阵
$$
T_{N\times N} = HXX^TH^T = U\Sigma^2 U^T
$$
而其中的 $U\Sigma$ 即为最终的坐标矩阵

证明：
$$
HXV = U\Sigma V^T V = U\Sigma 
$$


> [!info]
> 主坐标分析在 维度大于样本数量时,计算速度更快,因为其是 $N \times N$ 的矩阵

