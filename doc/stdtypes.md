# 内置类型

python 解释器中内置的标准类型。

## 真假值检测

任何对象都可以检测它的逻辑值，是否为真或为假，以便在 `if` 或 `while` 语句或其他布尔运算中使用。<br>

一个对象在默认情况下均被视为真值，除非当该对象被调用时其所属类定义了 `__bool__()` 方法且返回 False 或是定义了 `__len__()` 方法且返回零。<br>

以下是会被视为假的值：

1. 假值的常量 `None` 和 `False`
2. 数字0 `0`, `0.0`, `0j`, `Decimal(0)`, `Fraction(0, 1)`
3. 空的序列和多项集 `''`, `()`, `[]`, `{}`, `set()`, `range(0)`

## 布尔运算 and or not

|  运算符   | 含义  |  注释  |
| ------ | ------ | ------ |
| x or y | 如果x是False，返回y，否则返回x | 这是个短路运算符，只有在第一个参数为假值时才会对第二个参数求值 |
| x and y | 如果x是True，返回y，否则返回x | 这是个短路运算符，只有在第一个参数为真值时才会对第二个参数求值 |
| not x | 如果x是True，返回False，否则True | not 的优先级比非布尔运算符低，因此 not a == b 会被解读为 not (a == b) 而 a == not b 会引发语法错误。 |

上面三个布尔运算符优先级从上到下，升序。

## 比较

八种比较运算符。 它们的优先级相同。比较运算可以任意串连；例如，`x < y <= z` 等价于 `x < y and y <= z`，前者的不同之处在于 y 只被求值一次。


|  运算符  | 含义 |
| ---- | ----|
| < | 严格小于 |
| <= | 小于或等于 |
| > | 严格大于 |
| >= | 大于或等于 |
| == | 等于 |
| != | 不等于 |
| is | 对象标识 |
| is not | 否定的对象标识 |


除不同的数字类型外，不同类型的对象不能进行相等比较。

## 数字类型 int float complex

存在三种不同的数字类型：整数 int, 浮点数 float 和 复数 complex。 此外，布尔值属于整数的子类型。整数具有无限的精度。复数包含实部和虚部，分别以一个浮点数表示。 要从一个复数 z 中提取这两个部分，可使用 `z.real` 和 `z.imag`。 （标准库包含附加的数字类型，如表示有理数的 `fractions.Fraction` 以及以用户定制精度表示浮点数的 `decimal.Decimal`。）<br>

当使用不同的数值类型计算的时候，python首先将被操作的对象转换成其中最复杂的操作数的类型，然后再运算。整数比浮点数简单，浮点数比复数简单。<br>

所有数字类型（复数除外）都支持下列运算：

