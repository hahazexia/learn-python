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

## 文本序列类型 str

字符串是由 Unicode 码位构成的不可变 序列。 字符串字面值不同的写法：

* 单引号: '允许包含有 "双" 引号'
* 双引号: "允许包含有 '单' 引号"
* 三重引号: '''三重单引号''', """三重双引号"""

### 字符串实例的方法

* `str.capitalize()`

返回原字符串的副本，其首个字符大写，其余为小写。

* `str.casefold()`

返回原字符串消除大小写的副本。消除大小写类似于转为小写，但是更加彻底一些，因为它会移除字符串中的所有大小写变化形式。 

* `str.center(width[, fillchar])`

返回长度为 width 的字符串，原字符串在其正中。 使用指定的 fillchar 填充两边的空位（默认使用 ASCII 空格符）。 如果 width 小于等于 len(s) 则返回原字符串的副本。

* `str.count(sub[, start[, end]])`

返回子字符串 sub 在 [start, end] 范围内非重叠出现的次数。 可选参数 start 与 end 会被解读为切片表示法。

* `str.encode(encoding="utf-8", errors="strict")`

返回原字符串编码为字节串对象的版本。 默认编码为 'utf-8'。 可以给出 errors 来设置不同的错误处理方案。 errors 的默认值为 'strict'，表示编码错误会引发 UnicodeError。

* `str.endswith(suffix[, start[, end]])`

如果字符串以指定的 suffix 结束返回 True，否则返回 False。 suffix 也可以为由多个供查找的后缀构成的元组。 如果有可选项 start，将从所指定位置开始检查。 如果有可选项 end，将在所指定位置停止比较。

* `str.expandtabs(tabsize=8)`

返回字符串的副本，其中所有的制表符会由一个或多个空格替换。默认 tabsize 是 8。从第 0 个字符开始计数，到第一个出现的 `\t` 制表符处为一组（如果到第 tabsize 个字符仍然没有制表符，那么这 tabsize 个字符就独自成为一组），然后不足 tabsize 个数的字符用空格来补充，直到一组字符凑齐 tabsize 个字符。

* `str.find(sub[, start[, end]])`

返回子字符串 sub 在 `s[start:end]` 切片内被找到的最小索引。 可选参数 start 与 end 会被解读为切片表示法。 如果 sub 未被找到则返回 -1。

* `str.format(*args, **kwargs)`

执行字符串格式化操作。 调用此方法的字符串可以包含字符串字面值或者以花括号 {} 括起来的替换域。 每个替换域可以包含一个位置参数的数字索引，或者一个关键字参数的名称。 返回的字符串副本中每个替换域都会被替换为对应参数的字符串值。

```python
"The sum of 1 + 2 is {0}".format(1+2)
'The sum of 1 + 2 is 3'
```

* `str.format_map(mapping)`

类似于 `str.format(**mapping)`，mapping 是映射对象，例如 字典。format_map 将字典中 key 所代表的 value 替换到 str 中的对应 key 为名字的替换域中。和 `str.format(**mapping)` 的区别在于 mapping 直接被使用，而不会被复制。

* `str.index(sub[, start[, end]])`

类似于 find()，但在找不到子类时会引发 ValueError。

* `str.isalnum()`

如果字符串中的所有字符都是字母或数字且至少有一个字符，则返回 True ， 否则返回 False 。 如果 c.isalpha() ， c.isdecimal() ， c.isdigit() ，或 c.isnumeric() 之中有一个返回 True ，则字符``c``是字母或数字。

* `str.isalpha()`

如果字符串中的所有字符都是字母，并且至少有一个字符，返回 True ，否则返回 False 。字母字符是指那些在 Unicode 字符数据库中定义为 "Letter" 的字符，即那些具有 "Lm"、"Lt"、"Lu"、"Ll" 或 "Lo" 之一的通用类别属性的字符。 注意，这与 Unicode 标准中定义的"字母"属性不同。

* `str.isascii()`

如果字符串为空或字符串中的所有字符都是 ASCII ，返回 True ，否则返回 False 。ASCII 字符的码点范围是 U+0000-U+007F 。

* `str.isdecimal()`

如果字符串中的所有字符都是十进制字符且该字符串至少有一个字符，则返回 True ， 否则返回 False 。十进制字符指那些可以用来组成10进制数字的字符，例如 U+0660 ，即阿拉伯字母数字0 。 严格地讲，十进制字符是 Unicode 通用类别 "Nd" 中的一个字符。

* `str.isdigit()`

