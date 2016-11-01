pandas含有使数据分析工作变得更快更简单的高级数据结构和操作工具。pandas基于NumPy构建，让以NumPy为中心的应用变得更加简单。

pandas引入约定：

```
from pandas import Series, DataFrame
import pandas as pd
```

# pandas的数据结构介绍

pandas的两个主要数据为：Series和DataFrame。虽然他们并不能解决所有问题，但它们为大多数应用提供了一种可靠的、易于使用的基础。

## Series

Series是一种类似于一维数组的对象，它由一组数据（各种NumPy数据类型）以及一组与之相关的数据标签（即索引）组成。仅由一组数据可产生最简单的Series：
```
obj=Series([4,7,-5,3])
```
Series的字符串表现形式为：索引在左边，值在右边。由于没有为数据指定索引，于是Series会自动创建一个0到N-1（N为数据的长度）的整数型索引。可以通过Series的values和index属性获取其数组表示形式和索引对象：
```
obj.values
obj.index
```
通常希望所创建的Series带有一个可以对各个数据点进行标记的索引：
```
obj2=Series([4,7,-5,3], index=['d', 'b', 'a', 'c'])
```
与普通NumPy数组相比，pandas可以通过索引的方式选取Series中的单个或一组值：
```
obj2['a']
```
```
obj2['d'] = 6
```
```
obj2[['c', 'a', 'd']]
```

NumPy数组运算（如根据布尔型数组进行过滤、标量乘法、应用数学函数等）都会保留索引和值之间的链接：
```
obj2

obj2[obj>0]
obj2 * 2
np.exp(obj2)
```
还可以将Series看成是一个定长的有序字典，因为它是索引值到数据值的一个映射。它可以用在许多原本需要紫电参数的函数中：
```
'b' in obj2
'e' in obj2
```
如果数据被存放在一个Python字典中，也可以直接通过这个字典来创建Series：
```
sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
obj3 = Series(sdata)
```
如果只传入一个字典，则结果Series中的索引就是原字典的键（有序排列）。
```
states = ['California', 'Ohio', 'Oregon', 'Texas']
obj4 = Series(sdata, index=states)
```
在这个例子中，sdata中跟states速印相匹配的那3个值会被找出来并放到相应的位置上，但由于"California"所对应的sdata值找不到，所以其结果就为NaN，在pandas中，它用于表示缺失或NA值。pandas中的isnull和notnull函数可用于检测缺失数据：
```
pd.isnull(obj4)
pd.notnull(obj4)
```

对于许多应用而言，Series最重要的一个功能是：它在算术运算中会自动对齐不同索引的数据。
```
obj3
obj4
obj3 + obj4
```

Series对象本身及其索引都有一个name属性，该属性跟pandas其他的关键功能关系非常密切：
```
obj4.name = 'population'
obj4.index.name = 'state'
obj4
```
Series的索引可以通过赋值的方式就地修改：
```
obj.index = ['Bob', 'Steve', 'Jeff', 'Ryan']
obj
```

## DataFrame

DataFrame是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔值）。DataFrame既有行索引也有列索引，它可以被看做由Series组成的字典（共用同一个索引）。跟其他类型的数据结构相比（如R的data.frame），DataFrame中面向行和面向列的操作基本上是平衡的。其实，DataFrame中的数据是以一个或多个二维块存放的（而不是列表、字典或别的一维数据结构）。

构建DataFrame的方法有很多，最常用的一种是直接传入一个由等长列表或NumPy数组组成的字典：
```
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9]}
frame = DataFrame(data)
```

如果制定了列序列，则DataFrame的列就会按照指定顺序进行排序：
```
DataFrame(data, columns=['year', 'state', 'pop'])
```
跟Series一样，如果传入的列在数据中找不到，就会产生NA值：
```
frame2 = DataFrame(data, columns=['year', 'state', 'pop', 'debt'],
                   index=['one', 'two', 'three', 'four', 'five'])
```
通过类似字典标记的方式或属性的方式，可以将DataFrame的列获取为一个Series：
```
frame2['state']
frame2.year
```

注意，返回的Series拥有原DataF相同的索引，且其name属性页已经被相应地设置好了。行业可以通过位置或名称的方式进行获取，比如用速印字段ix：
```
frame2.ix['three']
```
列可以通过赋值的方式进行修改，例如，可以给那个空的"debt"列赋值一个变量值或一组值：
```
frame2['debt'] = 16.5
```
```
frame2['debt'] = np.arange(5)
```

将列表或数组赋值给某个列时，其长度必须跟DataFrame的长度相匹配。如果赋值的是一个Series，就会精确匹配DataFrame的索引，所有的空位都会将被填上缺失值：
```
val = Series([-1.2, -1.5, -1.7], index=['two', 'four', 'five'])
frame['debt'] = val
```
为不存在的列赋值会创建出一个新列。关键字`del`用于删除列：
```
frame2['eastern'] = frame2.state == 'Ohio'
del frame2['eastern']
```
另外一种常见的数据形式是嵌套字典（也就是字典的字典）：



