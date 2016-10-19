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