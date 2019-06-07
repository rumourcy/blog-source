---
title: 机器学习：K近邻
date: 2019-06-07 15:09:32
tags: 
  - Machine Learning
---

#### 构造kd树

输入：$k$维空间数据集$T = {x_{1}, x_{2}, \cdots, x_{N}}$

输出：kd树

> - 构造根节点，根节点对应于包含$T$的$k$维空间的超矩形区域
>   - 选择$x^{(1)}$为坐标轴，中位数为切分点，将根节点划分为两个子区域，并将切分点保存在根节点上
> - 为每个子区域生成一个节点，假设节点深度为$j$，则选择$x^{(l)}$为切分坐标轴，其中$l = j % k + 1$，并将切分点保存在该节点上
> - 直到子区域没有实例停止划分

<!--more-->

#### kd树近邻搜索

输入：已构造的kd树，目标点$x$, 以及$k$

输出：$x$的$k$近邻

> - 从根节点递归向下访问，直到叶节点为止
> - 将叶节点保存到存储$k$近邻的优先队列中
> - 向上回溯，在当前节点对应的区域查找$k$近邻
>   - 如果优先队列未满，则保存当前节点，否则比较当前距离与优先队列中的最大距离
>   - 如果优先队列未满，则访问当前节点的另个子树，否则比较优先队列中的最大距离是否比到另个子区域的距离要大
> - 直到回溯到根节点

#### 代码实现

```python
import numpy as np
from scipy.spatial import distance
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import load_iris
```

```python
class Node:
    def __init__(self, x, y, depth=0, lchild=None, rchild=None):
        self.x = x
        self.y = y
        self.depth = depth
        self.lchild = lchild
        self.rchild = rchild
```

```python
class KdTree:
    def __init__(self, X, y):
        assert len(X) == len(y)
        train = np.concatenate([X, y[:, np.newaxis]], axis=1)
        
        def build_tree(train, depth=0):
            if (len(train) > 0):
                m, n = train.shape
                axis = depth % (n - 1)
                mid = m // 2
                train = np.array(sorted(train, key=lambda x: x[axis]))
                node = Node(train[mid][:-1], train[mid][-1], depth)
                node.lchild = build_tree(train[:mid], depth+1)
                node.rchild = build_tree(train[mid+1:], depth+1)
                return node
            return None
        
        self.root = build_tree(train)
        self.n = X.shape[1]
        self._count = 0
        
    def knn(self, x, k=1):
        nearest = [[-1, None] for _ in range(k)]
        self._count = 0
        def search(node):
            if node is not None:
                axis = node.depth % self.n
                daxis = x[axis] - node.x[axis]
                if daxis <= 0:
                    search(node.lchild)
                else:
                    search(node.rchild)
                    
                dist = distance.euclidean(x, node.x)
                for i, d in enumerate(nearest):
                    if d[0] < 0 or dist < d[0]:
                        nearest.insert(i, [dist, node])
                        nearest.pop()
                        self._count += 1
                        break
                
                if self._count < k or nearest[-1][0] > abs(daxis):
                    if daxis <= 0:
                        search(node.rchild)
                    else:
                        search(node.lchild)
                        
        search(self.root)
        return nearest
```

```python
iris = load_iris()
```

```python
data = pd.DataFrame(iris.data, columns=iris.feature_names)
```

```python
data.head()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length (cm)</th>
      <th>sepal width (cm)</th>
      <th>petal length (cm)</th>
      <th>petal width (cm)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>

```python
sns.scatterplot(x='sepal length (cm)', y='sepal width (cm)', hue=iris.target, data=data)
```

![iris](https://github.com/trierbo/blog-source/raw/master/pics/knn/iris.png)

```python
kdtree = KdTree(iris.data, iris.target)
x = [5., 3.25, 1.4, 0.2]
nearest = kdtree.knn(x, 5)
```


```python
cat = {}
sns.scatterplot(x='sepal length (cm)', y='sepal width (cm)', hue=iris.target, data=data)
plt.scatter(x[0], x[1], c='red', marker='x')
for n in nearest:
    print(n[1].x, "dist:", n[0])
    if n[1].y not in cat:
        cat[n[1].y] = 1
    else:
        cat[n[1].y] += 1
    plt.scatter(n[1].x[0], n[1].x[1], c='green', marker='+')
```

    [5.  3.3 1.4 0.2] dist: 0.04999999999999982
    [5.  3.4 1.5 0.2] dist: 0.18027756377319945
    [5.1 3.4 1.5 0.2] dist: 0.20615528128088284
    [4.9 3.1 1.5 0.2] dist: 0.20615528128088284
    [5.  3.2 1.2 0.2] dist: 0.20615528128088295

![iris_knn](https://github.com/trierbo/blog-source/raw/master/pics/knn/iris_knn.png)

```python
max(cat)
```


    0.0


