---
title: 数据聚类(二)
categories:
  - 科研学习 - 计算机视觉与机器学习
tags:
  - 计算机视觉
  - 机器学习
  - 数据聚类
cover: /images/cover/数据聚类.jfif
katex: true
abbrlink: 658d69b7
date: 2020-02-24 12:27:03
---

数据聚类是一个将对象进行分组的任务，分组后，组内对象相比组间数据更相似。

应用：模式识别、图像分析、信息检索、生物信息等领域

**经典聚类方法：**

- K-means clustering
- Hierarchical Clustering
  - Top-down(divisive)
  - Bottom-up(agglomerative)

**谱聚类：**

- Average Weight(AW)
- Ratio Cut(RC)
- Normalized Cut(NC)
- Min-Max Cut(MMC)

# 经典聚类方法

## K-means

- 输入：
  - 需要聚类的数据集：$ \boldsymbol D = (x_1, x_2, ..., x_n) $
  - 群的个数 $ k $， $k \ll n$

- 输出：满足以下损失函数的 $k$ 个群： $\boldsymbol S = (S_1, S_2, ..., S_k)$
$$
\arg\min\limits_S \sum_{i=1}^k \sum_{x_j\in S_i}||x_j -\mu_i||^2
$$

- 算法步骤：

  1. 随机初始化 $k$ 个中心 $(m_1^{(1)}, m_2^{(1)},...,m_k^{(1))}$
  2. $Set\ t=1$
  3. 分配步骤：针对每个数据 $x_p \in \boldsymbol D$，找到与它距离最近的中心点 $m_i^{(t)}$，把 $x_p$ 分配给这个中心点所代表的群 $S_i^{(t)}$
  4. 更新步骤：为每个群计算新的中心 $m_i^{t+1} = \frac{1}{|S_i^{(t)}}\sum_{x_j \in S_i^{(t)}}x_j $
  5. 若在此次迭代中，群的分布没有变化，则聚类结束；否则，设置 $t = t+1$，并返回第 3 步

  ![1582020002754](/images/cvml-数据聚类-二/1582020002754.png)

### 算法特点

优点：

- 简单高效：时间复杂度为 $O(tkn)$，其中 $n$ 为数据量，$k$ 为数据群，$t$ 为迭代次数

缺点：

- 需要用户指定群的个数 $k$
- 聚类结果往往是局部最优
- 初始中心点的选择对聚类的结果影响非常大

## Hierarchical Clustering

**Bottom-Up(agglomerative)：**

1. 每个数据各自形成一个群
2. 现有群中距离最近的一对合并成一个新的群
3. 满足停止条件则停止合并，否则重复第 2 步

**Top-Down(divisive)：**

1. 用所有数据形成一个群
2. 利用某种划分方式将现有群划分为两个群
3. 若满足停止条件则停止划分，否则重复第 2 步

![1582020210189](/images/cvml-数据聚类-二/1582020210189.png)

### 算法特点

优点：

- 不需要事先指定群的个数
- 能很好地适应很多感知领域的问题

缺点：

- n由于每次合并或分裂都要逐个评估所有可能情况并选出最优合并或分裂，因此时间复杂度较高
- 容易陷入局部最优解

# 谱聚类（Spectral Clustering)

- 利用一个**相似图**（similarity graph）来表示数据间的相似关系。
- 把聚类问题转换成计算相似图**最佳切割法**的问题。
- 相似图最佳切割法可以通过计算**亲和矩阵（affinity matrix）的特征向量**来求得。

## 相似图的数学定义

假设 D 是需要聚类的数据集，用 D 建立的无向图可以表示为 $G(V, E, A)$

- $\boldsymbol V={v_i}$ 代表顶点集（vertex set），$v_i$ 代表 $D$ 中的一个数据点 $i$
- $\boldsymbol E$ 表示边缘集（edge set）， $(i, j) \in E$ 表示连接数据点 $i$ 与 $j$ 的一条边
- $\boldsymbol A = \{a_{ij}\}$ 表示图的关联矩阵（graph affinity matrix），矩阵的每个元素 $a_{ij}$ 代表数据点 $i$ 和 $j$ 之间的相似度（或连接 $(i, j)$ 的权重

## 相似图的构建方法

