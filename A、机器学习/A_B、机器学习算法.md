<span id="re_"></span>
## A_B、机器学习算法
* [KNN](#KNN)
* [朴素贝叶斯](#朴素贝叶斯)
* [逻辑斯蒂回归](#逻辑斯蒂回归)
* [支持向量机SVM](#支持向量机SVM)
* [决策树](#决策树)
* [集成学习](#集成学习)
  * [bagging和boosting](#bagging和boosting)
  * [Adaboost算法](#Adaboost算法)
  * [前向分步算法](#前向分步算法)
  * [GBDT](#GBDT)
  * [XGBoost算法](#XGBoost算法)
* [聚类](#聚类)
* [隐马尔可夫模型HMM](#隐马尔可夫模型HMM)
* [条件随机场CRF](#条件随机场CRF)
* [EM算法](#EM算法)
* [参考](#参考)
<span id="KNN"></span>
# [KNN](#re_)
* 两种方法
  * 蛮力求解欧式距离：适合小数据
  * KD树求解 ：适合大数据，通过减少计算欧式距离的次数来减小计算量
* K的选取
  * 一般较小，过大时会出现所有的数据都会被分成类别最多的训练数据的类别
  * K值小时对异常值敏感

## [朴素贝叶斯](#re_)
* 要求：[贝叶斯定理](https://blog.csdn.net/weixin_43824059/article/details/87934733)，特征条件独立假设
* 解析
  * 生成式模型，学习输入和输出的联合概率分布
  * 给定输入x，利用贝叶斯概率定理求出最大的后验概率作为输出y

<span id="逻辑斯蒂回归"></span>
## [逻辑斯蒂回归](#re_)
在**伯努利分布**的假设下，通过**极大似然**的方法，运用**梯度下降**求解参数，从而达到二分类目的  
logistic回归模型：  
$$\hat{y_i}=\sigma\left(w x_{i}+b\right)$$  
$$\sigma(x)=\frac{1}{1+e^{-x}}$$  
loss函数：  
$$-\frac{1}{n} \sum_{i=1}^{n}\left(y_{i} \log \hat{y_i}+\left(1-y_{i}\right) \log \left(1-\hat{y_i}\right)\right)$$  
[参考链接：逻辑回归的推导](https://www.cnblogs.com/lxs0731/p/8573044.html)   
### [逻辑回归实现多分类](https://blog.csdn.net/sinat_36811967/article/details/84378246)   
* **one-vs-one**(OVO)
  * n(n-1)/2个分类器
  * 每个分类器预测类别，投票得到最终类别
* **one-vs-rest**(OVR)
  * n个分类器
  * 每个分类器预测每个类别的概率，概率最大的即是最终的预测结果
* **softmax**
  * 输出预测（可以是某张图片）属于各个类别的概率，各类别总概率和为1
  ![softmax](https://i.ibb.co/RBbBSdY/softmax.jpg)

### 线性回归与LR的区别
* **线性回归**
  * 用来预测
  * **最小二乘来计算参数**
  * 受异常值影响
* **LR**
  * 用来分类
  * **最大似然估计来计算参数**
  * 对异常值有很好的稳定性

<span id="支持向量机SVM"></span>
## [支持向量机SVM](#re_)
[参考链接：手撕SVM](https://blog.csdn.net/Dominic_S/article/details/83002153)  
**寻找最优超平面在空间中分割数据**；  分割超平面满足的条件：离其最近的点到其的距离最大化  
**支持向量**：训练数据集中与分离超平面距离最近的样本点的实例  
**核函数**  
<div align=center><img src="https://github.com/FangChao1086/machine_learning/blob/master/依赖文件/核函数.jpg"/></div>

**<details><summary>支持向量机的分类</summary>**

* 线性可分支持向量机
  * 当训练数据**线性可分**时，通过**硬间隔最大化**，学习一个线性分类器，即线性可分支持向量机，又称硬间隔支持向量机 
* 线性支持向量机
  * 当训练数据**接近线性可分**时，通过**软间隔最大化**，学习一个线性分类器，即线性支持向量机，又称软间隔支持向量机
* 非线性支持向量机
  * 当训练数据**线性不可分**时，通过使用**核技巧**及软间隔最大化，学习非线性支持向量机
</details>  

**<details><summary>硬间隔、软间隔表达式</summary>**  
* 硬间隔
$$\min_{w, b} \frac{1}{2}\|\|w\|\|^{2}$$  
$$st.\ \ \ \ y^{(i)}\left(w^{T} x^{(i)}+b\right) \geq 1$$  
* 软间隔
$$\min_{w, b} \frac{1}{2}\|\|w\|\|^{2}+C \sum_{i=1}^{m} \xi_{i}$$  
$$st.\ \ \ \ y^{(i)}\left(w^{T} x^{(i)}+b\right) \geq 1-\xi_{i}$$  
$$\ \ \ \ \ \ \ \xi_{i} \geq 0$$  
</details>

### SVM推导
* 如何根据**间隔最大化**的目标导出 SVM 的**标准问题**  
* 拉格朗日乘子法对偶问题的求解过程
  
**符号定义**  
* 训练集 T
  * $T=\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), ... ,\left(x_{N}, y_{N}\right)$  
* 分离超平面 (w,b)
  * $w^{* } \cdot x+b^{* }=0$  
  * 使用映射函数：$w^{* } \cdot \Phi(x)+b^{* }=0$  
  映射函数 Φ(x) 定义了从输入空间到特征空间的变换，特征空间通常是更高维的，甚至无穷维；方便起见，这里假设 Φ(x) 做的是恒等变换  
* 分类决策函数 f(x)
  * $f(x)=\operatorname{sign}\left(w^{* } \cdot x+b^{* }\right)$
  
**函数间隔、几何间隔**  
* 函数间隔$\hat{\gamma}_ {i}$，函数间隔最小值（超平面关于训练数据集的函数间隔）：  
$$\hat{\gamma}=\min_{i=1, \cdots, N}, y_{i}\left(w x_{i}+b\right)=\min_{i=1, \cdots, N}, \hat{\gamma}_ {i}$$
* 几何间隔$\gamma_{i}$，几何间隔最小值（超平面关于训练数据集的几何间隔）：  
$$\gamma=\min_{i=1, \cdots, N}, y_{i}\left(\frac{w}{\|\|w\|\|} x_{i}+\frac{b}{\|\|w\|\|}\right)=\min_{i=1, \cdots, N}, \frac{\hat{\gamma_{i}}}{\|\|w\|\|}$$  

**最大化几何间隔**  
$$\max_{w, b} \quad \gamma; \quad \text { s.t. } \quad y_{i}\left(\frac{w}{\|\|w\|\|} x_{i}+\frac{b}{\|\|w\|\|}\right) \geq \gamma, \quad i=1,2, \cdots, N$$  
也就是：  
$$\max_{w, b} \frac{\hat{\gamma}}{\|\|w\|\|}; \quad \text { s.t. } \quad y_{i}\left(w x_{i}+b\right) \geq \hat{\gamma}, \quad i=1,2, \cdots, N$$    
函数间隔$\hat{\gamma}$的取值不会影响最终的超平面(w,b)：取$\hat{\gamma}$=1；  
* (ω,b)等比例改变，超平面不会改变，但函数间隔$\hat{\gamma}$会成比例改变，因此可以通过等比例改变(ω,b)使函数间隔等于1  

**线性支持向量机的最优化问题**  
$$\min_{w, b} \frac{1}{2}\|\|w\|\|^{2}$$  
$$s.t.\quad y_{i}\left(w \cdot x_{i}+b\right)-1 \geq 0, \quad i=1,2, \cdots, N$$  

这是一个**凸二次优化**问题，可以直接求解；但是为了简便，我们要应用用拉格朗日对偶性，求解它的对偶问题  

求解对偶问题的优点：  
* 对偶问题更易求解，因为不用求W
* 自然引入核函数，推广到线性不可分分类问题上  

**<details><summary>对偶问题求解</summary>**
* 构建**拉格朗日函数**  
$$L(w, b, \alpha)=\frac{1}{2} w^{T} w-\sum_{i=1}^{N} \alpha_{i}\left[y_{i}\left(w^{T} x_{i}+b\right)-1\right] \quad \alpha_{i} \geq 0, \quad i=1,2, \cdots, N$$  

* 根据原始问题的对偶性，原始问题的对偶是求极大极小问题：  
  * 原始问题：$$\min_{w, b} \max_{\alpha} L(w, b, \alpha)$$  
  * 对偶问题：$$\max_{\alpha} \min_{w, b} L(w, b, \alpha)$$  

* 求 L 对 (w,b) 的极小  
$$\quad \frac{\partial L}{\partial w}=0 \Rightarrow w-\sum_{i=1}^{N} \alpha_{i} y_{i} x_{i}=0 \quad \Rightarrow w=\sum_{i=1}^{N} \alpha_{i} y_{i} x_{i}$$  
$$\quad \frac{\partial L}{\partial b}=0 \Rightarrow \sum_{i=1}^{N} \alpha_{i} y_{i}=0$$  

* 代入L，有：  
$$\begin{aligned} L(w, b, \alpha) &=\frac{1}{2} w^{T} w-\sum_{i=1}^{N} \alpha_{i}\left[y_{i}\left(w^{T} x_{i}+b\right)-1\right] \\ &=\frac{1}{2} w^{T} w-w^{T} \sum_{i=1}^{N} \alpha_{i} y_{i} x_{i}-b \sum_{i=1}^{N} \alpha_{i} y_{i}+\sum_{i=1}^{N} \alpha_{i} \\ &=\frac{1}{2} w^{T} w-w^{T} w+\sum_{i=1}^{N} \alpha_{i} \\ &=-\frac{1}{2} w^{T} w+\sum_{i=1}^{N} \alpha_{i} \\&=-\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} \cdot y_{i} y_{j} \cdot x_{i}^{T} x_{j}+\sum_{i=1}^{N} \alpha_{i} \end{aligned}$$

* 对偶问题（L 对 α 的极大）
$$\max_{\alpha}\ \ \ \ -\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} \cdot y_{i} y_{j} \cdot x_{i}^{T} x_{j}+\sum_{i=1}^{N} \alpha_{i}$$  
$$s.t.\ \ \ \ \quad \sum_{i=1}^{N} \alpha_{i} y_{i}=0, \alpha_{i} \geq 0, \quad i=1,2, \cdots, N$$  

* 设 α 的解为 α\*，则存在下标j使α_j > 0，得到w，b：
  $$w^{* }=\sum_{i=1}^{N} \alpha_{i}^{* } y_{i} x_{i}$$  
  $$b^{* }=y_{j}-\sum_{i=1}^{N} \alpha_{i}^{* } y_{i}\left(x_{i}^{T} x_{j}\right)$$  
 
* 分离超平面及分类决策函数为：  
  * $w^{* } \cdot x+b^{* }=0$
  * $f(x)=\operatorname{sign}\left(w^{* } \cdot x+b^{* }\right)$
</details>

**软间隔最大化**  
<div align=center>
 
|C值|惩罚|划分错误的点|形状|  
|-----|-----|-----|-----|  
|大|大|少|瘦|  
|小|小|多|胖|  
 </div>

**合页损失（hinge损失）**  
* 当样本被正确分类且函数间隔大于1时，合页损失才是0，否则损失是1-y(wx+b)。
* 合页损失函数**不仅要正确分类**，而且**确信度足够高时损失才是0**。也就是说，合页损失函数对学习有更高的要求。
### SVM损失函数
* 合页损失函数加上正则化项
  $$\sum_{i}^{N}\left[1-y_{i}\left(w \cdot x_{i}+b\right)\right]_ {+ }+\lambda\|\|w\|\|^{2}$$  

### 核函数
**核函数的种类**
* **线性核函数**
  * 选择情况：**特征维数高**
  * 选择情况：**样本数量非常多**（避免非常庞大的计算量）
* **多项式核函数**
  * 多项式形式的核函数具有良好的全局性质
* **高斯核函数（RBF）**
  * 选择情况：**样本数量可观，特征少**
  * Gauss径向基函数则是局部性强的核函数
* sigmoid核函数
  * 采用Sigmoid函数作为核函数时，支持向量机实现的就是一种多层感知器神经网络
  
**核函数的选择**
* 样本特征多，维数高：线性核
* 样本数量较多，特征少：手动进行特征组合，线性核
* 样本数量较少且维度不高：高斯核

**核函数的作用：核函数隐含着一个低维空间到高维空间的映射，这个映射可以将低维空间中线性不可分的点变成线性可分**  

**SVM使用对偶函数求解的原因**
* 方便引入核函数
* 原始问题，求解问题的复杂度只与有样本的维度相关；对偶问题，只与样本的数量有关

**SVM与LR的区别**
* LR
  * 参数模型，损失函数为逻辑损失
  * 考虑所有的样本
* SVM
  * 非参数模型，损失函数为hinge损失
  * 只考虑与分类最相关的少数支持向量
### SVM实现多分类
[参考链接：SVM实现多分类](https://www.cnblogs.com/CheeseZH/p/5265959.html)  

<span id="决策树"></span>
## [决策树](#re_)
### 特征选择
* 分类树
  * ID3决策树：优先选择**信息增益大**的属性（偏向于特征取值多的特征，而信息增益比则抵消了特征变量的复杂程度）
  * C4.5:信息增益比
  * CART:gini指数最小化
* 回归树
  * CART:平方误差最小化
### CART算法
* 在给定输入随机变量 X 条件下输出随机变量 Y 的**条件概率分布**的学习方法
* 假设决策树是**二叉树**，内部节点特征的取值为“是”和“否”，左分支：是；右分支：否；
  * 这样的决策树等价于递归地二分每个特征，**将输入空间/特征空间划分为有限个单元**，然后在这些单元上确定在输入给定的条件下输出的条件概率分布。
* CART 决策树既**可以用于分类，也可以用于回归**；

<details><summary>决策树相关问题</summary>

* 信息熵、基尼指数都可以表示数据的不确定性；CART使用基尼指数的原因：
  * 信息熵需要计算对数，计算量大，但是可以处理多个类别
  * 基尼指数针对两个类别进行计算，CART树是一个二叉树
* 决策树怎剪枝：
  * **预剪枝**
    * 决策树的构建过程中加入限制；比如控制叶子节点最少的样本个数
  * **后剪枝**
    * 决策参数构建完成后，根据加上正则项的结构风险最小化自下而上进行的剪枝操作
* 决策树的优缺点：
  * 优点：模型可读性好，具有描述性，效率高
  * 缺点：对中间的缺失值敏感，容易产生过拟合
    
</details>

<span id="集成学习"></span>
## [集成学习](#re_)
* 基本思想：由多个学习器组合成一个性能更好的学习器

<span id="bagging和boosting"></span>
### bagging和boosting  
**1. Bagging：降低方差**  
* **并行策略** ：基学习器之间不存在依赖关系，可同时生成
* 代表算法：  
  * <details><summary>随机森林</summary>
 
    * 基学习器为决策树
    * 每次建树特征个数随机选择
    * 每次建树样本个数随机选择
    * 分类：k棵树投票
    * 回归：k棵树求平均
    </details>  
  * 神经网络中的dropout策略
* 基于**bootstrap**：从一个数据集中有放回的抽取N次，每次抽M个
  
**2. Boosting：降低偏差**  
* **串行策略** ：基学习器之间存在依赖关系，新的学习器需要依据旧的学习器生成
* 从某个基学习器出发，反复学习，得到一系列基学习器，然后组合它们构成一个强学习器
* 代表算法:
  * 提升方法Adaboost
  * 梯度提升树GBDT
* Boosting 策略要解决的两个基本问题:
  * 每一轮如何**改变数据的权值**或概率分布？
  * 如何将弱分类器组合成一个强分类器？

**<details><summary>3. stacking和blending</summary>** 
* stacking
  * 数据划分成两个不相交的集合
  * 第一个集合的数据集中训练多个模型
  * 第二个集合的数据测试这些模型
  * 将预测结果作为输入，将正确标签作为输出，再训练一个高层模型
* blending
  * 数据划分成多个不相交的集合
  * 用多个不相交的集合训练不同的基模型，加权平均输出结果
 </details>

**bagging和boosting的区别**  
* 样本选择
  * bagging每一轮训练集在原始集中有放回的选取，boosting每一轮的训练集不变
* 样例权重
  * bagging样例权重相等，boosting根据分错的样例不断调整样例权重，提高分错的样例权重
* 预测函数
  * bagging预测函数权重相等，boosting权重不同，错误率小的分类器有更大的权重
* 并行计算
  * bagging可以并行，boosting不能并行，后一个模型需要前一个模型的结果

<span id="Adaboost算法"></span>
### Adaboost算法
**本质**：通过前一步训练**更新样本权重**，使用新的权重样本训练得到新的弱分类器（包括此弱分类器在集成分类器中的系数）;最后**弱分类器加权组合**，集成强分类器  

**步骤介绍：**  
* **初始化**训练样本的权值分布  
* 构造训练集，**训练弱分类器**  
  * **样本被分类正确，在下一个训练集中权值就会降低**
  * **样本被分类错误，在下一个训练集中权值就会提高**
  * 更新训练集用于训练下一个分类器
* 把训练好的弱分类器**集成**为一个强分类器  
  * **误差率小的弱分类器系数大**
  * **误差率大的弱分类器系数小**

**详细步骤：**  
给定样本数量：m; label:{-1,1}  
* 初始化训练样本的权值  
$$D(1)=(\omega_{11},\omega_{12},...,\omega_{1m});  \omega_{1i}=\frac{1}{m};i=1,2,…,m$$  
* 多轮迭代产生T个弱分类器  
对于每一个`t = 1,2,..,T:`
  * 使用`D(t)`的训练集进行训练，得到一个弱分类器  
  $$G_t(x):\chi\rightarrow{-1,+1}$$  
  * 计算弱分类器`G_t(x)`的分类误差率  
  $$e_t=P(G_t(x_i)\neq{y_i})=\sum_{i=1}^{m}\omega_{ti}I(G_t(x_i)\neq{y_i})$$  
  * 计算弱分类器在最终分类器中的系数  
  $$\alpha_t=\frac{1}{2}ln{\frac{1-e_t}{e_t}}$$  
  * 更新训练数据集的权值分布  
  $$D(t+1)=\left (\omega_{t+1,1},\omega_{t+1,2},...,\omega_{t+1,m}\right)$$
    * `for i=1,...,m:`  
      * 更新各个样本的权值系数  
      $$\omega_{t+1,i}=\frac{\omega_{t,i}e^{(-\alpha_t y_iG_t(x_i))}}{Z_t}$$  
      * `z_t`是规范化因子  
      $$Z_{t}=\sum_{i=1}^{m} \omega_{t, i} \exp \left(-\alpha_{t}y_i G_{t}\left(x_{i}\right)\right)$$  
* 集成T个弱分类器为强分类器  
$$G(x)=sign\left(\sum_{t=1}^{T}\alpha_tG_t\left(x\right)\right)$$  
<details><summary>Adaboost相关问题</summary>

* Adaboost能够提高整体模型的学习精度的原因：  
  * 根据前向分布加法模型，虽然Adaboost单个模型误差会有波动，但是Adaboost每一次都会提高模型的整体复杂度，降低整体误差
* Adaboost使用m个基学习器与加权平均使用m的学习器的不同：
  * Adaboost的m个基学习器是有顺序的，实际上是同一个学习器针对**不同的数据分布**学习而来的；
  * 加权平均是m个学习器针对**同一个数据分布**学习到的
* Adaboost与GBDT:
  * 相同：
    * 降低偏差
    * 前向分布加法模型的一种
  * 不同：
    * Adaboost:通过调整错分数据点的权重来改进模型
    * GBDT:从负梯度的方向去拟合改进模型
* Adboost的优缺点：
  * 优点：基于泛化性能弱的学习器构建强的集成，不容易过拟合
  * 缺点：对异常样本敏感
</details>

<span id="前向分步算法"></span>
### 前向分步算法
---
#### 加法模型
* 加法模型：  
$$f(x)=\sum_{m=1}^{M} \beta_{m} b\left(x ; \gamma_{m}\right)$$  
  * 基函数：$b\left(x ; \gamma_{m}\right)$
  * 基函数的参数：$\gamma_{m}$
  * 基函数的系数：$\beta_{m}$
* 在给定训练数据和损失函数```L(y,f(x))```的情况下，学习加法模型相当于损失函数的最小化问题  
$$\min_{\beta_{m},\gamma_{m}} \sum_{i=1}^{N} L\left(y_{i},\sum_{m=1}^{M} \beta_{m} b\left(x ; \gamma_{m}\right)\right)$$  
#### 算法描述
思想：从前向后，每一步只学习一个基函数及其系数，逐步优化目标函数  
* 输入：训练集`T={(x1,y1),..,(xN,yN)}`，损失函数`L(y,f(x))`，基函数集`{b(x;γ)}`
* 输出：加法模型`f(x)`  

1. 初始化$f_0(x)=0$    
2. 对$m=1,2,..,M$
  * 极小化损失函数，得到$(\beta_m,\gamma_m)$  
  $$\left(\beta_{m},\gamma_{m}\right)=\arg \min_{\beta, \gamma}\sum_{i=1}^{N} L\left(y_{i},f_{m-1}\left(x_{i}\right)+\beta b\left(x_{i};\gamma\right)\right)$$  
  * 更新模型 $f_m(x)$  
  $$f_{m}(x)=f_{m-1}(x)+\beta b(x ; \gamma)$$  
3. 得到加法模型  
$$f(x)=f_{M}(x)=\sum_{m=1}^{M} \beta_{m} b\left(x ; \gamma_{m}\right)$$  
* 前向分步算法将同时求解$m=1,2,..,M$所有参数$(\beta_m,\gamma_m)$的问题简化为逐次求解各$(\beta_m,\gamma_m)$的优化问题——思想上有点像梯度下降  
#### 前向分步算法与 AdaBoost
* AdaBoost 算法是前向分步算法的特例。
* 此时，基函数为基分类器，损失函数为指数函数```L(y,f(x)) = exp(-y*f(x))```

<span id="GBDT"></span>
### GBDT  
[参考链接：GBDT算法原理以及实例理解](https://mp.weixin.qq.com/s/f9yqkWHOjTtzHfknC6elFA)  
全称Gradient Boosting Decision Tree。一种基于决策树实现的**分类回归**算法  

* GBDT 是以决策树（**CART回归树**）为基学习器、采用 Boosting 策略的一种集成学习模型
* **与提升树的区别**：残差的计算不同，提升树使用的是真正的残差，梯度提升树用当前模型的**负梯度**来拟合残差。  
#### 提升树 Boosting Tree
---
* 以**决策树**为基学习器，对分类问题使用二叉分类树，回归问题使用二叉回归树。
* 解决回归问题时，通过不断拟合残差得到新的树。
* 提升树模型可表示为**决策树的加法模型**：  
<a href="https://www.codecogs.com/eqnedit.php?latex=f_M(x)=\sum_{m=1}^MT(x;\Theta_m)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f_M(x)=\sum_{m=1}^MT(x;\Theta_m)" title="f_M(x)=\sum_{m=1}^MT(x;\Theta_m)" /></a>  
* 首先初始化提升树```f_0(x)=0```，则第 m 步的模型为  
<a href="https://www.codecogs.com/eqnedit.php?latex=f_m(x)=f_{m-1}(x)&plus;T(x;\Theta_m)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f_m(x)=f_{m-1}(x)&plus;T(x;\Theta_m)" title="f_m(x)=f_{m-1}(x)+T(x;\Theta_m)" /></a>  
* 然后通过最小化损失函数决定下一个决策树的参数  
<a href="https://www.codecogs.com/eqnedit.php?latex=\hat{\Theta}_m=\arg\underset{\Theta_m}{\min}\sum_{i=1}^NL(y_i,{\color{Red}&space;f_{m-1}(x_i)&plus;T(x_i;\Theta_m)})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\hat{\Theta}_m=\arg\underset{\Theta_m}{\min}\sum_{i=1}^NL(y_i,{\color{Red}&space;f_{m-1}(x_i)&plus;T(x_i;\Theta_m)})" title="\hat{\Theta}_m=\arg\underset{\Theta_m}{\min}\sum_{i=1}^NL(y_i,{\color{Red} f_{m-1}(x_i)+T(x_i;\Theta_m)})" /></a>  
* 对于二分类问题，提升树算法只需要将[AdaBoost算法](#Adaboost算法)中的基学习器限制为二叉分类树即可  

**提升树算法描述**
在回归问题中，新的树是通过不断**拟合残差**（residual）得到的。
* 输入：训练集```T={(x1,y1),..,(xN,yN)}, xi ∈ R^n, yi ∈ R```    
* 输出：回归提升树```f_M(x)```  
1. 初始化```f_0(x)=0```  
2. 对```m=1,2,..,M```  
  * 计算**残差**  
  <a href="https://www.codecogs.com/eqnedit.php?latex={\color{Red}&space;r_{m,i}}=y_i-f_{m-1}(x_i),\quad&space;i=1,2,..,N" target="_blank"><img src="https://latex.codecogs.com/gif.latex?{\color{Red}&space;r_{m,i}}=y_i-f_{m-1}(x_i),\quad&space;i=1,2,..,N" title="{\color{Red} r_{m,i}}=y_i-f_{m-1}(x_i),\quad i=1,2,..,N" /></a>  
  * **拟合残差**学习下一个回归树的参数  
  <a href="https://www.codecogs.com/eqnedit.php?latex=\hat{\Theta}_m=\arg\underset{\Theta_m}{\min}\sum_{i=1}^N&space;L({\color{Red}&space;r_{m,i}},{\color{Blue}&space;T(x_i;\Theta_m)})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\hat{\Theta}_m=\arg\underset{\Theta_m}{\min}\sum_{i=1}^N&space;L({\color{Red}&space;r_{m,i}},{\color{Blue}&space;T(x_i;\Theta_m)})" title="\hat{\Theta}_m=\arg\underset{\Theta_m}{\min}\sum_{i=1}^N L({\color{Red} r_{m,i}},{\color{Blue} T(x_i;\Theta_m)})" /></a>  
  * 更新```f_m(x)```  
  <a href="https://www.codecogs.com/eqnedit.php?latex=f_m(x)=f_{m-1}(x)&plus;T(x;\Theta_m)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f_m(x)=f_{m-1}(x)&plus;T(x;\Theta_m)" title="f_m(x)=f_{m-1}(x)+T(x;\Theta_m)" /></a>  
3. 得到最终学习器  
<a href="https://www.codecogs.com/eqnedit.php?latex=f_M(x)=f_0(x)+\sum_{m=1}^MT(x;\Theta_m)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f_M(x)=f_0(x)+\sum_{m=1}^MT(x;\Theta_m)" title="f_M(x)=f_0(x)+\sum_{m=1}^MT(x;\Theta_m)" /></a>  

**梯度提升算法（GB）**
* 当损失函数为平方损失或指数损失时，每一步的优化是很直观的；但对于一般的损失函数而言，不太容易——梯度提升正是针对这一问题提出的算法；
* 梯度提升是梯度下降的近似方法，其关键是利用**损失函数的负梯度**作为**残差的近似值**，来拟合下一个决策树。

#### GBDT算法描述
* 输入：训练集```T={(x1,y1),..,(xN,yN)}, xi ∈ R^n, yi ∈ R```；损失函数```L(y,f(x))```；
* 输出：回归树```f_M(x)```
1. 初始化回归树  
<a href="https://www.codecogs.com/eqnedit.php?latex=f_0(x)={\color{Red}&space;\arg\underset{c}{\min}}\sum_{i=1}^NL(y_i,c)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f_0(x)={\color{Red}&space;\arg\underset{c}{\min}}\sum_{i=1}^NL(y_i,c)" title="f_0(x)={\color{Red} \arg\underset{c}{\min}}\sum_{i=1}^NL(y_i,c)" /></a>  
2. 对```m=1,2,..,M```
  * 对```i=1,2,..,N```，计算残差/负梯度  
  <a href="https://www.codecogs.com/eqnedit.php?latex=r_{m,i}=-\frac{\partial&space;L(y_i,{\color{Red}&space;f_{m-1}(x_i)}))}{\partial&space;{\color{Red}&space;f_{m-1}(x_i)}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?r_{m,i}=-\frac{\partial&space;L(y_i,{\color{Red}&space;f_{m-1}(x_i)}))}{\partial&space;{\color{Red}&space;f_{m-1}(x_i)}}" title="r_{m,i}=-\frac{\partial L(y_i,{\color{Red} f_{m-1}(x_i)}))}{\partial {\color{Red} f_{m-1}(x_i)}}" /></a>  
  * 对```r_mi```拟合一个回归树，得到第 m 棵树的叶节点区域  
  
  <a href="https://www.codecogs.com/eqnedit.php?latex=R_{m,j},\quad&space;j=1,2,..,J" target="_blank"><img src="https://latex.codecogs.com/gif.latex?R_{m,j},\quad&space;j=1,2,..,J" title="R_{m,j},\quad j=1,2,..,J" /></a>  
  
  * 对```j=1,2,..,J```，计算  
  
  <a href="https://www.codecogs.com/eqnedit.php?latex=c_{m,j}={\color{Red}&space;\arg\underset{c}{\min}}\sum_{x_i\in&space;R_{m,j}}L(y_i,{\color{Blue}&space;f_{m-1}(x_i)&plus;c})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?c_{m,j}={\color{Red}&space;\arg\underset{c}{\min}}\sum_{x_i\in&space;R_{m,j}}L(y_i,{\color{Blue}&space;f_{m-1}(x_i)&plus;c})" title="c_{m,j}={\color{Red} \arg\underset{c}{\min}}\sum_{x_i\in R_{m,j}}L(y_i,{\color{Blue} f_{m-1}(x_i)+c})" /></a>  
  
  * 更新回归树  
  
  <a href="https://www.codecogs.com/eqnedit.php?latex=f_m(x)=f_{m-1}&plus;\sum_{j=1}^J&space;c_{m,j}{\color{Blue}&space;I(x\in&space;R_{m,j})}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f_m(x)=f_{m-1}&plus;\sum_{j=1}^J&space;c_{m,j}{\color{Blue}&space;I(x\in&space;R_{m,j})}" title="f_m(x)=f_{m-1}+\sum_{j=1}^J c_{m,j}{\color{Blue} I(x\in R_{m,j})}" /></a>  
  
3. 得到最终学习器  
<a href="https://www.codecogs.com/eqnedit.php?latex=f_M(x)=f_0(x)&plus;\sum_{i=1}^M\sum_{j=1}^Jc_{m,j}{\color{Blue}&space;I(x\in&space;R_{m,j})}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f_M(x)=f_0(x)&plus;\sum_{i=1}^M\sum_{j=1}^Jc_{m,j}{\color{Blue}&space;I(x\in&space;R_{m,j})}" title="f_M(x)=f_0(x)+\sum_{i=1}^M\sum_{j=1}^Jc_{m,j}{\color{Blue} I(x\in R_{m,j})}" /></a>

* 说明： 
  * 算法第 1 步初始化，估计使损失函数最小的常数值，得到一棵只有一个根节点的树
  * 第 2(i) 步计算损失函数的负梯度，将其作为残差的估计 
    * 对平方损失而言，负梯度就是残差；对于一般的损失函数，它是残差的近似
  * 第 2(ii) 步估计回归树的节点区域，以拟合残差的近似值
  * 第 2(iii) 步利用线性搜索估计叶节点区域的值，使损失函数最小化

<details><summary>GBDT相关问题</summary>

* GBDT使用负梯度代表残差的原因：
  * GBDT损失函数：  
  $$\begin{array}{c}{L\left(y, f_{m}(x)\right)=L\left(y, f_{m-1}(x)+\beta_{m} b\left(x ; \gamma_{m}\right)\right)} \\ {=L\left(y, f_{m-1}(x)\right)+\frac{\partial L\left(y, f_{m-1}(x)\right)}{\partial f_{m-1}(x)}\left(f_{m}(x)-f_{m-1}(x)\right)} \\ {=L\left(y, f_{m-1}(x)\right)+\frac{\partial L\left(y, f_{m-1}(x)\right)}{\partial f_{m-1}(x)}\left(\beta_{m} b\left(x ; \gamma_{m}\right)\right)}\end{array}$$  
  * 目标：损失函数最小化；$L\left(y, f_{m-1}(x)\right)$是个常数；要使损失函数最小，则  
  $$\left(\beta_{m} b\left(x ; \gamma_{m}\right)\right)=-\frac{\partial L\left(y, f_{m-1}(x)\right)}{\partial f_{m-1}(x)}$$  
  * 由公式可知第m棵树使用前m-1棵树的负梯度作为残差
* GBDT训练过程如何选择特征：
  * 数值连续特征使用最小均方误差
  * 离散特征使用gini指数
* GBDT与RF的区别：  

||**RF**|**GBDT**|  
|-----|-----|-----|
|**基分类器**|可以是**分类树/回归树**|只能是**回归树**；会累加所有树的结果，来构建新树|  
|**方差偏差**|**减少方差**|**减少偏差**|  
|**训练样本**|有放回抽样|全部数据|
|**集成学习**|bagging-树粒度上**并行**|boosting-树粒度上**串行**|  
|**最终结果**|**投票/平均**|将所有结果**加权融合**|  
|**数据敏感性**|**不敏感异常值**|**敏感异常值**|  
|**泛化能力**|不易过拟合|容易过拟合|

</details>

### XGBoost算法
详细推导：  
* 目标函数  
![xgb_1](https://i.ibb.co/d5DWZFR/xgboost-1.png) ![xgb_2](https://i.ibb.co/tYbBf5F/xgboost-2.png)  
  * 模型的复杂度：**叶子节点的数目**和**叶子节点输出Score的L2模的平方**  
  * 模型参数：1、树的结构	2、叶节点的分数（权重）  
* 加法训练：先优化一棵树，再优化另一棵树，依次优化完K棵树  
![xgb_3](https://i.ibb.co/ZmYtsVJ/xgboost-3.png)  
  * 第t步添加了一棵最优CART树，这棵树是从前t-1棵树产生：  
  ![xgb_4](https://i.ibb.co/kXmWcZR/xgboost-4.png)  
  * 由泰勒公式：  
  ![xgb_5](https://i.ibb.co/7JdHmsq/xgboost-5.png)  
  * 此时的目标函数：  
  ![xgb_6](https://i.ibb.co/bNHcmYw/xgboost-6.png)  
  * 第t棵树是优化后的树，即t为变量可优化，上式中的中括号以内是前t-1棵树优化完旧已经确定的常量，因此优化函数为：  
  ![xgb_7](https://i.ibb.co/9wXYyLr/xgboost-7.png)  
  ![xgb_8](https://i.ibb.co/W6frr7r/xgboost-8.png)![xgb_9](https://i.ibb.co/xHpk1kh/xgboost-9.png)  
* 求最优值：  
![xgb_10](https://i.ibb.co/1bHW62L/xgboost-10.png)![xgb_11](https://i.ibb.co/7Jpx2Qy/xgboost-11.png)    
* 上式是一个二次式，可求出叶子节点的最佳值和目标函数的值：  
  ![xgb_12](https://i.ibb.co/mJfX8bR/xgboost-12.png)  

求树结构：  
![xgb_13](https://i.ibb.co/SRNf641/xgboost-13.png)  
切分点：  
![xgb_14](https://i.ibb.co/q1GmMBJ/xgboost-14.png) **T(split)-T(nosplit)=1**  

<details><summary>XGBoost相关问题</summary>
 
* xgboost相对GBDT的改进：**损失函数进行了二阶泰勒展开（为了更为精准的逼近真实损失函数）、目标函数加入正则项、支持并行和默认缺失值处理**等  
  * **基分类器**：传统GBDT以CART决策树，xgboost还支持**线性分类器**
  * **导数信息**：传统GBDT只用到了一阶导数信息，xgboost对损失函数做了**二阶泰勒展开**，xgboost还支持自定义损失函数，只要函数可一阶二阶可导
  * **正则项**：xgboost损失函数里增加了**正则项**，相当于预剪枝，防止过拟合
  * **列抽样**，xgboost支持列采样（使用部分特征），借鉴随机森林，防止过拟合
  * **缺失值处理**：针对缺失值可以自动学习分裂方向
  * **支持并行**；并行在特征粒度上而非tree粒度上，在训练之前，预先将每个特征按特征值排好序，存储为块结构，分裂节点时采用多线程并行查找每个特征的最佳分割点
* xgboost如何处理缺失值
  * 对于某单个特征，将其没有缺失的样本进行遍历
  * 缺失该特征的样本分别划分到左叶子结点和右叶子结点，选择分裂增益大的那个方向，为预测时特征值缺失样本的默认分支方向
  * 训练时没有缺失值预测时出现缺失，那么自动将缺失值划分到右子节点
* xgboost树停止生长的条件
  * 新的分裂gain<0时
  * 树达到最大深度时
  * 叶子结点包含的样本数量小于设定值
* xgboost优缺点：
  * 缺失值：自动处理缺失值
  * 并行：特征列预排序后，以块的形式存储在内存中，在迭代时可以重复使用，在处理每个特征时可以做到并行
  * 占用内存大：很多叶子节点的分裂增益低，没必要在进行分裂，带来了不必要的开销
</details>

<details><summary>lightgbm相关问题</summary>

* lightgbm相对xgboost的改进：
  * xgboost:level-wise分裂策略，对一层的所有节点做无差别分裂，带来了不必要的开销；lightgbm:leaf-wise分裂策略，在所有的叶子节点中选择分裂收益最大的节点进行分裂
  * 直方图算法；牺牲了一定的切分准确性，换取训练速度
  * 训练决策树时**计算切分点的增益时**，xgboost的预排序需要对每个样本的切分位置进行计算，lightgbm将样本离散成直方图，只需要计算直方图的切割位置
  * **直方图差加速**，一个子节点的直方图可以通过父节点的直方图减去兄弟节点的直方图得到，从而加速计算。所以每次分裂只需计算分裂后样本数较少的子节点的直方图然后通过做差的方式获得另一个子节点的直方图，进一步提高效率
  * 节省内存  
  * 稀疏特征优化、支持类别特征  
</details>

<details><summary>catboost相关问题</summary>

* catboost采用特殊的方式**自动处理类别型特征**（categorical features）。首先对categorical features做一些统计，计算某个类别特征（category）出现的频率，之后加上超参数，生成新的数值型特征（numerical features）
</details>

<span id="聚类"></span>
## [聚类](#re_)
聚类算法：原型聚类（kmeans）、学习向量量化（Learning Vector Quatization）、高斯混合聚类（Mixture-of-Gaussian）、密度聚类（DBSCAN）、层次聚类（AGNES）
### [KMeans聚类算法原理总结](https://mp.weixin.qq.com/s/2o9JapW9X_Yx9TwHRE0QJA)  
* 普通KMeans  
  * **<details><summary>k-means步骤</summary>**
 
    * 初始化簇的质心
    * 对单个数据点，求到各个质心的距离，找到最近的质心，该点则是属于该质心形成的簇中的点
    * 对于更新后簇中的每个点重新计算质心，更新质心
    * 重复2，3
    * 直到每个点所属的簇都不发生改变，或者最大迭代次数后停止  
    </details>  
  * 优点：效率高，适用于大规模数据  
  * 缺点：
    * 结果易受**初始质心**影响，局部最优
    * 结果易受**k值选取**影响
    * <details><summary>详细</summary>
 
      |缺点|改进|描述|  
      |-----|-----|-----|  
      |K值的确定|ISODATA|当属于某个簇的样本数过少，将该簇去除；当属于某个簇的样本数过多，分散程度较大时将该簇分为两个子簇；|   
      |对奇异点敏感|K-median|中位数代替平均值作为簇中心|  
      |只能找到球状群|GMM|以高斯分布考虑簇内数据点的分布|  
      |分群结果不稳定|k-means++|初始聚类中心之间的距离要尽可能的远|  
      </details>
  
* k-means++：在KMeans基础上，**初始化簇的质心方法不同**
  * 随机选择一个点作为簇的质心
  * 计算对每个样本数据点到各个簇质心的距离，选最小的距离
  * 选择这些所有样本点最小距离中的最大距离，将此样本点定为新簇的质心
  * 重复2，3
  * 直到簇质心数目达到预设值，再使用普通KMeans算法聚类  
* MiniBatchKMeans
  * 无放回随机抽样进行聚类，减少收敛时间 
* 层次聚类：
  * 拆分层次聚类（二分KMeans）：[Hierarchical clustering（分层聚类）](https://github.com/machinelearningmindset/machine-learning-course/blob/master/docs/source/content/unsupervised/clustering.rst#hierarchical)  
    * 初始化：全部的数据是一个簇，只有一个质心
    * **找到使划分后sse最低的簇,对该簇划分为两个簇**，当只有一个簇时，则对这个簇进行划分  
      * **划分时用到Kmeans,将单个簇划分为两个簇**  
      * $$S S E=\sum_{i=1}^{k} \sum_{p \in C_{i}}\left|p-m_{i}\right|^{2}$$
        * $C_i$：第i个簇；p是$C_i$中的样本点；$m_i$是$C_i$的质心，SSE是所有样本的聚类误差
  * 聚合层次聚类
    * 假设每一个样本点都是一个簇类，然后聚合

**<details><summary>k-means不适用的场景</summary>**
* 样本数据各向异性
* 样本数据集是非凸数据集
* 训练数据各个簇间的标准差不相等
* 各簇类样本数目相差较大
* 数据维数过大，先考虑PCA降维
</details>

#### [K-means聚类选取最优k值的方法](https://blog.csdn.net/qq_15738501/article/details/79036255)    
* 手肘法
  * 思想：利用SSE;**当k小于真实聚类数时，由于k的增大会大幅增加每个簇的聚合程度，故SSE的下降幅度会很大，而当k到达真实聚类数时，再增加k所得到的聚合程度回报会迅速变小，所以SSE的下降幅度会骤减**，然后随着k值的继续增大而趋于平缓，也就是说SSE和k的关系图是一个手肘的形状，而**这个肘部对应的k值就是数据的真实聚类数**
* 轮廓系数法
  * 思想：平均轮廓系数最大的k便是最佳聚类数  

**簇质心k个数的选取**
* 评估指标：sklearn.metrics.calinski_harabaz_score(X,y_pred),簇间距离和簇内距离之比 越大越好
#### k-means与KNN
* k-means:无监督（无需标记数据），要训练
* knn：监督（标记数据），无需训练
### DBSCAN
* 基于密度的聚类算法

<span id="隐马尔可夫模型HMM"></span>
## [隐马尔可夫模型HMM](#re_)
**生成模型**  
隐藏的马尔可夫链随机生成**状态序列**，每个状态随机生成一个观测，由此产生**观测序列**  
* 初始状态概率向量pai
* 状态转移概率矩阵A
* 观测概率矩阵B  

其中pai和A决定状态序列，B决定观测序列  
### 维特比算法
用动态规划解隐马尔可夫模型**预测问题**，求最优路径（概率最大路径），也就是概率最大状态序列
* 输入：模型与观测
* 输出：最优路径

### 隐马尔科夫模型的三个基本问题：
* **计算问题**
  * 已知模型参数，计算观测序列Y出现的概率
* **预测问题**
  * 已知模型参数与观测序列，计算最有可能的隐状态(维特比算法)
* **学习问题**
  * 已知观测序列Y，求解使观测序列概率最大的模型参数（包括隐状态，隐状态之间的转移概率，隐状态到观测状态的概率分布）

<span id="条件随机场CRF"></span>
## [条件随机场CRF](#re_)
**判别模型**  

<span id="EM算法"></span>
## [EM算法](#re_)
* 解决隐含变量优化问题的有效方法
* jensen不等式确定下界

<span id="参考"></span>
## [参考](#re_)
① https://github.com/datawhalechina/Daily-interview  
② https://www.nowcoder.com/tutorial/95
