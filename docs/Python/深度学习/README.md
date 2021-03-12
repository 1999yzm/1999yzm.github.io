## 1.二分分类（Binary Classification）

### 1.1 逻辑回归（Logistic Regression）

+ Lost Function（单个训练集）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210305144837.png)

+ Cost Function（全部训练集）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210305144910.png)

+ 梯度下降法（Gradient Decent）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210305145614.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210305150406.png)

+ 计算图

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210305160623.png)

+ 逻辑回归的梯度下降法（Gradient descent for logistic regression）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210305163453.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210305163554.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210305164215.png)

> 深度学习中for循环效率低
>
> 通过向量化消除显示for循环

+ 向量化（vectorization）

向量化和for循环效率对比

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210305165911.png)

> 经验法则：能不用for循环就不用for循环

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210305171349.png)

+ 向量化logistic 回归的梯度输出

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210305172648.png)

+ python中的广播机制

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210305173427.png)

# 2. 神经网络（neural network）

+ 双层神将网络（只有一个隐藏层的神经网络）

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210306101909.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210306102502.png)

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210306102827.png)

+ 不同的激活函数
  1. sigmoid function（二元分类的输出层使用）![](https://gitee.com/yao_zhimin/myimg/raw/master/20210306105846.png)
  2. tanh function（几乎在所有场合都更优越）![](https://gitee.com/yao_zhimin/myimg/raw/master/20210306110034.png)
  3. ReLu function（最常用的默认激活函数）![](https://gitee.com/yao_zhimin/myimg/raw/master/20210306110141.png)
  4. leaky ReLu ![](https://gitee.com/yao_zhimin/myimg/raw/master/20210306110227.png)

+ 神经网络的梯度下降法

![](https://gitee.com/yao_zhimin/myimg/raw/master/20210306113231.png)

