# NumPy 生成随机数组，数组的处理

[TOC]

## 生成随机数组

简单的随机数据

| 方法                                | 详情                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| `rand(d0, d1, ..., dn)`             | 给定形状中的随机值。                                         |
| `randn(d0, d1, ..., dn)`            | 从“标准正太分布”中返回数据，没有固定的参数，每多加一个数字，代表多增加一个维度 |
| `randint(low[, high, size, dtype])` | 返回随机整数数组队列                                         |
| `random_sample([size])`             | 返回半开区间内的随机浮点数[0.0，1.0 )                        |
| `random([size])`                    | 返回半开区间内的随机浮点数[0.0，1.0 )                        |
| `ranf([size])`                      | 返回半开区间内的随机浮点数[0.0，1.0 )                        |
| `sample([size])`                    | 返回半开区间内的随机浮点数[0.0，1.0 )                        |
| `choice(a[,size,repace,p])`         | 从给定的一维阵列生成一个随机样本                             |
| `bytes(length)`                     | 返回随机字节                                                 |

```python
numpy.random.rand(d0, d1, ..., dn)
```

```python
In [3]: np.random.rand(3,2)
Out[3]: 
array([[0.0571501 , 0.69419937],
       [0.00347048, 0.4418788 ],
       [0.78621141, 0.43610193]])

```

```pyrhon
numpy.random.randn(d0, d1, ..., dn)
```

```python
In [4]: np.random.randn()
Out[4]: -0.6338632720376985

In [5]: np.random.randn(2,3)
Out[5]: 
array([[ 0.78971223,  1.39805523, -1.22261271],
       [-2.43055047,  0.28744977,  0.27502438]])

```

```python
numpy.random.randint(low, high=None, size=None, dtype='l')
```

```python
Out[6]: array([1, 0, 0, 1, 0, 1, 1, 0, 1, 0])

In [7]: np.random.randint(5, size=10)
Out[7]: array([2, 0, 1, 4, 3, 2, 3, 3, 2, 1])

In [8]: np.random.randint(5, size=(2,3))
Out[8]: 
array([[0, 4, 4],
       [1, 0, 0]])
```

```python
numpy.random.random_sample(size=None)
```

```python
numpy.random.random(size=None)
```

```python
numpy.random.ranf(size=None)
```

```python
numpy.random.sample(size=None)
```

```python
In [16]: np.random.random_sample()
Out[16]: 0.7739078635457212

In [17]: np.random.random_sample((5,))
Out[17]: array([0.1841273 , 0.91038324, 0.94730618, 0.30350681, 0.1904666 ])

In [18]: 5 * np.random.random_sample((3,2)) - 5
Out[18]: 
array([[-2.03088329, -3.69477823],
       [-0.60212395, -4.23423724],
       [-3.11376556, -0.13306831]])

```

```python
numpy.random.choice(a, size=None, replace=True, p=None)
```

```python
In [22]: np.random.choice(5,3)
Out[22]: array([1, 4, 0])

In [26]: np.random.choice(5,3,p=[0, 0, 0.5,0.5, 0])
Out[26]: array([2, 2, 3])

```

```python
numpy.random.bytes(length)
```

```python
In [27]: np.random.bytes(10)
Out[27]: b'D=}\x1e\xf4\xaf\xfbp\xa3\xb7'
```

## 生成一维数字数组

| 方法                                                | 详情                       |
| --------------------------------------------------- | -------------------------- |
| `arange([start,] stop[, step][, dtype])`            | 返回均匀的数值间隔数组     |
| `linspace(start,stop[,num,endpoint, ...])`          | 以指定个数返回均匀的数值   |
| `logspace(start, stop[, num, endpoint, base, ...])` | 返回一个log的均匀分布      |
| `geomspace(start,stop[, num, endpoint, dtype])`     | 返回对数几何级数均匀分布。 |

```python
numpy.arange([start], stop[,step], dtype=None)
```

