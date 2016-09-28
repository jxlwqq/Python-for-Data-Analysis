NumPy(Numerical Python的简称)是高性能科学计算和数据分析的基础包。它是所有高级工具的构建基础。

其部分功能如下：

- ndarray，一个具有矢量算术运算和复杂广播能力的快速且节省空间的多维数组。
- 用于对整组数据进行快速运算的标准数学函数(无需编写循)。
- 用于读写磁盘数据的工具以及用于操作内存映射文件的工具。
- 线性代数、随机数生成记忆傅里叶变换功能。
- 用于集成由C、C++、Fortran 等语言编写的代码的工具。

NumPy本身并没有提供多么高级的数据分析功能，理解NumPy数组以及面向数组的设计将有助于你更加高效地使用诸如pandas之类的工具。

对于大部分数据分析应用而言，其主要功能集中在：

- 用于数据整理和清理、子集构造和过滤、转换等快速的矢量化数组运算。
- 常用的数组算法，如排序、唯一化、集合运算等。
- 高效的描述统计和数据聚合/摘要运算。
- 用于异构数据集的合并/连接运算的数据对齐和关系型数据运算。
- 将条件逻辑表述为数组表达式（而不是带有if-elif-else分支的循环）。
- 数据的分组运算(聚合、转换、函数应用等)。

# NumPy的ndarray：一种多维数组对象

Num最重要的一个特点就是其N维数组对象（即ndarray），该对象是一个快速而灵活的大数据集容器。利用这种数组对整个数据执行一些数学运算，其语法跟标量元素之间的运算是一样的。

ndarray是一个通用的同构数据多维容器，其中的所有元素必须是相同类型的。每个数组都有一个shape(一个表示各位赌大小的元组)和一个dtype(一个用于说明数组数据类型的对象):

## 创建ndarray

创建数据最简单的办法是使用array函数。它接受一切序列型的对象(包括其他数组),然后产生一个新的含有传入数据的NumPy数组:

```
import numpy as np
data = [6, 7.5, 8, 0, 1]
arr = np.array(data)

arr
arr.ndim
arr.shape
arr.dtype
```

嵌套序列(比如由一组等长列表组成的列表)将会被转换为一个多维数组:

```
data = [[1, 2, 3, 4], [5, 6, 7, 8]]
arr = np.array(data)

arr
arr.ndim
arr.shape
arr.dtype
```

除非显式说明,np.array会尝试为新建的这个数组推断出一个较为合适的数据类型。数据类型保存在一个特殊的dtype对象中。

除了np.array之外,还有一些函数也可以新建数组。比如,zeros和ones分别可以创建指定长度或形状的全0或全1数组。empty可以创建一个没有任何具体值的数组。要用这些方法创建多维数组,只需传入一个表示形状的元组即可
```
np.zeros(5)
np.zeros((2,4))
np.ones((2,4,2))
np.empty((2,3))
```

arange是Python内置函数range的数组版:
```
np.arange(15)
```
表1列出了一些数组创建函数。由于NumPy关注的是数值计算,因此,如果没有特别指定,数据类型基本都是float64(浮点型)。

表1:数组创建函数

|函数|说明|
|:---|:---|
|array|将输入数据(列表、元组、数组或其他序列类型)转换成ndarray。要么推断出dtype,要么显式指定dtype。默认直接复制输入数据|
|asarray|将输入转换成ndarray,如果出入本身就是一个ndarray就不进行复制|
|arange|类似于Python内置的range,但返回的是一个ndarray而不是列表|
|ones/ones_like|根据指定的形状和dtype创建一个全1数组。ones_like以另一个数组为参数,根据其形状和dtype创建一个全1数组|
|zeros/zeros_like|类似于ones和ones_like,产生的是全0数组|
|empty/empty_like|创建新数组,只分配内存空间但不填充任何值|
|eye/identity|创建一个正方的N*N单位矩阵(对角线为1,其余为0)|


## ndarray的数据类型

dtype(数据类型)是一个特殊的对象,它含有ndarray将一个内存解释为特定数据类型所需的信息:
```
arr_float = np.array([1,2,3], dtype=np.float64)
arr_int = np.array([1,2,3], dtype=np.int64)

arr_float
arr_int
```

dtype是NumPy如此强大和灵活的原因之一。多数情况下,他们直接映射到相应的机器表示,这使得"读写磁盘上的二进制数据流"以及"集成低级语言代码"等工作变得更加简单。数值型dtype的命名方式相同:一个类型名(如float或int),后面跟一个用于与表示各元素位长的数字。标准的双精度浮点数(即Python中的float对象)需要占用8字节(即64位)。因此,该类型在NumPy中就记作float64。表2列出了NumPy所支持的全部数据类型。

表2:NumPy的数据类型

|类型|类型代码|说明|
|:---|:---|:---|
|int8 uint8|i1 u1|有符号和无符号的8位(1个字节)整型|
|int16 uint16|i2 u2|有符号和无符号的16位(2个字节)整型|
|int32 uint32|i4 u4|有符号和无符号的32位(4个字节)整型|
|int64 uint64|i8 u8|有符号和无符号的64位(8个字节)整型|
|float16|f2|半经度浮点数|
|float32|f4 f|标准的单精度浮点数。与C的float兼容|
|float64|f8 d|标准的双精度浮点数。与C的double和Python的float对象兼容|
|float128|f16 g|扩展精度浮点数|
|complex64 complex128|c8 c16|分别用两个32位、64位或128位浮点数表示的复数|
|complex256|c32|复数|
|bool| |存储True和False值的布尔类型|
|object|O|Python对象类型|
|string_|S|固定长度的字符串类型(每个字符1个字节)|
|unicode_|U|固定长度的unicode类型(字节数由平台决定)|

你可以通过ndarray的astype方法显式地转换其dtype:
```
arr = np.array([1, 2, 3, 4, 5])

arr.dtype

float_arr = arr.astype(np.float64)
float_arr.dtype
```

如果将浮点数转换成整数,则小数部分将会被截断:
```
arr = np.array([3.7, -1.2, -2.6, 0.5, 12.9, 10.1])

arr

arr.astype(np.int32)
```

如果某字符串数组表示的全是数字,也可以用astype将其转换为数值形式:
```
numeric_strings = np.array(['1.25', '-9.6', '42'], dtype=np.string_)

numeric_strings.astype(float)
```

如果转换过程因为某种原因而失败了(比如某个不能被转换为float64的字符串),就会抛出一个TypeError。上面astype()的参数参数是float而不是np.float64,NumPy会将Python类型映射到等价的dtype上。

数组的dtype还有另外一个用法:




