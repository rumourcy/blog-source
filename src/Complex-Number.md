---
title: Complex Number
date: 2018-01-15 11:18:43
tags: 
  - Complex Analysis
---
### 复数概述
#### 复数表示
- 笛卡儿坐标系
> $$ z=a+ib $$
> $a$表示实部，$b$表示虚部
> 可以将 $a+ib$ 看成xy平面上以$(a, b)$为坐标的点，或者等价地看作连接原点到此点的向量
> 这样的平面记作$C$，称为复平面
![complex plan](https://github.com/trierbo/blog-source/raw/master/pics/complex-number/complex_plan.png)

- 极坐标
> $$ z=r\angle\theta $$
> $r=|z|$, $\theta=\arg z$

<!--more-->

#### 复数算术
- 几何算术
  - 加法法则：两个复数之和$A+B$通常由向量加法的平行四边形法则给出
  - 乘法法则：$AB$之长是$A$之长与$B$之长的乘积，$AB$的辐角是$A$与$B$的辐角之和
  > 极坐标表示几何乘法法则：
  > $$ (R \angle \phi)(r \angle \theta) = (Rr) \angle (\phi\theta) $$

![operation](https://github.com/trierbo/blog-source/raw/master/pics/complex-number/operation.png)

- 符号算术
  - 加法法则：$A+B = (a+ib)+(c+id) = (a+c)+i(b+d)$
  - 乘法法则：$(a+ib)(c+id) = ac+i(ad+bc)+i^2{bd}$
  
> 两者是等价的

#### 术语与记号
| 名称          | 含意              | 记号         |
| :-----------: | :-----------:     | :----------: |
| $z$的模       | $z$的长度$r$      |              |
| $z$的辐角     | $z$的角度$\theta$ | $ \arg z $   |
| $z$的实部     | $z$的$x$坐标      | $ Re(z) $    |
| $z$的虚部     | $z$的$y$坐标      | $ Im(z) $    |
| 虚数          | $i$的实数倍       |              |
| 实轴          | 实数的集合        |              |
| 虚轴          | 虚数的集合        |              |
| $z$的复共轭   | $z$对实轴的反射   |$\overline{z}$|

![sign](https://github.com/trierbo/blog-source/raw/master/pics/complex-number/sign.png)

### 欧拉公式
$$ e^{i\theta} = \cos\theta + i\sin\theta $$

#### 物理意义
> 这个公式表明$e^{i\theta}$是单位圆上辐角为$\theta$的一点
> ![mean](https://github.com/trierbo/blog-source/raw/master/pics/complex-number/ei.png)
> 因此，
> $$ z = r\angle\theta $$
> 可以写为
> $$ z = re^{i\theta} $$
> 则复数乘法的几何法则可表示为：
> $$ (Re^{i\phi})(re^{i\theta}) = Rre^{i(\theta+\phi)} $$

#### 泰勒公式
- 佩亚诺(Peano)型余项
![Peano](https://github.com/trierbo/blog-source/raw/master/pics/complex-number/peano.png)
- 拉格朗日(Lagrange)型余项
![Lagrange](https://github.com/trierbo/blog-source/raw/master/pics/complex-number/Lagrange.png)

#### 证明
**利用上述泰勒公式进行证明**
> 把函数$e^x$,$\cos x$和$\sin x$利用泰勒公式展开：
> ![ex](https://github.com/trierbo/blog-source/raw/master/pics/complex-number/ex.png)
> ![cosx](https://github.com/trierbo/blog-source/raw/master/pics/complex-number/ecossin.png)
> ![sinx](https://github.com/trierbo/blog-source/raw/master/pics/complex-number/sinx.png)
> 将$x=iz$带入$e^x$得：
> ![e^iz](https://github.com/trierbo/blog-source/raw/master/pics/complex-number/eiz.png)

### 参考链接
*复分析 可视化方法*
[Euler's formula](https://en.wikipedia.org/wiki/Euler%27s_formula)
[欧拉公式](https://zh.wikipedia.org/wiki/%E6%AC%A7%E6%8B%89%E5%85%AC%E5%BC%8F)