```python
Out[28]: array([0, 1, 2])

In [29]: np.arange(3.0)
Out[29]: array([0., 1., 2.])

In [30]: np.arange(3,7)
Out[30]: array([3, 4, 5, 6])
    
In [31]: np.arange(3,7,2)
Out[31]: array([3, 5])
    
In [34]: np.arange(3,10,3)
Out[34]: array([3, 6, 9])

```

```python
numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)
```

```python
In [35]: np.linspace(2.0, 3.0, num=5)
Out[35]: array([2.  , 2.25, 2.5 , 2.75, 3.  ])

In [36]: np.linspace(2.0, 3.0, num=5, endpoint=False)
Out[36]: array([2. , 2.2, 2.4, 2.6, 2.8])

In [37]: np.linspace(2.0, 3.0, num=5, retstep=True)
Out[37]: (array([2.  , 2.25, 2.5 , 2.75, 3.  ]), 0.25)

```

```python
numpy.logspace(start, stop, num=50, endpoint=True, base=10.0, dtype=None)
```

```python
In [53]: np.logspace(2.0, 3.0, num=4)
Out[53]: array([ 100.        ,  215.443469  ,  464.15888336, 1000.        ])

In [54]: np.logspace(2.0, 3.0, num=4, endpoint=False)
Out[54]: array([100.        , 177.827941  , 316.22776602, 562.34132519])

In [55]: np.logspace(2.0, 3.0, num=4, base=2.0)
Out[55]: array([4.        , 5.0396842 , 6.34960421, 8.        ])
    
In [56]: np.log10(100)
Out[56]: 2.0

```

```python
numpy.geomspace(start,stop, num=50, endpoint=True, dtype=None)
# 对数刻度（几何级数）均匀分布。
```

```python
In [57]: np.geomspace(1, 1000, num=4)
Out[57]: array([   1.,   10.,  100., 1000.])

In [58]: np.geomspace(1, 1000, num=3, endpoint=False)
Out[58]: array([  1.,  10., 100.])

In [59]: np.geomspace(1, 1000, num=4, endpoint=False)
Out[59]: array([  1.        ,   5.62341325,  31.6227766 , 177.827941  ])

In [60]: np.geomspace(1, 256, num=9)
Out[60]: array([  1.,   2.,   4.,   8.,  16.,  32.,  64., 128., 256.])

```

## 建立矩阵

| 方法                       | 详情                                                      |
| -------------------------- | --------------------------------------------------------- |
| `diag(v[,k])`              | 提取一个对角线或构建一个对角阵列                          |
| `diagflat(v[,k])`          | 创建一个二维数组，并将其输入为对角线。                    |
| `tri(N[,M,k,dtype])`       | 在给定对角线和其他地方的零点数组                          |
| `tril(m[,k])`              | 数组下三角形。返回第k个对角线归零之上的元素的数组副本。   |
| `triu(m[,k])`              | 数组的上三角形。返回第k个对角线归零下的元素的矩阵的副本。 |
| `vander(x[,N,increasing])` | 生成范德蒙德矩阵。                                        |

```python
numpy.diag(v, k=0)
```

```python
In [61]: x = np.arange(9).reshape((3,3))

In [62]: x
Out[62]: 
array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8]])

In [63]: np.diag(x)
Out[63]: array([0, 4, 8])

In [64]: np.diag(x, k=1)
Out[64]: array([1, 5])

In [65]: np.diag(x, k=-1)
Out[65]: array([3, 7])

In [66]: np.diag(np.diag(x))
Out[66]: 
array([[0, 0, 0],
       [0, 4, 0],
       [0, 0, 8]])
```

```python
numpy.diagflat(v, k=0)
```

```python
In [67]: np.diagflat([[1, 2], [3, 4]])
Out[67]: 
array([[1, 0, 0, 0],
       [0, 2, 0, 0],
       [0, 0, 3, 0],
       [0, 0, 0, 4]])

In [69]: np.diagflat([1,2], k=1)
Out[69]: 
array([[0, 1, 0],
       [0, 0, 2],
       [0, 0, 0]])

```

```python
numpy.tri(N, M=None, k=0, dtype=<class 'float'>)
```