|  运算  | 含义 |
| ---- | ----|
| x + y | x 和 y 的和 |
| x - y | x 和 y 的差 |
| x * y | x 和 y 的乘积 |
| x % y | x / y 的余数 |
| x / y | x 和 y 的商 |
| x // y | x 和 y 向下整除的商 |
| -x | x 取反 |
| +x | x 不变 |
| abs(x) | x 的绝对值 |
| int(x) | 将 x 转换为整数 |
| float(x) | 将 x 转换为浮点数 |
| complex(re, im) | 一个带有实部 re 和虚部 im 的复数。im 默认为0。 |
| c.conjugate() | 复数 c 的共轭 |
| divmod(x, y) | (x // y, x % y) 向下整除的商和余数组成的元组 |
| pow(x, y) | x 的 y 次幂 |
| x ** y | x 的 y 次幂 |

numbers.Real 类型  int 和 float 还支持以下运算：

|  运算  | 含义 |
| ---- | ----|
| math.trunc(x) | x 截断为整数 |
| round(x[, n]) | x 舍入到 n 位小数。如果省略 n，则默认为 0。 |
| math.floor(x) | <= x 的最大 Integral |
| math.floor(x) | >= x 的最小 Integral |

### 数字按位运算

按位运算只对整数有意义。二进制按位运算的优先级全都低于数字运算，但又高于比较运算；一元运算 `~ `具有与其他一元算术运算 (+ and -) 相同的优先级。
<br>
下面是优先级升序排列

* `x | y` x 和 y 按位或（对应的二进位有一个为1时，结果就为1）
* `x ^ y` x 和 y 按位异或（对应的二进位不同时，结果为1）
* `x & y` x 和 y 按位与（如果二进位都为1,则结果为1,否则为0）
* `x << n` x 左移 n 位（高位丢弃，低位补0）
* `x >> n` x 右移 n 位
* `~x` x 逐位取反

1. 左移 n 位等价于乘以 `pow(2, n)`
2. 右移 n 位等价于除以 `pow(2, n)` ，作向下取整除法。

### 整数附加方法

* `int.bit_length()`

返回以二进制表示一个整数所需要的位数，不包括符号位和前面的零

```python
a = -10
bin(a)
'-0b1010'

a.bit_length()
4
```

它等价于这个函数：

```python
def bit_length(self):
    s = bin(self)       # binary representation:  bin(-37) --> '-0b100101'
    s = s.lstrip('-0b') # remove leading zeros and minus sign
    return len(s)       # len('100101') --> 6
```

<br><br>

* `int.to_bytes(length, byteorder, *, signed=False)`

to_bytes() 返回表示一个整数的字节数组。

```python
(1024).to_bytes(2, byteorder='big')
b'\x04\x00'
(1024).to_bytes(10, byteorder='big')
b'\x00\x00\x00\x00\x00\x00\x00\x00\x04\x00'
(-1024).to_bytes(10, byteorder='big', signed=True)
b'\xff\xff\xff\xff\xff\xff\xff\xff\xfc\x00'
x = 1000
x.to_bytes((x.bit_length() + 7) // 8, byteorder='little')
b'\xe8\x03'
```
整数会使用 `length` 个字节来表示。 如果整数不能用给定的字节数来表示则会引发 `OverflowError`。

`byteorder` 参数确定用于表示整数的字节顺序。 如果 `byteorder` 为 `big`，则最高位字节放在字节数组的开头。 如果 `byteorder` 为 `little`，则最高位字节放在字节数组的末尾。 要请求主机系统上的原生字节顺序，请使用 `sys.byteorder` 作为字节顺序值。

`signed` 参数确定是否使用二的补码来表示整数。 如果 `signed` 为 `False` 并且给出的是负整数，则会引发 OverflowError。 `signed` 的默认值为 False。

<br><br>

* classmethod  `int.from_bytes(bytes, byteorder, *, signed=False)`

from_bytes() 返回由给定字节数组所表示的整数。

```python
int.from_bytes(b'\x00\x10', byteorder='big')
16
int.from_bytes(b'\x00\x10', byteorder='little')
4096
int.from_bytes(b'\xfc\x00', byteorder='big', signed=True)
-1024
int.from_bytes(b'\xfc\x00', byteorder='big', signed=False)
64512
int.from_bytes([255, 0, 0], byteorder='big')
16711680
```

`bytes` 参数必须为一个 `bytes-like object` 或是生成字节的可迭代对象。

`byteorder` 参数确定用于表示整数的字节顺序。 如果 `byteorder` 为 `big`，则最高位字节放在字节数组的开头。 如果 `byteorder` 为 `little`，则最高位字节放在字节数组的末尾。 要请求主机系统上的原生字节顺序，请使用 `sys.byteorder` 作为字节顺序值。

`signed` 参数指明是否使用二的补码来表示整数。

* `int.as_integer_ratio()`

将整数表示为分数形式，返回一个分子和分母组成的元组。

### 浮点数附加方法

* `float.as_integer_ratio()`

返回一个分子和分母组成的元组，其比率正好等于原浮点数并且分母为正数。 无穷大会引发 OverflowError 而 NaN 则会引发 ValueError。

* `float.is_integer()`

判断一个浮点数是否可以用整数来表示。

```python
(-2.0).is_integer()
True

(3.2).is_integer()
False
```

* `float.hex()`

以十六进制字符串的形式返回一个浮点数表示。 对于有限浮点数，这种表示法将总是包含前导的 `0x` 和尾随的 `p` 加指数。

```python
1.2.hex()
'0x1.3333333333333p+0'
```

* classmethod `float.fromhex(s)`

返回以十六进制字符串 s 表示的浮点数的类方法。

```python
float.fromhex('0x3.a7p10')
3740.0
```

## 集合类型 set frozenset

`set` 对象是由具有唯一性的 `hashable` 对象所组成的无序多项集。可 `hashable` 的对象也就是简单值，不可变的对象，例如，数字，字符串，改变都需要重新生成新对象，而不能在原位置修改。<br>

与其他多项集一样，集合也支持 `x in set`, `len(set)` 和 `for x in set`。 作为一种无序的多项集，集合并不记录元素位置或插入顺序。 相应地，集合不支持索引、切片或其他序列类的操作。<br>

`set` 和 `frozenset`。 `set` 类型是可变的 --- 其内容可以使用 `add()` 和 `remove()` 这样的方法来改变。 由于是可变类型，它没有哈希值，且不能被用作字典的键或其他集合的元素。 `frozenset` 类型是不可变并且为 `hashable`， 其内容在被创建后不能再改变；因此它可以被用作字典的键或其他集合的元素。<br>

除了可以使用 `set` 构造器，非空的 `set` 还可以使用字面量直接构建： 例如: {'jack', 'sjoerd'}。<br>

`class set([iterable])`
`class frozenset([iterable])`

返回一个新的 set 或 frozenset 对象，其元素来自于 iterable。 集合的元素必须为 hashable。 要表示由集合对象构成的集合，所有的内层集合必须为 frozenset 对象。 如果未指定 iterable，则将返回一个新的空集合。<br>

集合可用多种方式来创建:

* 使用花括号内以逗号分隔元素的方式: {'jack', 'sjoerd'}
* 使用集合推导式: {c for c in 'abracadabra' if c not in 'abc'}
* 使用类型构造器: set(), set('foobar'), set(['a', 'b', 'foo'])

set 和 frozenset 的实例提供以下操作：

* `len(s)`

返回集合 s 中的元素数量

* `x in s`

检测 x 是否为 s 中的成员。

* `x not in s`

检测 x 是否非 s 中的成员。

* `s.isdisjoint(other)`

如果集合 s 中没有与 other 共有的元素则返回 True。 当且仅当两个集合的交集为空集合时，两者为不相交集合。

* `s.issubset(other)` `s <= other`

检测是否集合s中的每个元素都在 other 之中。

* `s < other`

检测s是否为 other 的真子集，即 s <= other and s != other。

* `s.issuperset(other)` `s >= other`

检测是否 other 中的每个元素都在s之中。

* `s > other`

检测s是否为 other 的真超集，即 s >= other and s != other。

* `s.union(*others)`  `s | other | ...`

求并集，返回一个新集合，其中包含来自s以及 others 指定的所有集合中的元素。

* `s.intersection(*others)` `s & other & ...`

求交集，返回一个新集合，其中包含s以及 others 指定的所有集合中共有的元素。

* `s.difference(*others)` `s - other - ...`

求差集，返回一个新集合，其中包含s在 others 指定的其他集合中不存在的元素。

* `s.symmetric_difference(other)` `s ^ other`

求对称差集，返回一个新集合，其中的元素或属于s或属于 other 指定的其他集合，但不能同时属于两者。

* `s.copy()`

返回s的浅拷贝。

⚠️注意：

1. 非运算符版本的 `union()`, `intersection()`, `difference()`，以及 `symmetric_difference()`, `issubset()` 和 `issuperset()` 方法会接受任意可迭代对象作为参数。 相比之下，它们所对应的运算符版本则要求其参数为集合。看下面例子：

```python
set('abc').union('cde')
{'d', 'b', 'a', 'c', 'e'}

set('abc') | 'cde'
TypeError: unsupported operand type(s) for |: 'set' and 'str'
```

2. `set` 的实例与 `frozenset` 的实例之间基于它们的成员进行比较。 例如 `set('abc') == frozenset('abc')` 返回 `True`，`set('abc') in set([frozenset('abc')])` 也一样。<br>

3. 混合了 `set` 实例与 `frozenset` 的运算将返回与第一个操作数相同的类型。例如: `frozenset('ab') | set('bc')`将返回 `frozenset` 的实例。

<br>

下面是可用于 `set` 而不可用于 `frozenset` 的方法：

* `update(*others)` `s |= other | ...`

更新为并集，更新集合，添加来自 others 中的所有元素。

* `intersection_update(*others)` `s &= other & ...`

更新为交集，更新集合，只保留其中在所有 others 中也存在的元素。

* `difference_update(*others)` `s -= other | ...`

更新为差集，更新集合，移除其中也存在于 others 中的元素。

* `symmetric_difference_update(other)` `s ^= other`

更新为对称差集，更新集合，只保留存在于集合的一方而非共同存在的元素。

* `s.add(elem)`

将元素 elem 添加到集合中。

* `s.remove(elem)`

从集合中移除元素 elem。 如果 elem 不存在于集合中则会引发 KeyError。

* `s.discard(elem)`

如果元素 elem 存在于集合中则将其移除。

* `s.pop()`

从集合中移除并返回任意一个元素。 如果集合为空则会引发 KeyError。

* `s.clear()`

从集合中移除所有元素。

⚠️注意：

1. 非运算符版本的 `update()`, `intersection_update()`, `difference_update()` 和 `symmetric_difference_update()` 方法将接受任意可迭代对象作为参数。

2. `__contains__()`, `remove()` 和 `discard()` 方法的 elem 参数可能是一个 set。 为支持对一个等价的 frozenset 进行搜索，会根据 elem 临时创建一个该类型对象。
