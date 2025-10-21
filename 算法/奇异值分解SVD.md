通过SVD，能发现一组 A 的行空间的单位正交基 $v_1,v_2,\cdots,v_r$ 与 列空间 的单位正交基 $u_1,u_2,\cdots,u_r$ 且这组 ($v_i, ~u_i$) 能通过 A 相关联：
$$
\text{奇异向量} \quad Av_1 = \sigma_1u_1, ~Av_2 = \sigma_2u_2,~ \cdots~,~Av_r = 
\sigma_ru_r 
$$

设 A 的秩为 r， 即独立的行数或列数。后 n-r 个 $v$ 在 $A$ 的零空间里，后 m-r  个 $u$ 在 $A$ 的左零空间里。
所以 完整的SVD 包含 完整的 方阵 V 和 U
$$
AV = U\Sigma, \quad A \begin{bmatrix}v_1\cdots v_r \cdots v_n\end{bmatrix} = \begin{bmatrix}u_1\cdots u_r \cdots u_n\end{bmatrix}\begin{bmatrix} \sigma_{1} & & & \\ & \ddots & & \\ & & \sigma_{r}&\\ & & & 0 \end{bmatrix}$$

> [!特征、正交、奇异值]
> 这三类转换 矩阵 A的本质想法都是一样的，都是 $A=U\Sigma V^{-1}$ 先将行空间 x 通过 行空间的基的逆转为 单位空间，再通过对角矩阵拉伸空间，最后将 拉伸后的空间通过乘上列空间的基转为列空间。而通过构造正交的基，可以让解转为垂直基上的空间。
> 
> ![[Pasted image 20250414160016.png|300]]

又由于 通过 构造正交，所有的 v(或u)都是正交的，即 $V^T = V^{-1},~U^T = U^{-1}$
 
这样就得到了SVD最著名的表达式：$A = U\Sigma V^T$

### 计算方法
现在我们要 求出 v 和 u
通过将 $A$ 和 $A^T$ 相乘，能消去U 和 V，就得到了V和U
$$
\begin{array}{c}A^TA = V\Sigma^T U^TU\Sigma V^T = V\Sigma^T\Sigma V^T\\
AA^T = U\Sigma V^TV\Sigma^T U^T = U\Sigma\Sigma^TU^T\end{array}

$$

这样我们就得到了U和V，且U和V一定正交，因为 $A^TA$ 对称 （据[[谱定理]])

但是这未完成全部，因为 我们不知道 $\sigma$的符号，且当 $\lambda$为二重特征值时，多个v对应一个u
$$
先 v 后 u,~  A^TAv_k=\sigma_k^2v_k,~然后 u_k=\frac{Av_k}{\sigma_k},~k=1,2,\cdots,k
$$
验证 $u$ 为 $AA^T$ 的特征向量：
$$
AA^Tu_k = AA^T\frac{Av_k}{\sigma_k} = A \frac{A^TAv_k}{\sigma_k} = A\frac{\sigma_k^2v_k}{\sigma_k} = \sigma_k^2u_k
$$

同时通过选择单位正交的 $v$, 验证 $u$ 也是单位正交的：
$$
u_j^Tu_k = (\frac{Av_j}{\sigma_j})^T\frac{Av_k}{\sigma_k} = \frac{v_j^T(A^TAv_k)}{\sigma_j\sigma_k} = \frac{\sigma_k}{\sigma_j}v^T_jv_k = \begin{cases}1,~j=k\\0,~j\ne k\end{cases}
$$

#### 从另一个角度审视 $v_k$
$$
\text{最大化比值} \frac{\|Ax\|}{\|x\|}。当 x=v_k时取到最大值\sigma_k
$$
通过对这个比值平方后求导数会得到：
$$
2Sx= 2\lambda x
$$
>向量通过线性变换A后,其与原来本身比值最大的向量

