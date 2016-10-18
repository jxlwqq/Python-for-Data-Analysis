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
```
int_array = np.arange(10)
calibers = np.array([.22, .270, .357, .380, .44, .50], dtype=np.float64)

int_array.astype(calibers.dtype)
```
还可以用简洁的类型代码来表示dtype:
```
empty_uint32 = np.empty(8, dtype='u4')

empty_unit32
```
>  **注意：** 调用astype无论如何都会创建出一个新的数组（原始数据的一份拷贝），即使新dtype跟旧dtype相同。

## 数组和标量之间的运算

ndarray不同编写循环即可对数据执行批量运算。这一过程被称为矢量化（vectorization）。大小相等的数组之间的任何算术都会将运算应用到元素级：
```
arr = np.array([[1,2,3],[4,5,6]])

arr

arr + arr
arr - arr
arr * arr
arr / arr
```

同样，数组与标量的算术运算也会将标量值传播到各个元素：
```
1 / arr

arr / 0.5
```
不同大小的数组之间的运算叫做广播（broadcasting）。

## 基本的索引和切片

NumPy数组的索引是一个内容丰富的主题，因为选取数据子集或单个元素的方式是很多。一维数组很简单。从表面上看，它们跟Python列表的功能差不多：
```
arr = np.arange(10)

arr
arr[5]
arr[5:8]
arr[5:8] = 12
arr
```
当给一个切片赋值一个标量值时，该值会自动传播（广播）到整个选区。跟列表最重要的区别在于，数组切片是原始数组的视图。这意味着数据不会被复制，视图上的任何修改都会直接反映到源数组上：
```
arr_slice = arr[5:8]
arr_slice[1] = 12345

arr

arr_slice[:] = 64

arr
```
这样设计是有原因的，由于NumPy的设计目的是处理大数据，所以假设NumPy坚持要将数据复制来复制去的话会产生何等的性能和内存问题。

> 如果想要得到ndarray切片的一份副本而非视图，就需要显式地进行复制操作：
```
arr_new = arr[5:8].copy()
```

对于高维度数组，能做的事情更多。在一个二维数组中，各索引位置上的元素不再是标量而是一维数组：
```
arr2d = np.array([[1,2,3],[4,5,6],[7,8,9]])
arr2d[2]
```
因此，可以对各个元素进行递归访问，但这样需要做的事情有点多。你可以传入一个以逗号隔开的索引列表来选取单个元素。也就是说，下面两种方式是等价的：
```
arr2d[0][2]
arr2d[0,2]
```

在多维数组中，如果省略了后面的索引，则返回对象会是一个维度低一点的ndarray。因此在2*2*3数组arr3d中：
```
arr3d = np.array([[[1,2,3],[4,5,6]],[[7,8,9],[10,11,12]]])
arr3d
arr3d[0]
arr3d[0,0,0]
```

### 切片索引

ndarray的切片语法跟Python列表这样的一维对象差不多类似：
```
arr = np.arange(10)

arr[1:6]
```
高维度对象的花样更多，可以在一个或多个轴上进行切片，也可以跟整数索引混合使用。对于二维数组arr2d，其切片方式稍显不同：
```
arr2d = np.array([[1,2,3],[4,5,6],[7,8,9]])

arr2d[:2]
```
可以看出，它是沿着第0轴（即第一个轴）切片的。也就是说，切片是沿着一个轴向选取元素的。可以一次传入多个切片，就像传入多个索引那样：
```
arr2d[:2,1:]
```
像这样进行切片时，只能得到相同维数的数组视图。通过将整数索引和切片混合，可以得到低维度的切片：
```
arr2d[1, :2]
arr2d[:2, 1]
```
注意，"只有冒号"表示选取整个轴，因此可以想下面这样对只对高维轴进行切片：
```
arr2d[:,:1]
```
对切片表达式的赋值操作也会被扩散到整个选区：
```
arr2d[:2,1:]
```
## 布尔型索引