如果字符串中的所有字符都是数字，并且至少有一个字符，返回 True ，否则返回 False 。 数字包括十进制字符和需要特殊处理的数字，如兼容性上标数字。这包括了不能用来组成 10 进制数的数字，如 Kharosthi 数。 严格地讲，数字是指属性值为 Numeric_Type=Digit 或 Numeric_Type=Decimal 的字符。

* `str.isidentifier()`

如果字符串是有效的标识符，返回 True ，依据语言定义， 标识符和关键字 节。

调用 keyword.iskeyword() 来检测字符串 s 是否为保留标识符，例如 def 和 class。

```python
>>> from keyword import iskeyword

>>> 'hello'.isidentifier(), iskeyword('hello')
True, False
>>> 'def'.isidentifier(), iskeyword('def')
True, True
```

* `str.islower()`

如果字符串中至少有一个区分大小写的字符 4 且此类字符均为小写则返回 True ，否则返回 False 。

* `str.isnumeric()`

如果字符串中至少有一个字符且所有字符均为数值字符则返回 True ，否则返回 False 。 数值字符包括数字字符，以及所有在 Unicode 中设置了数值特性属性的字符，例如 U+2155, VULGAR FRACTION ONE FIFTH。 正式的定义为：数值字符就是具有特征属性值 Numeric_Type=Digit, Numeric_Type=Decimal 或 Numeric_Type=Numeric 的字符。

* `str.isprintable()`

如果字符串中所有字符均为可打印字符或字符串为空则返回 True ，否则返回 False 。 不可打印字符是在 Unicode 字符数据库中被定义为 "Other" 或 "Separator" 的字符，例外情况是 ASCII 空格字符 (0x20) 被视作可打印字符。 （请注意在此语境下可打印字符是指当对一个字符串发起调用 repr() 时不必被转义的字符。 它们与字符串写入 sys.stdout 或 sys.stderr 时所需的处理无关。）

* `str.isspace()`

如果字符串中只有空白字符且至少有一个字符则返回 True ，否则返回 False 。

空白 字符是指在 Unicode 字符数据库 (参见 unicodedata) 中主要类别为 Zs ("Separator, space") 或所属双向类为 WS, B 或 S 的字符。

* `str.istitle()`

如果字符串中至少有一个字符且为标题字符串则返回 True ，例如大写字符之后只能带非大写字符而小写字符必须有大写字符打头。 否则返回 False 。

* `str.isupper()`

如果字符串中至少有一个区分大小写的字符且此类字符均为大写则返回 True ，否则返回 False 。

```python
'BANANA'.isupper()
True
'banana'.isupper()
False
'baNana'.isupper()
False
' '.isupper()
False
```

* `str.join(iterable)`

返回一个由 iterable 中的字符串拼接而成的字符串。 如果 iterable 中存在任何非字符串值包括 bytes 对象则会引发 TypeError。 调用该方法的字符串将作为元素之间的分隔。

* `str.ljust(width[, fillchar])`

返回长度为 width 的字符串，原字符串在其中靠左对齐。 使用指定的 fillchar 填充空位 (默认使用 ASCII 空格符)。 如果 width 小于等于 len(s) 则返回原字符串的副本。

* `str.lower()`

返回原字符串的副本，其所有区分大小写的字符均转换为小写。

* `str.lstrip([chars])`

返回原字符串的副本，移除其中的前导字符。 chars 参数为指定要移除字符的字符串。 如果省略或为 None，则 chars 参数默认移除空白符。 实际上 chars 参数并非指定单个前缀；而是会移除参数值的所有组合:

```python
'   spacious   '.lstrip()
'spacious   '

'www.example.com'.lstrip('cmowz.')
'example.com'
```

参见 str.removeprefix() ，该方法将删除单个前缀字符串，而不是全部给定集合中的字符。 例如:

```python
'Arthur: three!'.lstrip('Arthur: ')
'ee!'

'Arthur: three!'.removeprefix('Arthur: ')
'three!'
```

* `static str.maketrans(x[, y[, z]])`

此静态方法返回一个可供 str.translate() 使用的转换对照表。

如果只有一个参数，则它必须是一个将 Unicode 码位序号（整数）或字符（长度为 1 的字符串）映射到 Unicode 码位序号、（任意长度的）字符串或 None 的字典。 字符键将会被转换为码位序号。

如果有两个参数，则它们必须是两个长度相等的字符串，并且在结果字典中，x 中每个字符将被映射到 y 中相同位置的字符。 如果有第三个参数，它必须是一个字符串，其中的字符将在结果中被映射到 None。

* `str.partition(sep)`

在 sep 首次出现的位置拆分字符串，返回一个元组，其中包含分隔符之前的部分、分隔符本身，以及分隔符之后的部分。 如果分隔符未找到，则返回的元组中包含字符本身以及两个空字符串。

