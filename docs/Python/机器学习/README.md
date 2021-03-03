# 一.基本概念

## 1.机器学习（machine learning）的定义

+ Arthur Samuel：在没有明确设置的情况下，使计算机具有学习能力的研究领域。
+ Tom Mitchell：计算机程序从经验**E**中学习，解决某一任务**T**，进行某一性能标度**P**，通过**P**测定在**T**上的表现因经验E而提高。

<img src="https://gitee.com/yao_zhimin/myimg/raw/master/20210302104402.png" style="zoom: 80%;" />

### 1.1 监督学习（supervised learning）

+ 回归问题（regression problem）：预测一个连续值输出
+ 分类问题（classification problem）：预测离散值输出![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302112434.png)

### 1.2 无监督学习（unsupervised learning）

+ 聚类算法（clustering）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302114549.png)

# 二、学习算法

## 1.线性回归（linear regression）

> m：训练样本的数量
>
> x：输入（特征）变量
>
> y：输出（目标）变量

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302121622.png)

+ 代价函数（Cost function）又称平方误差代价函数（square error function）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302122825.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302124413.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302130828.png)

+ 梯度下降

> := 赋值符 a:=b(将b的值赋给a)
>
> a=b(真假判定)
>
> α：学习率

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302132456.png)

> θ0 和 θ1 同时更新

+ 房价预测

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302135314.png)

## 2.多元线性回归（linear regression with multiple features）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210302160444.png)

