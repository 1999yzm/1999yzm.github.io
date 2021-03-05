





# 吴恩达机器学习

## 一.基本概念

### 1.机器学习（machine learning）的定义

+ Arthur Samuel：在没有明确设置的情况下，使计算机具有学习能力的研究领域。
+ Tom Mitchell：计算机程序从经验**E**中学习，解决某一任务**T**，进行某一性能标度**P**，通过**P**测定在**T**上的表现因经验E而提高。

<img src="https://gitee.com/yao_zhimin/myimg/raw/master/20210302104402.png" style="zoom: 80%;" />

### 2 监督学习（supervised learning）

+ 回归问题（regression problem）：预测一个连续值输出
+ 分类问题（classification problem）：预测离散值输出![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302112434.png)

### 3 无监督学习（unsupervised learning）

+ 聚类算法（clustering）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302114549.png)

## 二、学习算法

### 1.线性回归（linear regression）

> m：训练样本的数量
>
> x：输入（特征）变量
>
> y：输出（目标）变量

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302121622.png)

+ 代价函数（Cost function）又称平方误差代价函数（square error function）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303150049.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302124413.png)

> 通过简化的代价函数可得当θ1 =1 时，取得最小值

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302122825.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303145320.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302130828.png)

+ 梯度下降

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303150220.png)

> 起始点选取不同会得到不同的局部最优解

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303150639.png)

> := 赋值符 a:=b(将b的值赋给a)
>
> a=b(真假判定)
>
> α：学习率

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302132456.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303152303.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303152611.png)

> 如果初始点已经是局部最优解，则导数为0，θ值将不再变化

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303153057.png)

+ "Batch"梯度下降

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303153756.png)

+ 房价预测案例

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302135314.png)

### 2.多元线性回归（linear regression with multiple features）

> Xn：不同的特征
>
> y：要预测的输出变量
>
> n：特征量的数目
>
> x^(i)：第i个训练样本的输入特征值
>
> Xj^(i)：第i个训练样本的第j个特征量的值

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303155405.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303155747.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303155943.png)

+ 特征缩放（使得每个特征取值尽量接近-1~1之间）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303160450.png)

+ 均值归一化

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303161135.png)

+ 学习率α

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303161823.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303162001.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303162209.png)

+ 正规方程（直接一次性求解θ的最优值，不需要进行特征缩放）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303164025.png)

+ 梯度下降法和正规方程法比较

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303165240.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210303170053.png)

### 3、分类问题（Classification）

+ logistic function（sigmoid function）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210304143421.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210304143847.png)

+ logistic regression

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210304144502.png)

+ 决策边界（decision boundary）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210304145029.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210304145449.png)

> + 决策边界不是训练集的属性，而是假设本身及其参数的属性
>
> + 训练集 --> θ  -->  决策边界

+ 代价函数

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210304151432.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210304152010.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210304152418.png)

+ 多类别分类

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210304154246.png)

### 4.过拟合现象（overfitting）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210304154909.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210304155118.png)

+ 解决方案

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210304155454.png)