* `str.removeprefix(prefix, /)`

如果字符串以 prefix 字符串开头，返回 string[len(prefix):] 。否则，返回原始字符串的副本：

```python
'TestHook'.removeprefix('Test')
'Hook'

'BaseTestCase'.removeprefix('Test')
'BaseTestCase'
```

* `str.removesuffix(suffix, /)`

如果字符串以 suffix 字符串结尾，并且 suffix 非空，返回 string[:-len(suffix)] 。否则，返回原始字符串的副本：

```python
'MiscTests'.removesuffix('Tests')
'Misc'

'TmpDirMixin'.removesuffix('Tests')
'TmpDirMixin'
```

* `str.replace(old, new[, count])`

返回字符串的副本，其中出现的所有子字符串 old 都将被替换为 new。 如果给出了可选参数 count，则只替换前 count 次出现。

* `str.rfind(sub[, start[, end]])`

返回子字符串 sub 在字符串内被找到的最大（最右）索引，这样 sub 将包含在 s[start:end] 当中。 可选参数 start 与 end 会被解读为切片表示法。 如果未找到则返回 -1。

* `str.rindex(sub[, start[, end]])`

类似于 rfind()，但在子字符串 sub 未找到时会引发 ValueError。

* `str.rjust(width[, fillchar])`

返回长度为 width 的字符串，原字符串在其中靠右对齐。 使用指定的 fillchar 填充空位 (默认使用 ASCII 空格符)。 如果 width 小于等于 len(s) 则返回原字符串的副本。

* `str.rpartition(sep)`

在 sep 最后一次出现的位置拆分字符串，返回一个 3 元组，其中包含分隔符之前的部分、分隔符本身，以及分隔符之后的部分。 如果分隔符未找到，则返回的 3 元组中包含两个空字符串以及字符串本身。

* `str.rsplit(sep=None, maxsplit=-1)`

返回一个由字符串内单词组成的列表，使用 sep 作为分隔字符串。 如果给出了 maxsplit，则最多进行 maxsplit 次拆分，从 最右边 开始。 如果 sep 未指定或为 None，任何空白字符串都会被作为分隔符。 除了从右边开始拆分，rsplit() 的其他行为都类似于下文所述的 split()。

* `str.rstrip([chars])`

返回原字符串的副本，移除其中的末尾字符。 chars 参数为指定要移除字符的字符串。 如果省略或为 None，则 chars 参数默认移除空白符。 实际上 chars 参数并非指定单个后缀；而是会移除参数值的所有组合:

```python
'   spacious   '.rstrip()
'   spacious'

'mississippi'.rstrip('ipz')
'mississ'
```

要删除单个后缀字符串，而不是全部给定集合中的字符，请参见 str.removesuffix() 方法。 例如:

```python
'Monty Python'.rstrip(' Python')
'M'

'Monty Python'.removesuffix(' Python')
'Monty'
```

* `str.split(sep=None, maxsplit=-1)`

返回一个由字符串内单词组成的列表，使用 sep 作为分隔字符串。 如果给出了 maxsplit，则最多进行 maxsplit 次拆分（因此，列表最多会有 maxsplit+1 个元素）。 如果 maxsplit 未指定或为 -1，则不限制拆分次数（进行所有可能的拆分）。

如果给出了 sep，则连续的分隔符不会被组合在一起而是被视为分隔空字符串 (例如 '1,,2'.split(',') 将返回 ['1', '', '2'])。 sep 参数可能由多个字符组成 (例如 '1<>2<>3'.split('<>') 将返回 ['1', '2', '3'])。 使用指定的分隔符拆分空字符串将返回 ['']。

```python
'1,2,3'.split(',')
['1', '2', '3']

'1,2,3'.split(',', maxsplit=1)
['1', '2,3']

'1,2,,3,'.split(',')
['1', '2', '', '3', '']
```

如果 sep 未指定或为 None，则会应用另一种拆分算法：连续的空格会被视为单个分隔符，其结果将不包含开头或末尾的空字符串，如果字符串包含前缀或后缀空格的话。 因此，使用 None 拆分空字符串或仅包含空格的字符串将返回 []。

```python
'1 2 3'.split()
['1', '2', '3']

'1 2 3'.split(maxsplit=1)
['1', '2 3']

'   1   2   3   '.split()
['1', '2', '3']
```

* `str.splitlines([keepends])`

返回由原字符串中各行组成的列表，在行边界的位置拆分。 结果列表中不包含行边界，除非给出了 keepends 且为真值。

此方法会以下列行边界进行拆分。 特别地，行边界是 universal newlines 的一个超集。