```python
In [70]: np.tri(3, 5, 2, dtype=int)
Out[70]: 
array([[1, 1, 1, 0, 0],
       [1, 1, 1, 1, 0],
       [1, 1, 1, 1, 1]])

In [71]: np.tri(3, 5, -1)
Out[71]: 
array([[0., 0., 0., 0., 0.],
       [1., 0., 0., 0., 0.],
       [1., 1., 0., 0., 0.]])

```

```python
numpy.tril(m, k=0)
```

```python
In [72]: np.tril([[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12]], -1)
Out[72]: 
array([[ 0,  0,  0],
       [ 4,  0,  0],
       [ 7,  8,  0],
       [10, 11, 12]])

```

```python
numpy.vander(x, N=None, increasing=False)
```

```python
In [75]: x = np.array([1, 2, 3, 5])

In [76]: N = 3

In [77]: np.vander(x, N)
Out[77]: 
array([[ 1,  1,  1],
       [ 4,  2,  1],
       [ 9,  3,  1],
       [25,  5,  1]])

```

## 数组的基本操作

### 索引

`[i:j:k]` `i`为起始点，`j`为终止点，`k`为步长

`[d0, d1, ..., dn]` d0维度一，d1维度2。。。。。。

```python
In [78]: nd1 = np.random.randint(0, 150, size=(5,4,3))

In [79]: nd1
Out[79]: 
array([[[ 33,   0,  31],
        [129,  59, 131],
        [ 57, 114, 105],
        [  9,  61,  99]],

       [[145, 138,  75],
        [ 66,  19,  67],
        [136,  11,  83],
        [128, 116,   1]],

       [[ 13,   9, 128],
        [ 91, 102, 113],
        [  3,  71, 118],
        [105,  34,  93]],

       [[140,  30,  77],
        [ 11, 120,  24],
        [111,   2, 109],
        [114,  48,  16]],

       [[ 47,  30, 112],
        [146,  95,  51],
        [ 19,  58,  66],
        [106, 120,  10]]])

```

我们要获取维度为，0，1，2的值

```python
In [80]: nd1[0,1,2]
Out[80]: 131
```

获取第一维度的0 ，2， 4的值

```python
In [82]: nd1[0:6:2]
Out[82]: 
array([[[ 33,   0,  31],
        [129,  59, 131],
        [ 57, 114, 105],
        [  9,  61,  99]],

       [[ 13,   9, 128],
        [ 91, 102, 113],
        [  3,  71, 118],
        [105,  34,  93]],

       [[ 47,  30, 112],
        [146,  95,  51],
        [ 19,  58,  66],
        [106, 120,  10]]])

```

获取第一维度为1， 3，第二维度为2，第三维度倒叙输出

```python
In [83]: nd1[1:4:2,2,::-1]
Out[83]: 
array([[ 83,  11, 136],
       [109,   2, 111]])

```

### 重设形状

`reshape` 可以在不改变数组数据的同时，改变数组的形状。其中，`numpy.reshape()` 等效于 `ndarray.reshape()`。`reshape`方法非常简单：

```python
In [85]: nd1 = np.random.randint(0,150,size=(3, 4))

In [86]: nd1
Out[86]: 
array([[ 96,  78, 143, 106],
       [121,  33,  16,   6],
       [  0, 142,  24, 135]])

In [87]: nd1.reshape(6, 2)
Out[87]: 
array([[ 96,  78],
       [143, 106],
       [121,  33],
       [ 16,   6],
       [  0, 142],
       [ 24, 135]])

```

### 数组展开

`ravel` 的目的是将任意形状的数组扁平化，变为 1 维数组。`ravel` 方法如下：

```python
In [88]: nd1.ravel()
Out[88]: array([ 96,  78, 143, 106, 121,  33,  16,   6,   0, 142,  24, 135])
```

### 数组的级联

`np.concatenate() `级联 可通过axis参数改变级联的方向，默认为0, （0表示列相连,表示的X轴的事情，1表示行相连,Y轴的事情）

