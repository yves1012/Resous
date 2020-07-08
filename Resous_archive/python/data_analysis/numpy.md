# Numpy介绍

## 为什么会有Numpy
Python中用列表(list)保存一组值，可以用来当作数组使用，不过由于列表的元素可以是任何对象，因此列表中所保存的是对象的指针。这样为了保存一个简单的[1,2,3]，需要有3个指针和三个整数对象。对于数值运算来说这种结构显然比较浪费内存和CPU计算时间。

此外Python还提供了一个array模块，array对象和列表不同，它直接保存数值，和C语言的一维数组比较类似。但是由于它不支持多维，也没有各种运算函数，因此也不适合做数值运算。

所以Numpy就这么登场了，NumPy是Python的一种开源的数值计算扩展。这种工具可用来存储和处理大型矩阵，比Python自身的嵌套列表（nested list structure)结构要高效的多（该结构也可以用来表示矩阵（matrix））。 

NumPy的主要对象是同种元素的多维数组。这是一个所有的元素都是一种类型、通过一个正整数元组索引的元素表格(通常是元素是数字)。在NumPy中维度(dimensions)叫做轴(axes)，轴的个数叫做秩(rank)。

Numpy的所有的函数docs，[点击](https://docs.scipy.org/doc/numpy/genindex.html)查看。

## Numpy的核心array对象以及创建array的方法
### array对象的背景
* Numpy的核心数据结构，就叫做array就是数组，array对象可以是一维数组，也可以是多维数组。
* Python的List也可以实现相同的功能，但是array比List的优点在于性能好、包含数组元数据信息、大量的便捷函数。
* Numpy成为事实上的Scipy、Pandas、Scikit-Learn、Tensorflow、PaddlePaddle等框架的“通用底层语言”。
* Numpy的array和Python的List的一个区别，是它元素必须都是同一种数据类型，比如都是数字int类型，这也是Numpy高性能的一个原因。

### 创建array的方法
* 从Python的列表List和嵌套列表创建array。

```
# 创建一个一维数组，也就是Python的单元素List
x = np.array([1, 2, 3, 4, 5, 6, 7, 8])
print(x)
```

```
# 创建一个二维数组，也就是Python的嵌套List
x = np.array(
    [
        [1, 2, 3, 4],
        [5, 6, 7, 8]
    ]
)
print(x)
```

* 使用预定函数arange、ones/ones_like、zeros/zeros_like、empty/empty_like、full/full_like、eye等函数创建。

```
# 使用arange创建数字序列
# arange([start,] stop[, step,], dtype=None)
x = np.arange(10)
print(x)

y = np.arange(2, 10, 2)
print(y)
```

```
# 使用ones创建全是1的数组
# np.ones(shape, dtype=None, order='C')
# shape : int or tuple of ints Shape of the new array, e.g., (2, 3) or 2.
x = np.ones(10)
print(x)

y = np.ones((2, 3))
print(y)
```

```
# 使用ones_like创建形状相同的数组
# ones_like(a, dtype=float, order='C')
print(np.ones_like(x))
print(np.ones_like(X))
```

```
# 使用zeros创建全是0的数组
# np.zeros(shape, dtype=None, order='C')
print(np.zeros(10))
print(np.zeros((3, 4)))
```

```
# 使用zeros_like创建形状相同的数组
# np.zeros_like(a, dtype=None)
print(np.zeros_like(x))
print(np.zeros_like(X))
```

```
# 使用empty创建全是0的数组
# empty(shape, dtype=float, order='C')
# 注意：数据是未初始化的，里面的值可能是随机值不要用
print(np.empty(10))
print(np.empty((3, 4)))
```

```
# 使用empty_like创建形状相同的数组
# empty_like(prototype, dtype=None)
print(np.empty_like(x))
print(np.empty_like(X))
```

```
# 使用full创建指定值的数组
# np.full(shape, fill_value, dtype=None, order='C')
print(np.full(10, 666))
print(np.full((3, 4), 888))
```

```
# 使用full_like创建形状相同的数组¶
# np.full_like(a, fill_value, dtype=None)

print(np.full_like(x, 666))
print(np.full_like(X, 666))
```

* 生成随机数的np.random模块构建。


```
# 使用random模块生成随机数的数组
# randn(d0, d1, ..., dn)
print(np.random.randn())
print(np.random.randn(3))
print(np.random.randn(3, 4))
print(np.random.randn(3, 3, 4))
```


### array本身的属性
* shape：返回一个元组，表示array的维度。

```
# 创建一个一维数组，也就是Python的单元素List
x = np.array([1, 2, 3, 4, 5, 6, 7, 8])
print(x)

# 创建一个二维数组，也就是Python的嵌套List
X = np.array(
    [
        [1, 2, 3, 4],
        [5, 6, 7, 8]
    ]
)
print(X)

print(x.shape)
print(X.shape)

# (8,)
# (2, 4)
```

* ndim：一个数字，表示array的维度的数目。

```
print(x.ndim)
print(X.ndim)

# 1
# 2
```

* size：一个数字，表示array中所有数据元素的数目。

```
print(x.size)
print(X.size)

# 8
# 8
```

* dtype：array中元素的数据类型。

```
print(x.dtype)
print(X.dtype)

# int64
# int64

```

### array本身支持的大量操作和函数

* 直接逐元素的加减乘除等算数操作。

```
A = np.arange(10).reshape(2, 5)
print(A)
print(A + 3)
print(A * 2)
print(A ** 2)
```

* 更好用的面向多维的数组索引。
* 求sum/mean等聚合函数。
* 线性代数函数，比如求解逆矩阵、求解方程组。

## Numpy对数组按索引查询
```
import numpy as np

x = np.arange(20)
X = np.arange(20).reshape(4, 5)
print(x)
print(X)
```

### 基础索引
```
# 一维数组
# 和Python的List一样
print(x[2], x[5], x[-1])
print(x[2:4])
print(x[2:-1])
print(x[-3:])
```
```
# 二维数组
# 分别用行坐标、列坐标，实现行列筛选
print(X[0, 0])
print(X[-1, 2])
print(X[:-1])  # 筛选多行
print(X[:2, 2:4])  # 筛选多行，然后筛选多列
print(X[:, 2])  # 筛选所有行，然后筛选多列
```
```
# 注意：切片的修改会修改原来的数组
# 原因：Numpy经常要处理大数组，避免每次都复制
x[2:4] = 666
print(x)
```

### 神奇索引
```
# 其实就是：用整数数组进行的索引，叫神奇索引
# 一维数组
x = np.arange(10)
print(x)
print(x[[3, 4, 7]])
indexs = np.array([[0, 2], [1, 3]])
print(x[indexs])
```
```
# 二维数组
X = np.arange(20).reshape(4, 5)
print(X[[0, 2]])  # 筛选多行，列可以省略
print(X[[0, 2], :])
print(X[:, [0, 2, 3]])  # 筛选多列，行不能省略
print(X[[0, 2, 3], [1, 3, 4]])  # 同时指定行列-列表
```

### 布尔索引
```
# 一维数组
x = np.arange(10)
print(x > 5)
print(x[x > 5])
```

```
# 二维数组
X = np.arange(20).reshape(4, 5)
print(X > 5)  # X>5的boolean数组，既有行，又有列
print(X[X > 5])
print(X[X[:, 3] > 5])
```

## Numpy常用random随机函数

官方文档[地址](https://docs.scipy.org/doc/numpy-1.14.0/reference/routines.random.html)。

* rand(d0, d1, ..., dn)：返回数据在[0, 1)之间，具有均匀分布

```
print(np.random.rand(5))
print(np.random.rand(3, 4))
print(np.random.rand(2, 3, 4))
```

* randn(d0, d1, ..., dn)：返回数据具有标准正态分布（均值0，方差1）

```
print(np.random.randn(5))
print(np.random.randn(3, 4))
print(np.random.randn(2, 3, 4))
```

* randint(low[, high, size, dtype])：生成随机整数，包含low，不包含high，如果high不指定，则从[0, low)中生成数字

```
print(np.random.randint(3))
print(np.random.randint(1, 10))
print(np.random.randint(10, 30, size=(5,)))
print(np.random.randint(10, 30, size=(2, 3, 4)))
```

* random([size])：生成[0.0, 1.0)的随机数

```
print(np.random.random(5))
print(np.random.random(size=(3, 4)))
print(np.random.random(size=(2, 3, 4)))
```

* choice(a[, size, replace, p])：a是一维数组，从它里面生成随机结果

```
print(np.random.choice(5, 3))  # 这时候，a是数字，则从range(5)中生成，size为3
print(np.random.choice(5, (2, 3)))
print(np.random.choice([2, 3, 6, 7, 9], 3))  # 这时候，a是数组，从里面随机取出数字
print(np.random.choice([2, 3, 6, 7, 9], (2, 3)))
```

* shuffle(x)：把一个数组x进行随机排列

```
# 一维数组
a = np.arange(10)
np.random.shuffle(a)
print(a)

# 如果是多维数组，则只会在第一维度上随机
A = np.arange(20).reshape(4, 5)
np.random.shuffle(A)
print(A)
```

* permutation(x)：把一个数组x进行随机排列，或者数字的全排列

```
print(np.random.permutation(10))  # 这时候，生成range(10)的随机排列
print(np.random.permutation(np.arange(9).reshape((3, 3))))  # 这时候，在第一维度进行打散
# 注意，这里不会更改原来的arr，会返回一个新的copy
```

* normal([loc, scale, size])：按照平均值loc和方差scale生成高斯分布的数字

```
print(np.random.normal(1, 10, 10))
print(np.random.normal(1, 10, (3, 4)))
```

* uniform([low, high, size])：在[low, high)之间生成均匀分布的数字

```
print(np.random.uniform(1, 10, 10))
print(np.random.uniform(1, 10, (3, 4)))
```

## Numpy的数学统计函数
### Numpy有哪些数学统计函数

函数名 | 说明
---|---
np.sum | 所有元素的和
np.prod | 所有元素的乘积
np.cumsum | 元素的累积加和
np.cumprod | 元素的累积乘积
np.min | 最小值
np.max | 最大值
np.percentile | 0-100百分位数
np.quantile | 0-1分位数
np.median | 中位数
np.average | 加权平均，参数可以指定weights
np.mean | 平均值
np.std | 标准差
np.var | 方差

```
arr = np.arange(12).reshape(3, 4)
print(np.sum(arr))  # 所有元素的和
print(np.prod(arr))  # 所有元素的乘积
print(np.cumsum(arr))  # 元素的累积加和
print(np.cumprod(arr))  # 元素的累积乘积
print(np.min(arr))  # 最小值
print(np.max(arr))  # 最大值
print(np.percentile(arr, [25, 50, 75]))  # 0-100百分位数
print(np.quantile(arr, [0.25, 0.5, 0.75]))  # 0-1分位数
print(np.median(arr))  # 中位数
weights = np.random.rand(*arr.shape)  # 加权平均，参数可以指定weights
np.average(arr, weights=weights)  # weights的shape需要和arr一样
print(np.mean(arr))  # 平均值
print(np.std(arr))  # 标准差
print(np.var(arr))  # 方差
```

### 怎样实现按不同的axis计算
以上函数，都有一个参数叫做axis用于指定计算轴为行还是列，如果不指定，那么会计算所有元素的结果。

axis=0代表行、axis=1代表列，对于sum/mean/media等聚合函数：
* axis=0代表把行消解掉，axis=1代表把列消解掉。

```
arr = np.arange(20).reshape(4, 5)
print(arr)
print(arr.sum(axis=0))
print(arr.sum(axis=1))
```

## Numpy计算数组中满足条件元素个数

```
arr = np.random.randint(1, 10000, size=int(1e8))
print(arr[:10])
# 计算下结果，用于对比是否准确
print(arr[arr > 5000].size)
```

## Numpy怎样给数组增加一个维度
* 背景

很多数据计算都是二维或三维的，对于一维的数据输入为了形状匹配，经常需升维变成二维。

* 需要

在不改变数据的情况下，添加数组维度；（注意观察这个例子，维度变了，但数据不变）

原始数组：一维数组arr=[1,2,3,4]，其shape是(4,)，取值分别为arr[0],arr[1],arr[2],arr[3]。

变形数组：二维数组arr[[1,2,3,4]]，其shape实(1,4), 取值分别为a[0,0],a[0,1],a[0,2],a[0,3]。

* 实操的3种方法

np.newaxis：关键字，使用索引的语法给数组添加维度。

```
# np.newaxis：关键字，使用索引的语法给数组添加维度
# 注意：np.newaxis其实就是None的别名
# print(np.newaxis is None)
arr = np.arange(5)
# 给一维向量添加一个行维度
print(arr[np.newaxis, :])
# 给一维向量添加一个列维度
print(arr[:, np.newaxis])
```

np.expand_dims(arr,axis)：方法，和np.newaxis实现一样的功能，给arr在axis位置添加维度。

```
arr = np.arange(5)
# np.expand_dims方法实现的效果，和np.newaxis关键字是一模一样的
# 给一维数组添加一个行维度
print(np.expand_dims(arr, axis=0))
# 给一维数组添加一个列维度
print(np.expand_dims(arr, axis=1))
```

np.reshape(a, newshape)：方法，给一个维度设置为1完成升维。

```
arr = np.arange(5)
# 给一维数组添加一个行维度
print(np.reshape(arr, (1, 5)))
print(np.reshape(arr, (1, -1)))  # -1表示让Numpy自己计算有几列

# 给一维数组添加一个列维度
print(np.reshape(arr, (-1, 1)))  # -1表示让Numpy自己计算有几行
```

## Numpy重要的数组合并操作
背景：在给机器学习准备数据的过程中，经常需要进行不同来源的数据合并的操作。

两类场景：

* 给已有的数据添加多行，比如增添一些样本数据进去；
* 给已有的数据添加多列，比如增添一些特征进去；

以下操作均可以实现数组合并：
* np.concatenate(array_list, axis=0/1）：沿着指定axis进行数组的合并。
* np.vstack或者np.row_stack(array_list)：垂直vertically、按行row wise进行数据合并。
* np.hstack或者np.column_stack(array_list)：水平horizontally、按列column wise进行数据合并。

```
# 给数据添加新的多行
a = np.arange(6).reshape(2, 3)
b = np.random.randint(10, 20, size=(4, 3))

print(np.concatenate([a, b]))
print(np.vstack([a, b]))
print(np.row_stack([a, b]))

# 给数据添加新的多列
A = np.arange(12).reshape(3, 4)
B = np.random.randint(10, 20, size=(3, 2))
print(np.concatenate([A, B], axis=1))
print(np.hstack([A, B]))
print(np.column_stack([A, B]))
```

## Numpy怎么对数组进行排序
Numpy给数组排序有三种方法：
* numpy.sort：返回排序后数组的拷贝。
* array.sort：原地排序数组而不是返回拷贝。
* numpy.argsort：间接排序，返回的是排序后的数字索引。

三种方法都支持一个参数kind，可以是以下一个值：
* quicksort：默认值，快速排序，平均O(nlogn)，不稳定情况。
* mergesort：归并排序，平均O(nlogn)，稳定排序。
* heapsort：堆排序，平均O(nlogn)，不稳定排序。
* stable：稳定排序。

```
arr = np.array([1, 2, 5, 3, 8, 6, 9, 3])
print(np.sort(arr)) # 返回排序后的拷贝数组

arr.sort()  # 原地排序
print(arr)

indices = np.argsort(arr)  # 获得排序元素对应的索引数字列表
print(arr[indices])  # 直接获取对应的数据列表
```

## Numpy怎么实现数组的乘法
按照两个相乘数组的维度的不同，分为以下几种乘法：
* 数字与一维/二维数组相乘。
* 一维数组与一维数组相乘。
* 一维数组与二维数组相乘。
* 二维数据与二维数组相乘。

### 乘法函数
* *符号或者np.multily：逐元素乘法，对应位置的元素相乘，要求shape相同。
* @符号或者np.matmul：矩阵乘法，形状要求满足(n, k),(k, m)—>（n, m）。
* np.dot：点积乘法（也叫内积或数量积），两个向量a=[a1, a2,···，an]和b=[b1, b2, ···, bn]的点积定义为：a·b = a1b1 + a2b2 + ··· + anbn。

```
# 数字与一维/二维数组相乘
A = np.arange(10)  # 一维数组
print(A * 0.5)

B = np.arange(12).reshape(3, 4)
print(B * 0.5)

# 一维数组与一维数组相乘
A = np.arange(1, 11)
B = np.arange(11, 21)
print(np.multiply(A, B))  # 逐元素乘法
print(A * B)
print(np.matmul(A, B))
print(A @ B)
print(np.dot(A, B))

# 一维数组与二维数组相乘
A = np.arange(1, 5)
B = np.arange(1, 21).reshape(5, 4)
print(A@B)

```