假设有一个用于存储数据的数组以及一个存储姓名的数组（含有重复项）。在这里使用numpy.random中的randn函数生成一些正态分布的随机数据：
```
names = np.array(['Bob','Joe','Will','Bob','Will','Joe','Joe'])
data = np.random.randn(7,4)
```
假设每个名字都对应data数组的一行，而我们想要选出对应出名字"Bob"的所有行。跟算术运算一样，数组的比较运算（如==）也是矢量化的。因此，对names和字符串"Bob"的比较将会产生一个布尔型数组：
```
names == 'Bob'
```
这种布尔型数组可用于数组索引：
```
data[names=='Bob']
```
布尔型数组的长度必须跟被索引的轴长度一致。此外，还可以将布尔型数组跟切片、整数（或整数序列）混合使用：
```
data[names=='Bob',2:]
data[names=='Bob',3]
```
要选择除"Bob"以外的其他值，既可以使用不等于符号（!=），也可以通过负号（-）对条件进行否定：
```
names != 'Bob'
-(names == 'Bob')

data[-(names=='Bob')]
```
选取这三个名字中的两个需要组合应用多个布尔条件，使用&（和）、|（或）之类的布尔算术运算符即可：
```
mask = (names=='Bob') | (names=='Will')
data[mask]
```
通过布尔型索引选取数组中的额数据，将总是创建数据的副本，即使返回一模一样的数组也是如此。

通过布尔型数组设置值是一种经常用到的手段。为了将data中的所有肤质都设置为0，只需：
```
data[data<0]=0
```
通过一维布尔数组设置整行或列的值也很简单：
```
data[names!='Joe']=7
```
## 花式索引
花式索引（Fancy indexing）是一个Numpy术语，它指的是利用整数数组进行索引。假设一个8*4数组：
```
arr = np.empty((8,4))

for i in range(8):
    arr[i]=i
```
为了以特定顺序选取行子集，只需传入一个用于指定顺序的整数列表或ndarray即可：
```
arr[[4,3,0,6]]
```
>注意：arr[4,3]与arr[[4,3]]的区别

使用负数索引将会从末尾开始选取行：
```
arr[[-3,-5,-7]]
```
一次传入多个索引数组会有一点特别。它返回的是一个一维数组，其中的元素对应各个索引元组：
```
arr = np.arange(32).reshape((8,4))

arr[[3,2,1,0],[3,2,1,0]]

arr[[3,2,1,0]][:,[3,2,1,0]]
arr[np.ix_([3,2,1,0], [3,2,1,0])]
```
花式索引跟切片不一样，它总是将数据复制到新数组中。

## 数组转置和轴对换

转置（transpose）是重塑的一种特殊形式，它返回的是源数据的视图（不会进行任何复制操作）。数组不仅有transpose方法，还有一个特殊的T属性：
```
arr = np.arange(15).reshape(3,5)

arr
arr.T
```
在进行矩阵计算时，经常需要用到该操作，比如利用np.dot计算矩阵内积X^TX：
```
arr = np.random.randn(6,3)
np.dot(arr.T, arr)
```
对于高维数组，transpose需要得到一个由轴编号组成的元组才能对这些轴进行转置：
```
arr = np.arange(16).reshape((2,2,4))

arr.T

arr.transpose((1,0,2))
```
简单的转置可以使用.T，它其实就是进行轴对换而已。ndarray还有一个swapaxes方法，它需要接受一对轴编号：
```
arr.swapaxes(1,2)
```
swapaxes也是返回源数据的视图（不会进行任何复制操作）。

# 通用函数：快速的元素级数组函数

通用函数（即ufunc）是一种对ndarray重的数据执行元素级运算的函数。可以将其看做简单函数的矢量化包装器。