- $\varepsilon$ -近邻图：若两数据点间的距离 $\le e$，就把它们连接起来。

- $k$-近邻图：

  - 如果 $v_i$ 是 $v_j$ 的 $k$-近邻，或者$v_j$ 是 $v_i$ 的 $k$-近邻，
  - 如果 $v_i$ 和 $v_j$ 互为 $k$-近邻，

  则在两者间建立一个连接，并把距离作为连接的权重

- 全连接：任意两点建立连接，并定义新全重 $a_{ij}$，最常用权重：$a_{ij} = exp(-\frac{||x_i-x_j||^2}{2\sigma^2})$

## 基于相似图的数学定义

- 顶点 $v_i$ 的度数（degree）：$d_i = \sum^{n}_{j=1}a_{ij}$

- 度数矩阵 $D$：对角矩阵，对角线元素 $d_i$ 就是顶点 $v_i$ 的度数：

- 相似图 $G$ 的子图 $S_i, S_j$ 间的相似度：
  $$
  W(S_i, S_j) = \sum\limits_{i \in S_i, j \in S_j} a_{ij}
  $$

- 子图 $S$ 的大小：
  $$
  \begin{aligned}
  &|s| = No.\ of\ vertices\ in\ S \\
  &vol(S) = \sum d_i
  \end{aligned}
  $$

## 损失函数的定义

四种常用损失函数：

- Average Weight(AW):      $F_{AW} = \sum_{c=1}^k \frac{W(S_c, S_c)}{|S_c|}$
- Ratio Cut(RC):                  $F_{RC} = \sum_{c=1}^k \frac{W(S_c, \overline {S_c})}{|S_c|}$

