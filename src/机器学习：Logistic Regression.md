---
title: 机器学习：Logistic Regression
date: 2019-05-05 19:50:20
tags:
  - Machine Learning
  - ZERO
---

#### 模型

$$
f(x) = sigmoid(w \cdot x + b) = \frac{1}{1+e^{-(w \cdot x + b)}} = \frac{e^{w \cdot x + b}}{1 + e^{w \cdot x + b}}
$$

表示为条件概率分布：
$$
P(y=1|x) = \frac{e^{w \cdot x + b}}{1 + e^{w \cdot x + b}} \\
P(y=0|x) = \frac{1}{1 + e^{w \cdot x + b}}
$$

#### 代价函数

令：
$$
\hat{y}_{i} = f(x_{i})
$$
则其似然函数为：
$$
\prod_{i}{\hat{y}_{i}^{y_{i}}(1-\hat{y}_{i})^{1-y_{i}}}
$$
对数似然函数：
$$
\sum_{i} y_{i} \log \hat{y}_{i} + (1-y_{i}) \log (1-\hat{y}_{i})
$$
代价函数即交叉熵损失函数：
$$
-\sum_{i} y_{i} \log \hat{y}_{i} + (1-y_{i}) \log (1-\hat{y}_{i})
$$

#### 多分类

##### Softmax

$$
P(y=c_{k}|x) = \frac{e^{w_{k} \cdot x + b_{k}}}{\sum_{i} e^{w_{i} \cdot x + b_{i}}}
$$

##### one vs rest

训练K个分类器，每个分类器将第k类标记为正例，其余标记为负例，选择概率最大的最为最终结果

**问题**：类别不均衡问题

##### one vs one

训练$\tbinom{K}{2}$个分类器，每个分类器选择两个类别进行训练，最终通过投票选举出预测结果