许多ufunc都是简单的元素级变体，如sqrt和exp：
```
arr = np.arange(10)

np.sqrt(arr)

np.exp(arr)
```
这些都是一元（unary）ufunc。另外一些（如add或maximun）接受2个数组（因此也叫二元（binary）ufunc），并返回一个结果数组：
```
x=np.random.randn(8)
y=np.random.randn(8)

np.maximum(x,y)
```
虽然并不常见，但有些ufunc的确可以返回多个数组。modf就是一个例子，它是Python内置函数divmod的矢量化版本，用于拆分浮点数数组的小数和整数部分：
```
arr = np.random.randn(7)*5

np.modf(arr)
```

一元ufunc

|函数|说明|
|:---|:---|
|abs fabs|计算整数、浮点数或复数的绝对值。对于非复数值，可以使用更快的fabs|
|sqrt|计算各元素的平方根。相当于arr**0.5|
|square|计算各元素的平方。相当于arr**2|
|exp|计算各元素的指数e^x|
|log log10 log2 log1p|分别为自然对数（底数为e）、底数为10的log、底数为2的log、log(1+x)|
|sign|计算各元素的正负号：1（整数）、0（零）、-1（负数）|
|ceil|计算各元素的ceiling值，即大于等于该值的最小整数|
|floor|计算各元素的floor值，即小于等于该值的最大整数|
|rint|将各元素值四舍五入到最接近的整数，保留dtype|
|modf|将数组的小数和整数部分以两个独立数组的形式返回|
|isnan|返回一个表示"哪些值是NaN（这不是一个数字）"的布尔型数组
|isfinite isinf|分别返回一个表示"哪些元素是有穷（非inf，非NaN）"或"哪些元素是无穷的"的布尔型数组|
|cos cosh sin sinh tan tanh|普通型和双曲型三角函数|
|arccos arccosh arcsin arcsinh arctan arctanh|反三角函数|
|logical_not|计算各元素 not x的真值。相当于-arr|

二元ufunc

|函数|说明|
|:---|:---|
|add|将数组中对应的元素相加|
|subtract|从第一个数组中减去第二个数组中的元素|
|multiply|数组元素相乘|
|divide floor_divide|除法或向下圆除法（丢弃余数）|
|power|对第一个数组中的元素A，根据第二个数组中的相应元素B，计算A^B|
|maximun fmax|元素级的最大值计算。fmax将忽略NaN|
|minimum fmin|元素级的最小值计算。fmin将忽略NaN|
|mod|元素级的求模计算（除法的余数）|
|copysign|将第二个数组中的值的符号复制给第一个数组中的值|
|greater greater_equal less less_equal equal not_equal|执行元素级的比较运算，最总产生布尔型数组。相当于中缀运算符 > >= < <= == !=|
|logical_and logical_or logical_xor|执行元素级的真值逻辑运算。相当于中缀运算符& \| ^|

# 利用数组进行数据处理

NumPy数组可以将许多种数据处理任务表述为间接的数组表达式（否则需要编写循环）。用数组表达式代替循环的做法，通常被称为矢量化。一般来说，矢量化数组运算要比等价的纯Python方式快上一两个数量级（甚至更多），尤其是各种数值计算。

假设要在一组值（网格型）上计算函数sqrt(x^2 + y^2)。np.meshgrid函数接受两个一维数组，并产生两个二维矩阵（对应于两个数组中所有的(x, y)对）：
```
points = np.arange(-5,5,0.01) # 1000个间隔相等的点

xs, ys = np.meshgrid(points, points)
```
现在，对该函数的求值运算就好办了，把这两个数组当做两个浮点数那样编写表达式即可：
```
import matplotlib.pyplot as plt
z = np.sqrt(xs**2+ys**2)
plt.imshow(z, cmap=plt.cm.gray)
plt.colorbar()
plt.title("Image plot of $\sqrt{x^2 + y^2}$ for a grid of values")
```
函数值（一个二维数组）的图形化结果是用matplotlib的imshow函数创建的。