```python

In [89]: nd1 = np.random.randint(0, 150, size=(5,4))

In [90]: nd2 = np.random.randint(0, 150, size=(5,4))

In [91]: np.concatenate((nd1, nd2))
Out[91]: 
array([[ 87, 126,  23,  77],
       [141, 116,  44, 140],
       [149,  15,  77,   7],
       [ 78,  15,  35,  64],
       [131,   9, 120,  26],
       [  0, 142, 131, 131],
       [ 63, 123, 148,  82],
       [104, 126,  20,  51],
       [103,  87,  98, 148],
       [110,  77,  60,  72]])

In [92]: np.concatenate((nd1, nd2), axis=1)
Out[92]: 
array([[ 87, 126,  23,  77,   0, 142, 131, 131],
       [141, 116,  44, 140,  63, 123, 148,  82],
       [149,  15,  77,   7, 104, 126,  20,  51],
       [ 78,  15,  35,  64, 103,  87,  98, 148],
       [131,   9, 120,  26, 110,  77,  60,  72]])


```

`numpy.[hstack|vstack]`分别代表水平级联与垂直级联,填入的参数必须被小括号或中括号包裹

vertical垂直的 horizontal水平的 stack层积

```python
In [93]: np.hstack((nd1, nd2))
Out[93]: 
array([[ 87, 126,  23,  77,   0, 142, 131, 131],
       [141, 116,  44, 140,  63, 123, 148,  82],
       [149,  15,  77,   7, 104, 126,  20,  51],
       [ 78,  15,  35,  64, 103,  87,  98, 148],
       [131,   9, 120,  26, 110,  77,  60,  72]])

In [94]: np.vstack((nd1, nd2))
Out[94]: 
array([[ 87, 126,  23,  77],
       [141, 116,  44, 140],
       [149,  15,  77,   7],
       [ 78,  15,  35,  64],
       [131,   9, 120,  26],
       [  0, 142, 131, 131],
       [ 63, 123, 148,  82],
       [104, 126,  20,  51],
       [103,  87,  98, 148],
       [110,  77,  60,  72]])
```

### 分割数组

numpy.split(array,[index1,index2,.....]，axis)

axis默认值为0，表示垂直轴，如果值为1，表示水平的轴

```python
In [95]: nd1

Out[95]: 

array([[ 87, 126,  23,  77],

       [141, 116,  44, 140],

       [149,  15,  77,   7],

       [ 78,  15,  35,  64],

       [131,   9, 120,  26]])

In [96]: nd2, nd3, nd4 = np.split(nd1, [1, 2])

In [97]: nd2

Out[97]: array([[ 87, 126,  23,  77]])

In [98]: nd3

Out[98]: array([[141, 116,  44, 140]])

In [99]: nd4

Out[99]: 

array([[149,  15,  77,   7],

       [ 78,  15,  35,  64],

       [131,   9, 120,  26]])


In [100]: nd2, nd3, nd4 = np.split(nd1, [1, 2], axis=1)

In [101]: nd2
Out[101]: 
array([[ 87],
       [141],
       [149],
       [ 78],
       [131]])

In [102]: nd3
Out[102]: 
array([[126],
       [116],
       [ 15],
       [ 15],
       [  9]])

In [103]: nd4
Out[103]: 
array([[ 23,  77],
       [ 44, 140],
       [ 77,   7],
       [ 35,  64],
       [120,  26]])

```

### ndarray的聚合函数

求和`np.sum`

```python
In [104]: one = np.ones((5,4))

In [105]: one

Out[105]: 

array([[1., 1., 1., 1.],

       [1., 1., 1., 1.],

       [1., 1., 1., 1.],

       [1., 1., 1., 1.],

       [1., 1., 1., 1.]])

In [106]: one.sum()

Out[106]: 20.0

```

### 最大最小值平均值

` np.max/ np.min` `np.mean()`

```python
In [109]: nd1
Out[109]: 
array([[ 87, 126,  23,  77],
       [141, 116,  44, 140],
       [149,  15,  77,   7],
       [ 78,  15,  35,  64],
       [131,   9, 120,  26]])

In [110]: nd1.max()
Out[110]: 149

In [111]: nd1.min()
Out[111]: 7

In [112]: nd1.mean()
Out[112]: 74.0

```



### 