|  行边界表示符  | 描述  |
| ------ | ------ |
|  \n  | 换行 |
| \r | 回车 |
| \r\n | 回车 + 换行 |
| \v 或 \x0b  | 行制表符 |
| \f 或 \x0c  | 换表单 |
| \x1c  | 文件分隔符 |
| \x1d  | 组分隔符 |
| \x1e  | 记录分隔符 |
| \x85  | 下一行 (C1 控制码) |
| \u2028  | 行分隔符 |
| \u2029  | 段分隔符 |


例如:

```python
'ab c\n\nde fg\rkl\r\n'.splitlines()
['ab c', '', 'de fg', 'kl']

'ab c\n\nde fg\rkl\r\n'.splitlines(keepends=True)
['ab c\n', '\n', 'de fg\r', 'kl\r\n']
```

不同于 split()，当给出了分隔字符串 sep 时，对于空字符串此方法将返回一个空列表，而末尾的换行不会令结果中增加额外的行:

```python
"".splitlines()
[]

"One line\n".splitlines()
['One line']
```

作为比较，split('\n') 的结果为:

```python
''.split('\n')
['']

'Two lines\n'.split('\n')
['Two lines', '']
```

* `str.startswith(prefix[, start[, end]])`

如果字符串以指定的 prefix 开始则返回 True，否则返回 False。 prefix 也可以为由多个供查找的前缀构成的元组。 如果有可选项 start，将从所指定位置开始检查。 如果有可选项 end，将在所指定位置停止比较。

* `str.strip([chars])`

返回原字符串的副本，移除其中的前导和末尾字符。 chars 参数为指定要移除字符的字符串。 如果省略或为 None，则 chars 参数默认移除空白符。 实际上 chars 参数并非指定单个前缀或后缀；而是会移除参数值的所有组合:

```python
'   spacious   '.strip()
'spacious'

'www.example.com'.strip('cmowz.')
'example'
```

最外侧的前导和末尾 chars 参数值将从字符串中移除。 开头端的字符的移除将在遇到一个未包含于 chars 所指定字符集的字符时停止。 类似的操作也将在结尾端发生。 例如:

```python
comment_string = '#....... Section 3.2.1 Issue #32 .......'

comment_string.strip('.#! ')
'Section 3.2.1 Issue #32'
```

* `str.swapcase()`

返回原字符串的副本，其中大写字符转换为小写，反之亦然。 请注意 s.swapcase().swapcase() == s 并不一定为真值。

* `str.title()`

返回原字符串的标题版本，其中每个单词第一个字母为大写，其余字母为小写。

```python
'Hello world'.title()
'Hello World'
```

该算法使用一种简单的与语言无关的定义，将连续的字母组合视为单词。 该定义在多数情况下都很有效，但它也意味着代表缩写形式与所有格的撇号也会成为单词边界，这可能导致不希望的结果:

```python
"they're bill's friends from the UK".title()
"They'Re Bill'S Friends From The Uk"
```

可以使用正则表达式来构建针对撇号的特别处理:

```python
import re
def titlecase(s):
    return re.sub(r"[A-Za-z]+('[A-Za-z]+)?",
                   lambda mo: mo.group(0).capitalize(),
                   s)

titlecase("they're bill's friends.")
"They're Bill's Friends."
```

* `str.translate(table)`

返回原字符串的副本，其中每个字符按给定的转换表进行映射。 转换表必须是一个使用 __getitem__() 来实现索引操作的对象，通常为 mapping 或 sequence。 当以 Unicode 码位序号（整数）为索引时，转换表对象可以做以下任何一种操作：返回 Unicode 序号或字符串，将字符映射为一个或多个字符；返回 None，将字符从结果字符串中删除；或引发 LookupError 异常，将字符映射为其自身。

你可以使用 str.maketrans() 基于不同格式的字符到字符映射来创建一个转换映射表。

另请参阅 codecs 模块以了解定制字符映射的更灵活方式。

* `str.upper()`

返回原字符串的副本，其中所有区分大小写的字符均转换为大写。 请注意如果 s 包含不区分大小写的字符或者如果结果字符的 Unicode 类别不是 "Lu" (Letter, uppercase) 而是 "Lt" (Letter, titlecase) 则 s.upper().isupper() 有可能为 False。

* `str.zfill(width)`

返回原字符串的副本，在左边填充 ASCII '0' 数码使其长度变为 width。 正负值前缀 ('+'/'-') 的处理方式是在正负符号 之后 填充而非在之前。 如果 width 小于等于 len(s) 则返回原字符串的副本。

例如:

```python
"42".zfill(5)
'00042'

"-42".zfill(5)
'-0042'
```