## 将条件逻辑表述为数组运算
numpy.where函数是三元表达式 x if condition else y 的矢量化版本。假设有一个布尔数组和两个值数组：
```
xarr = np.array([1.1, 1.2, 1.3, 1.4, 1.5])
yarr = np.array([2.1, 2.2, 2.3, 2.4, 2.5])
cond = np.array([True, False, True, True, False])
```
假设要根据cond中的值选取xarr和yarr的值：当cond中的值为True时，选取xarr的值，否则从yarr中选取。列表推导式的写法应该如下所示：
```
result = [(x if c else y) for x, y, c in zip(xarr, yarr, cond)]
```
这有几个问题。第一，它对大叔组的处理速度不是很快（因为所有工作都是由纯Python完成的）。第二，无法用于多维数组。若使用np.where，则可以将该功能写的非常简洁：
```
result=np.where(cond, xarr, yarr)
```
np.where的第二个和第三个参数不必是数组，他们都可以是标量值。在数据分析工作中，where通常用于根据另个一数组而产生一个新的数组。假设有一个由随机数据组成的矩阵，要将所有正值替换为2，将所有负值替换为-2。若利用np.where，则会非常简单：
```
arr = np,random.randn(4,4)
np.where(arr>0,2,-2)
np.where(arr>0,2,arr)
```
传递给where的数组大小可以不相等，甚至可以是标量值。

np.where可以表述更为复杂的逻辑。假设有两个布尔型数组cond1和cond2，希望根据4种不同的布尔值组合实现不同的赋值操作：
```
result = []

for i in range(n):
    if cond1[i] and cond2[i]:
        result.append(0)
    elif cond1[i]:
        result.append(1)
    elif cond2[i]:
        result.append(2)
    else:
        result.append(3)
```
这个for循环可以被改写成一个嵌套的where表达式：
```
np.where(cond1 & cond2, 0, np.where(cond1, 1, np.where(cond2, 2, 3)))
```
在这个特殊的例子中，可以利用"布尔值在计算过程中可以被当做0或1处理"这个事实，素以还能将其写成下面这个算术运算：
```
result = 1 * (cond1 - cond2) + 2 * (con2 & -cond1) + 3 * （cond1 | cond2）
```

## 数学和统计方法

可以通过数组上的一组数学函数对整个数组或某个轴向的数据进行统计计算。sum、mean以及标准差std等聚合计算（aggregation，通常叫做约简（reduction））既可以当做数组的实例方法调用，也可以当做顶级NumPy函数使用：
```
arr=np.random.randn(5,4) # 正态分布的数据
arr.mean()
np.mean(arr)

arr.sum()
np.sum(arr)
```
mean和sum这类的函数可以接受一个axis参数（用于计算该轴向上的统计值），最终结果是一个少一维的数组：
```
arr.mean(axis=1)
arr.mean(1)
arr.mean(0)
```
其他如cumsum和cumprod之类的方法则不聚合，而是产生一个由中间结果组成的数组：
```
arr = np.array([[1,2,3],[4,5,6],[7,8,9]])
arr.cumsum() # 累加
arr.cumsum(0)
arr.cumsum(1)

arr.cumprod() # 累积
arr.cumprod(0)
arr.cumprod(1)
```

基本数组统计方法

|方法|说明|
|:---|:---|
|sum|对数组中全部或某轴向的元素求和。零产犊的数组的sum为0|
|mean|算术平均数。零长度的数组的mean为NaN|
|std var|分别为标准差和方差，自由度可调（默认为n）|
|min max|最小值和最大值|
|argmin argmax|分别为最大和最小元素的索引|
|cumsum|所有元素的累计和|
|cumprod|所有元素的累计积|

## 用于布尔型数组的方法
在上述方法中，布尔值会被强制转换为1（True）和0（False）。因此，sum经常被用来对布尔型数组中的True值计数：
```
arr=np.random.randn(100)
(arr>0).sum()
np.sum(arr>0)
```
另外还有两个方法any和all，它们对布尔型数组非常有用。any用于测试数组中是否存在一个或多个True，而all则检查数组中所有值是否都是True：
```
bool_arr = np.array([False, False, True, False])
bool_arr.any()
bool_arr.all()
```
这两个方法也能用于非布尔型数组，所有非0元素将会被当做True。