- Normalized Cut(NC):     $F_{RC} = \sum_{c=1}^k \frac{W(S_c, \overline {S_c})}{(S_c, V}$
- Min-Max Cut(MMC):      $F_{RC} = \sum_{c=1}^k \frac{W(S_c, \overline {S_c})}{(S_c, S_c}$

## 重要数学变换

用 $x=[x_1,x_2,...x_n]^T$ 来代表群 $S$ 的指示向量（indication vector），每个元素 $x_i={0,1}$，$x_i=1$ 表示数据点 $i$ 属于群 $S$，$x_i=0$ 表示 $i$ 不属于 $S$。则有：

$$
\begin{aligned}
&|S|=x^Tx \\
&W(S_1,S_2)=x_1^TAx_2 \\
&W(S,V)=x^TDx \\
&W(S,\overline S)=x^T(D-A)x
\end{aligned}
$$

通过以上定义，四个损失函数进一步转化为：

$$
F_{AW} = \sum_{c=1}^k \frac{W(S_c, S_c)}{|S_c|} = \sum_{c=1}^k \frac{x_c^T A x_c}{x_c^T x_c} = \sum_{c=1}^k y_c^T A y_c
$$

$$
F_{RC} = \sum_{c=1}^k \frac{W(S_c, \overline S_c)}{|S_c|} = \sum_{c=1}^k \frac{x_c^T (D-A) x_c}{x_c^T x_c} = \sum_{c=1}^k y_c^T (D-A) y_c
$$

其中 $y_c = \frac{x_c}{\|x_c\|}$


$$
\begin{aligned}
F_{NC} &= \sum_{c=1}^k \frac{W(S_c, \overline S_c)}{W(S_c, V)} = \sum_{c=1}^k \frac{x_c^T (D-A) x_c}{x_c^T D x_c} = \sum_{c=1}^k k - \frac{x_c^T A x_c}{x_c^T D x_c} \\ 
&= \sum_{c=1}^k y_c^T (D-A) y_c = \sum_{c=1}^k \frac{x_c^T D^\frac{1}{2}D^{-\frac{1}{2}} (D-A) D^{-\frac{1}{2}}D^{\frac{1}{2}} x_c}{x_c^T D^\frac{1}{2}D^{\frac{1}{2}} x_c} \\
&= \sum_{c=1}^{k} y_c^T D^{-\frac{1}{2}}(D-A)D^{-\frac{1}{2}} y_c
\end{aligned}
$$

其中，$y_c = \frac{D^{\frac{1}{2}} x_c}{\|D^{\frac{1}{2} x)c}\|}$

$$
\begin{aligned}
F_{MMC} &= \sum_{c=1}^k \frac{W(S_c, \overline S_c)}{W(S_c, S_c)} = \sum_{c=1}^k \frac{x_c^T (D-A) x_c}{x_c^T A x_c} \\
&= -k + \frac{x_c^T D x_c}{x_c^T A x_c} \to \text{与 }F_{NC}\text{等价}
\end{aligned}
$$

求上述4个损失函数的精准解是一个 NP hard 问题，原因在于：群 $S$ 的指示向量 $x=[x_1,x_2,...,x_n]$ 的每个元素只取离散值 $x_i \in \{0,1\}\,x_i=1$ 表示数据点 $i$ 属于群 $S$，$x_i=0$ 表示 $i$ 不属于 $S$。

**如果我们把这个限制去掉，就能够利用瑞利商定理(Rayleigh Quotient Theorem)求得近似解！**

## 瑞利商定理(Rayleigh Quotient Theorem)

假设 $A$ 是一个实对称矩阵(symmetric matrix)，$\lambda_{min1},\lambda_{min2},...,\lambda_{max}$ 是矩阵 $A$ 的最小，第二小， … ， 最大特征值，$v_{min1} ,v_{min2}, …,v_{max}$ 是对应的特征向量，则:

$$
\frac{x^T A x}{x^T x}
$$

- 在 $x=v_{min1}$ 处取得最小值 $\lambda_{min1}$，
- 在 $x=v_{min2}, v_{min3}$ 处取得下一个极值$\lambda_{min2}, \lambda_{min3}$
- $\frac{x^T A x}{x^T x} \le \lambda_{max}$在 $x=v_{max}$处取得最大值 $\lambda_{max}$

**解决方案**：

将瑞利商定理用于上述损失函数，得到如下结论：

1. Average Weight
   
   $$
   F_{AW} \le \lambda_1 + \cdots + \lambda_k
   $$

   $\lambda_1 + \cdots + \lambda_k$ 是 $A$ 的 $k$ 个最大特征值，当 $x_c$ 分别等于相应的 $k$ 个最大特征向量时，上式取等号。

2. Ratio Cut

   $$
   F_{RC} \ge \lambda_n + \cdots + \lambda_{n-k+1}
   $$

   $\lambda_n + \cdots + \lambda_{n-k+1}$ 是 $D-A$ 的 $k$ 个最小特征值，当 $x_c$ 分别等于相应的 $k$ 个最小特征向量时，上式取等号。

3. Normalized Cut
   
   $$
   F_{NC} \ge \lambda_n + \cdots + \lambda_{n-k+1}
   $$

   $\lambda_n + \cdots + \lambda_{n-k+1}$ 是 $D^{-\frac{1}{2}}(D-A)D^{-\frac{1}{2}}$ 的 $k$ 个最小特征值，当 $y_c$ 分别等于相应的 $k$ 个最小特征向量时，上式取等号。

4. Min-Max Cut 的最佳解和 NC 是等价的
   
    $$
    \begin{aligned}
    F_{NC} &= \sum_{c=1}^k \frac{W(S_c, \overline S_c)}{W(S_c, V)} = \sum_{c=1}^k \frac{x_c^T (D-A) x_c}{x_c^T D x_c} = k - \sum_{c=1}^k \frac{x_c^T A x_c}{x_c^T D x_c} \\ 
    &= k - \sum_{c=1}^k \frac{x_c^T D^\frac{1}{2}D^{-\frac{1}{2}} (D-A) D^{-\frac{1}{2}}D^{\frac{1}{2}} x_c}{x_c^T D^\frac{1}{2}D^{\frac{1}{2}} x_c} \\
    &= k - \sum_{c=1}^{k} y_c^T D^{-\frac{1}{2}}(D-A)D^{-\frac{1}{2}} y_c
    \end{aligned}
    $$

    $$
    \begin{aligned}
    F_{MMC} &= \sum_{c=1}^k \frac{W(S_c, \overline S_c)}{W(S_c, S_c)} = \sum_{c=1}^k \frac{x_c^T (D-A) x_c}{x_c^T A x_c} = -k + \frac{x_c^T D x_c}{x_c^T A x_c} \\
    &= -k + \sum_{c=1}^{k} \frac{1}{y_c^T D^{-\frac{1}{2}}(D-A)D^{-\frac{1}{2}} y_c}
    \end{aligned}
    $$

    其中 $y_c = \frac{x_c}{\|x_c\|}$

   由于二者等价：

   $$
   F_{MMC} \ge -k + \frac{k^2}{\lambda_1 + \cdots + \lambda_k}
   $$

   $\lambda_1 + \cdots + \lambda_k$ 是 $D^{-\frac{1}{2}}(D-A)D^{-\frac{1}{2}}$ 的 $k$ 个最大特征值，当 $y_c$ 分别等于相应的 $k$ 个最大特征向量时，上式取等号。

## 通用谱聚类算法

$n \times k$ 的矩阵 $Y$ 由 $k$ 个固有向量组成：

$$
Y = [y_1, y_2,...,y_k] = 
\begin{bmatrix} 
  \boldsymbol u^T_1 \\ \vdots \\ \boldsymbol u^T_N
\end{bmatrix}
= \begin{bmatrix} 
  a_1\tilde\boldsymbol  u^T_1 \\ \vdots \\ a_n \tilde\boldsymbol u^T_N
\end{bmatrix}
$$

是由 $k$ 个固有向量的第 $i$ 个元素组成的$ k \times 1$ 维向量。上式中 $a_i = \|u_i\|$ 且 $\tilde u_i = \frac{u_i}{\|u_i\|}$。

## 谱聚类算法

- 输入：需聚类的数据集 $D$，和群的个数 $k, k \ll n$$

- 算法：
  
  1. 为数据集 $D$ 建立一个相似图 $G(V, E, A)$
  2. 计算矩阵 $A$ 的 $k$ 个最大/最小固有向量 $Y = [y_1, y_2,...,y_k]$
  3. 计算 $\tilde u_i$
  4. 在 $\tilde u_i$ 空间利用 k-means 找出 $k$ 个群

  $$
  Y = [y_1, y_2,...,y_k] = 
  \begin{bmatrix} 
    \boldsymbol u^T_1 \\ \vdots \\ \boldsymbol u^T_N
  \end{bmatrix}
  = \begin{bmatrix} 
    a_1\tilde\boldsymbol  u^T_1 \\ \vdots \\ a_n \tilde\boldsymbol u^T_N
  \end{bmatrix}
  $$

## 四种谱聚类算法对比

|算法|Average Weight|Ratio Cut|Normalized Cut|Min-Max Cut|
|---|---|---|---|---|
|准则|$\sum_c \frac{W(S_c, S_c)}{\|S_c\|}$|$\sum_c \frac {W(S_c, \overline{S_c})}{W(S_c, V)}$|$\sum_c \frac{W(S_c, \overline {S_c})}{\|S_c\|}$|$\sum_c \frac {W(S_c, \overline{S_c})}{W(S_c, S_c)}$|
|矩阵形式|$\sum_c \frac{x_c^T A x_c}{x_c^T x_c}$|$\sum_c \frac{x_c^T (D - A) x_c}{x_c^T x_c}$|$\sum_c \frac{x_c^T (D - A) x_c}{x_c^T D x_c}$|$\sum_c \frac{x_c^T (D - A) x_c}{x_c^T A x_c}$|
|固有值&向量问题|$\begin{aligned} & Ay = \lambda y \\ &y_c = \frac{x_c}{\|x_c\|} \end{aligned}$|$\begin{aligned} & (D - A)y = \lambda y \\ &y_c = \frac{x_c}{\|x_c\|} \end{aligned}$|$\begin{aligned} & Ay = \lambda Dy \\ &y_c = \frac{D^{\frac{1}{2}}x_c}{\|D^{\frac{1}{2}}x_c\|} \end{aligned}$|$\begin{aligned} & Ay = \lambda Dy \\ &y_c = \frac{D^{\frac{1}{2}}x_c}{\|D^{\frac{1}{2}}x_c\|} \end{aligned}$|
|损失函数取值|$\le \sum_{i=1}^k \lambda_i$|$\ge \sum_{i=1}^k \lambda_{n-i+1}$|$\ge \sum_{i=1}^k \lambda_{n-i+1}$|$\ge -k + \frac{k^2}{\sum_{i=1}^k \lambda_i}$|


## 谱聚类总结

- 谱聚类没有 Structural model
- 损失函数 $F_{AW}, F_{RC}, F_{NC}, F_{MMC}$
- 用瑞利商定理（Tayleigh Quotient Theorem）求最佳解

  因为瑞利商定理求得的解是近似解，所以需要在固有向量空间里运用k-means算法得到最终结果。

## 谱聚类算法重要特性

**K-means和AW谱聚类算法间的联系**

K-means 算法和 Average Weight 谱聚类算法是等效的。

$$
F_{KM}=\sum_{c=1}^k\sum_{x\in S_c}||x-u_c||^2
$$

其中，$u_c$ 是子集 $S_c$ 的中心。

若 K-means 中用两数据点特征向量的内积/点积（dot product）来度量相似度，则有：

$$
\begin{aligned}
&F_{KM}=\sum_{c=1}^k \frac{1}{2|S_c|}\sum_{x_1,x_2 \in S_c} ||x_1-x_2||^2 \\
&= \sum_{c=1}^k \frac{1}{2|S_c|}\sum_{x_1,x_2 \in S_c} (||x_1||^2+||x_2||^2-2x_1\cdot x_2) \\
&=\sum_x ||x||^2 - \sum_{c=1}^k \frac{1}{|S_c|} \sum_{x_1, x_2 \in S_c} x_1 \cdot x_2 \\
&=\sum_x ||x||^2 - F_{AW}
\end{aligned}
$$

因此，最小化 $F_{KM}$ 与最大化 $F_{AW}$ 是等效的。

## 谱聚类性能比较

$RC, NC, MMC$ 的性能比 $AW$ 好，因为 $RC, NC, MMC$ 的损失函数里包含 $L (L_{sys})$，而 $AW$ 的损失函数则没有。

$NC, MMC$ 的性能比$RC$ 好，因为 $NC, MMC$ 的损失函数里包含 $L_{sys}$，而 $RC$ 的损失函数里则包含 $L$。

# 图拉普拉斯（Graph Laplacian）

定义：$L=D=A$

$L$ 的特性：
1. 仍和向量 $x\in \mathcal R^n$ 满足下列关系：
  $$
  x^TLx = \frac{1}{2}\sum_{i,j=1}{n}a_{ij}(x_i-x_j)^2
  $$
2. $L$ 是对称和半正定矩阵
3. 矩阵 $L$ 的最小固有值是 0，对应的固有向量为 1
4. 矩阵 $L$ 拥有 $n$ 个非负的实数固有值：
  $$
  0=\lambda_1 \le \lambda_2 \le \cdots \le \lambda_n
  $$

## 归一化拉普拉斯（Normalized Graph Laplacian）

定义：$L_{sys}=D^{-\frac{1}{2}}(D-A)D^{-\frac{1}{2}}$
$L_{sys}$ 的特性：

1. 任何向量 $x \in \mathcal R^n$ 满足下列关系：
  $$
  x^T L_{sys} x = \frac{1}{2} \sum_{i,j=1}^n (\frac{x_i}{\sqrt d_i}-\frac{x_j}{\sqrt{d_j}})^2$$
2. L_{sys} 是对称和半正定矩阵
3. L_{sys} 的最小固有值是 0，对应的固有向量为 $D^{-\frac{1}{2}}1$
4. L_{sys} 拥有 $n$ 个非负的实数固有值：
  $$
  0=\lambda_1 \le \lambda_2 \le \cdots \le \lambda_n
  $$

## 图拉普拉斯的重要定理

如果 $G$ 是一个无向图，且它的每个边缘的权重都 $\ge 0$，那么图拉普拉斯 $L(L_{sys})$ 取值为 0 的固有值个数等于 $G$ 的连接子图 $A_1,...,A_k$ 的个数。固有值 0 的子空间的基有固有值 0 的对应固有向量 $1_{A_1},...1_{A_k}(D^\frac{1}{2}1_{A_1},...,D^\frac{1}{2}1_{A_k})$ 构成。

**数据分布于最小固有值间的关系**

![数据分布于最小固有值间的关系](/images/cvml-数据聚类-二/2020-02-24-09-34-13.png)

**最小固有值于固有向量可视化**

![最小固有值于固有向量可视化](/images/cvml-数据聚类-二/2020-02-24-09-36-45.png)

# 基于非负矩阵因子分解的聚类算法(Nonenegative Matrix Factorization)

## 单线性 NMF 模型(Single Linear NMF Model)

- 输入：

  - 需要聚类的数据集：$ \boldsymbol D = (x_1, x_2, ..., x_n) $
  - 群的个数 $ k $， $k \ll n$

- 假设：

  - 每个数据点 $x_i$ 都是一个 $m$ 维特征向量：
    
    $$x_i = [x_{1i}, x_{2i}, ... , x_{mi}]^T$$

  - 每个群的中心点课表示为：
  
    $$u_k = [u_{1k}, u_{2k}, ... , u_{mk}]^T$$

### 中心思想

每个群的中心 $u_k$ 代表一种具体概念，而每一个数据点 $x_i$ 是这些概念的线性组合：

$$ x_i = v_{i1}u_1, v_{i2}u_2, ... , v_{ik}u_k$$

写成矩阵形式，就有:

$$ \boldsymbol X = \boldsymbol U \cdot \boldsymbol V^T$$

其中，
-  $\boldsymbol X$ 是 $m \times n$ 的**数据集矩阵** $\boldsymbol X = [x_1, x_2, ... , x_n]$

- $\boldsymbol U$ 是 $m \times n$ 的**非负中心点矩阵** $\boldsymbol U = [u_1, u_2, ... , u_k]$
  
- $V^T$ 是 $k \times n$ 的非负矩阵 $V^T = [v_1, v_2,...,v_n], v_i = [v_{i1}, v_{i2}, ...,v{ik}]^T$

### 损失函数及求解算法

**损失函数**：

$$J = \frac{1}{2} \|X - UV^T\|^2$$

我们的目标是将 $X$ 分解为两个非负矩阵 $U, V$ 的乘积，使得上述损失函数最小。

**求解算法**：使用标准的非负矩阵分解算法便可求解，最终结果如下：

$$
\begin{aligned}
&u_{ij} \leftarrow u_{ij} \frac{(XV)_{ij}}{(UV^TV)_{ij}} \\
&v_{ij} \leftarrow v_{ij} \frac{(X^T U)_{ij}}{(VU^T U)_{ij}}
\end{aligned}
$$

### 解的讨论

损失函数 $J$ 的解并不唯一，如果 $U$ 和 $V$ 是 $J$ 的解，则对任意正对角矩阵 $D$ 来说，$UD$ 和 $VD^{-1}$ 同样是 $J$ 的解。

为了使解唯一，归一化 $U$ 的列向量：

$$
\begin{aligned}
&v_{ij} \leftarrow v_{ij} \sqrt{\sum_i u_{ij}^2} \\\\
&u_{ij} \leftarrow \frac{u_{ij}}{\sqrt{\sum_i u_{ij}^2}}
\end{aligned}
$$

矩阵 $V$ 的元素 $v_{ij}$ 表示数据点 $i$ 对群 $j$ 的归属程度。如果数据点 $i$ 只属于群 $j$，则 $v_{ij}$ 的值将很大，而 $v_i = [v_{i1}, v_{i2}, ..., v_{ik}]^T$ 其余元素的值则很小。

### 聚类算法

- 为数据集 $D$ 构建数据集矩阵 $X$，它的第 $i$ 列代表数据点 $i$ 的特征向量

- 用非负矩阵分解（NMF）算法将 $X$ 分解为两个非负矩阵 $U$ 和 $V$
  
- 对矩阵 $U$ 和 $V$ 进行归一化
  
- 用 $V$ 来确定每个数据点 $i$ 的群标签。具体来说，找出矩阵 $V^T$ 的列向量 $v_i$ 的最大元素 $c, c = \arg \max \limits_j v_{ij}$ 然后将数据点 $i$ 分配到第 $c$ 个群中。

## 双线性 NMF 模型(Bilinear NMF Model)

在单线性 NMF 模型中，对聚类中心非负的要求约束着适用场合。因此，我们提出了双线性 NMF 模型。

### 中心思想

每个群的中心 $r_c$ 代表一种具体概念，而每一个数据点 $x_i$ 是这些概念的线性组合，用数学公式表示，就是：

$$ x_i = v_{i1}r_1, v_{i2}r_2, ... , v_{ik}r_k = \sum_{c=1}^k c_{ic}r_c
\tag{1}
$$

每个群的中心点 $r_c$ 是所有数据点 $x_i$ 的线性组合：

$$
r_c = \omega_{1c}x_1 + \omega_{2c}x_2 + \cdots + \omega_{nc}x_n
    = \sum_{i=1}^n \omega_{ic} x_i
\tag{2}
$$

将 $(2)$ 代入 $(1)$ 可以得到：

$$
x_i = \sum_{c=1}^k v_{ic} r_c = \sum_{c=1}^k v_{ic} \sum_{j=1}^n x_j \omega_{jc}
$$

写成矩阵形式，得到：

$$
X \approx XWV^T
$$

$W = [\omega_{ic}]$ 是 $n \times k$ 的关联矩阵。

### 损失函数及求解算法

**损失函数**：

$$J = \frac{1}{2} \|X - XWV^T\|^2$$

我们的目标是将 $X$ 分解为两个非负矩阵 $W, V$ ，使得上述损失函数最小。

**求解算法**：使用标准的非负矩阵分解算法便可求解，最终结果如下：

$$
\omega_{ij} \leftarrow \omega_{ij} \frac{(KV)_{ij} +  \sqrt{(KV)_{ij}^2 + 4P_{ij}^+ P_{ij}^-}}{2P_{ij}^+}
\tag{3}
$$

其中，$K = X^TX; K^+, K^-$ 是每个元素都为证书的堆成矩阵，且满足 $K = K^+ - K^-$。另外，$P^+ = K^+ WV^T V, P^- = K^- WV^T V$。

由于矩阵 $K$ 的全部元素均为正，也就是说 $K^- = 0$，对 $(3)$进行简化，得:

$$
\omega_{ij} \leftarrow \omega_{ij} \frac{(KV)_{ij}}{P_{ij}}
$$

类似的，也可以求得矩阵 $V$ 的迭代式以及简化式：

$$
\begin{aligned}
&v_{ij} \leftarrow v_{ij} \frac{(KW)_{ij} +  \sqrt{(KW)_{ij}^2 + 4Q_{ij}^+ Q_{ij}^-}}{2Q_{ij}^+} \tag{4}\\
&v_{ij} \leftarrow v_{ij} \frac{(KW)_{ij}}{Q_{ij}^+}
\end{aligned}
$$

其中，$Q^+ = VW^TK^+ W, Q^- = VW^TK^- W$

与单线性模型类似，由于最小化损失函数 $J$ 的解并不唯一，我们需要对 $W$ 和 $V$ 进行归一化，加入约束 $\|r_c = 1\|$ 之后，得标准化公式：

$$
V \leftarrow V[diag(W^TKW)]^\frac{1}{2} \tag{5}
$$

$$
W \leftarrow W[diag(W^TKW)]^{-\frac{1}{2}} \tag{6}
$$

### 核函数的导入

上述算法中，$K = X^TX, k_{ij} = x_i \cdot x_j$

除了这种标准的核函数之外，我们还可定义任意的核函数：$k_{ij} = \Kappa(x_i, x_j)$，可以使用任意的核函数这一性质可以显著提高双线性模型的聚类精度。

## 基于双线性 NMF 模型的聚类算法

1. 为数据集 $D$ 构建数据集矩阵 $X$，其第 $i$ 列代表数据点 $i$ 的特征向量。
2. 使用适当的核函数 $\Kappa(x_i, x_j)$ 构造核矩阵 $K$。
3. 通过式 $(3)$ 与 $(4)$ 交替迭代，求得损失函数 $J$ 的最优解。
4. 利用式 $(5)$ 和 $(6)$ 对 $W$ 和 $V$ 进行归一化。
5. 用 $V^T = [v_1, v_2, ..., v_n]$ 来确定每个数据点的群标签。具体来说，找出矩阵每个列向量 $v_i$ 的最大要素，$x = \arg \max_c v_{ic}$，将数据点 $i$ 分配至第 $x$ 个群中。 

# 谱聚类 VS. NMF聚类

![](/images/cvml-数据聚类-二/2020-03-15-23-08-02.png)

![](/images/cvml-数据聚类-二/2020-03-15-23-08-35.png)

# 更多

参考[科研学习-计算机视觉与机器学习](/categories/科研学习-计算机视觉与机器学习/)