## 排序

跟Python内置的列表类型一样，NumPy数组也可以通过sort方法就地排序：
```
arr=np.random.randn(8)

arr.sort()
```

多维数组可以在任何一个轴向上进行排序，只需将轴边哈传给sort即可：
```
arr=np.random.rand(5,3)
arr.sort()
arr.sort(1)

arr.sort(0)
```

顶级方法np.sort返回的是数组的已排序副本，而就地排序则会修改数组本身。计算数组分位数最简单的办法是对其进行排序，然后选取特定位置的值：
```
large_arr = np.random.randn(1000)
large_arr.sort()
large_arr[int(0.05 * len(large_arr))] # 5%分位数
```

## 唯一化以及其他的集合逻辑

NumPy提供了一些针对一维ndarray的基本集合运算。最常用的可能要数np.unique了，它用于找出数组中的唯一值并返回已排序的结果：
```
names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])
np.unique(names)

ints = np.array([3,3,3,4,4,2,3,2,3,1,1,1])
np.unique(ints)
```

np.unique与等价的纯Python代码对比：
```
sorted(set(names))
```
另一个函数np.in1d用于测试一个数组中的值在另一个数组中的成员资格，返回一个布尔型数组：
```
values = np.array([6,0,0,3,2,5,6])
np.in1d(values, [2,3,6])
```
数组的集合运算

|方法|说明|
|:---|:---|
|unique(x)|计算x中的唯一元素，并返回有序结果|
|intersect1d(x,y)|计算x和y中的公共元素（交集），并返回有序结果|
|union1d(x,y)|计算x和y的并集，并返回有序结果|
|in1d(x,y)|得到一个表示"x的元素是否包含于y"的布尔型数组|
|setdiff1d|集合的差，即元素在x中且不在y中|
|setxor1d|集合的对称查，即存在于一个数组中但不同时存在在于两个数组中的元素（异或）|

# 用于数组的文件输入输出

NumPy能够读写磁盘上的文本数据或二进制数据。

## 将数组以二进制格式保存到磁盘

np.save和np.load是读写磁盘数组数据的两个主要函数。默认情况下，数组是以未压缩的原始二进制格式保存在扩展名为.npy的文件中的。
```
arr=np.arange(10)
np.save('some_array', arr)
```
如果文件路径末尾没有扩展名.npy，则该扩展名会被自动加上。然后就可以通过np.load读取磁盘上的数组：
```
np.load('some_array.npy')
```
通过np.savez可以将多个数组保存到一个压缩文件中，将数组以关键字参数的形式传入即可：
```
np.savez('some_array', a=arr, b=arr)
```
加载.npz文件时，会得到一个类似字典的对象，该对象会对各个数组进行延迟加载：
```
arch=np.load('some_array.npz')
arch['b']
```

## 存取文本文件

在文件中加载文本是一个非常标准的任务。常用的函数有np.loadtxt或者更为专门化的np.genfromtxt将数据加载到普通的NumPy数组中。

这些函数都有许多选项可供使用：指定各种分隔符、针对特定列的转换器函数、需要跳过的行数等。以一个简单的逗号分隔文件（CSV）为例：
```
np.loadtxt('test.csv', delimiter=',')
```
np.savetxt执行的是相反的操作：将数组写到以某种分隔符隔开的文件文件中。

gentfromtxt跟loadtxt差不多，只不多它面向的是结构化数组和缺失数据处理。

# 线性代数

线性代数（如矩阵乘法、矩阵分解、行列式以及其他方阵数学等）是任何数组库的重要组成部分。不像某些语言（如MATLA），通过*对两个二维数组相乘得到的是一个元素级的积，而不是一个矩阵点积。因此，NumPy提供了一个用于矩阵乘法的dot函数（既是一个数组方法也是NumPy命名空间中的一个函数）：
```
x=np.array([[1,2,3],[4,5,6]])
y=np.array([[6,23],[-1,7],[8,9]])

x.dot(y)
np.dot(x,y)
```
一个二维数组跟一个大小合适的一维数据的矩阵点积运算之后将会得到一个一维数据：
```
np.dot(x, np.ones(3))
```

numpy.linalg中有一组标准的矩阵分解运算以及诸如求逆和行列式之类的东西。他们跟MATLAB和R等语言所使用的相同的行业标准级Fortran库，如BLAS、LAPACK、Intel MKL（可能有，取决于NumPy版本）等：
```
from numpy.linalg import inv, qr
x=np.random.randn(5,5)
mat=x.T.dot(x)
int(mat)
mat.dot(inv(mat))
q,r=qr(mat)
```

常用的numpy.linalg函数

|函数|说明|
|:---|:---|
|diag|以一维数组的形式返回方阵的对角线（或非对角线）元素，或将一维数组转换为方阵（非对角线元素为0）|
|dot|矩阵乘法|
|trace|计算对角线元素的和|
|det|计算矩阵行列式|
|eig|计算方阵的本证值和本证向量|
|inv|计算方阵的逆|
|pinv|计算矩阵的Moore-Penrose伪逆|
|qr|计算QR分解|
|svd|计算奇异值分解（SVD）|
|solve|解线性方程组Ax=b，其中A为一个方阵|
|lstsq|计算Ax=b的最小二乘解|

# 随机数生成
numpy.random模块对Python内置的random进行了补充，增加了一些用于高效生成多种概率分布的样本值的函数。例如，可以用normal来得到一个准备正态分布的4*4样本数组：
```
samples=np.random.normal(size=(4,4))
samples
```
而Python内置的random模块则只能一次生成一个样本值。在生成速度上，numpy.random快了不止一个数量级。

部分numpy.random函数

|函数|说明|
|:---|:---|
|seed|确定随机数生成器的种子|
|permutation|返回一个序列的随机排列或返回一个随机排列的范围|
|shuffle|对一个序列就地随机排列|
|rand|产生均匀分布的样本值|
|randint|从给定的上下限范围内随机选取整数|
|randn|产生正态分布（平均值为0，标准差为1）的样本值，类似于MATLAB接口|
|binomial|产生二项分布的样本值|
|normal|产生正态（高斯）分布的样本值|
|beta|产生Beta分布的样本值|
|chisquare|产生卡方分布的样本值|
|gamma|产生Gamma分布的样本值|
|uniform|产生在[0,1)中均匀分布的样本值|

# 范例：随机漫步
通过模拟随机漫步来说明如何运用数组运算。一个简单的随机漫步的例子：
> 从0开始，步长1和-1出现的概率相等。
通过内置的random模块以纯Python的方式实现1000步的随机漫步：
```
import numpy as np

position=0
walk=[position]
steps=1000
for i in xrange(steps):
    step = 1 if np.random.randint(0,2) else -1
    position+=step
    walk.append(position)
```
不难看出，这其实就是随机漫步中各步的累计和，可以用一个数组运算来实现。因此，可以使用np.random模块一次性随机生成1000个"掷硬币"结果，将其分别设置为1或-1，然后计算累计和：
```
nsteps=1000
draws=np.random.randint(0,2, size=nsteps)
steps=np.where(draws>0,1,-1)
walk=steps.cumsum()
```
在这些数据的基础之上，就可以做一些统计工作了，比如求取最大值和最小值：
```
walk.min()
walk.max()
```
现在来看一个复杂点的统计任务——首次穿越时间，即速记漫步过程中第一次到达某个特定值的时间。假设需要了解随机漫步需要多久才能距离初始0点至少10步元（任一方向均可）。
