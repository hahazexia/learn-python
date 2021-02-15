# 内置函数

python 内置了一些函数可以在任何时候调用。

## 与数值和运算有关的内置函数

* `abs(x)`

返回一个数的绝对值。

```python
abs(-1)
1

abs(-4.333)
4.333

from decimal import Decimal
abs(Decimal('-0.1'))
Decimal('0.1')

from fractions import Fraction
abs(Fraction(-1,2))
Fraction(1, 2)

abs(-1 + 5j) # 复数传递给 abs() 会返回复数的模，也就是实部和虚部的平方和的平方根的绝对值
5.0990195135927845
```
<br><br>

* `hex(x)`

将整数转换成十六进制字符串，以 `0x` 开头。

```python
hex(255)
'0xff'

hex(-42)
'-0x2a'
```
<br><br>

* `oct(x)`

将整数转换成八进制字符串，以 `0o` 开头。

```python
oct(64)
'0o100'

oct(-56)
'-0o70'
```

<br><br>

* `bin(x)`

将整数转换成二进制字符串，以 `0b` 开头。

```python
bin(3)
'0b11'

bin(-10)
'-0b1010'
```
<br><br>

* class `int(x, base)`

返回一个基于整数或字符串的 `x` 的整数对象，没有参数返回0。

```python
int(10)
10

int(3.999)
3

from fractions import Fraction
int(Fraction(10, 3))
3

int()
0
```

如果 `x` 不是数字，或者有 `base` 参数，则 `x` 必须是字符串。这时 `base` 指定使用什么进制来解析 `x`。

```python
int('100', 8)
64

int('0o100', 8)
64

int('-0b1010', 2)
-10

int('0xff', 16)
255
```
<br><br>

* class `float(x)`

返回从数字或字符串 `x` 生成的浮点数。去除首位空格后，参数 `x` 必须符合以下语法：<br>

|  名称   | 内容  |
| ------ | ------ |
| sign    | "+"   "-" |
| infinity  | "Infinity" "inf" |
| nan  | "nan" |
| numeric_value  | floatnumber infinity   nan |
| numeric_string  | [sign] numeric_value |


```python
float('+1.23')
1.23
float('   -12345\n')
-12345.0
float('1e-003')
0.001
float('+1E6')
1000000.0
float('-Infinity')
-inf
```

<br><br>

* class `bool(x)`

返回一个布尔值，`True` 或 `False`。如果 `x` 是假的或者没有参数，则返回 `False`，否则返回 `True`。

在 python 中，假值有如下这些：

1. 假值的常量 `None` 和 `False`
2. 数字0 `0`, `0.0`, `0j`, `Decimal(0)`, `Fraction(0, 1)`
3. 空的序列和多项集 `''`, `()`, `[]`, `{}`, `set()`, `range(0)`

<br><br>

* class `complex(real[, imag])`

返回值为 `real + imag*1j` 的复数，或将字符串或数字转换为复数。如果第一个参数是字符串，则它被解释为一个复数，此时必须没有第二个形参。第二个形参不能是字符串。如果两个参数都省略，则返回 `0j`。

```python
complex('1+1j')
(1+1j)

complex()
0j

complex(1, 5)
(1+5j)
```
<br><br>

* class `set([iterable])` class `frozenset([iterable])`

生成新的 `set` 或 `frozenset` 对象，也就是`集合`。参数必须是可迭代对象（iterable，能够逐一返回其成员项的对象），并且最终集合的元素必须都是可哈希的（hashable，一个对象的哈希值如果在其生命周期内绝不改变，就被称为可哈希）。没有参数返回空集合。有下面的方式可以生成集合：

```python
# 字面量写法，使用花括号内以逗号分隔元素的方式
{'jack', 'sjoerd'}

# 使用集合推导
{c for c in 'abracadabra' if c not in 'abc'}

# 使用set
set(), set('foobar'), set(['a', 'b', 'foo'])
```

<br><br>

* `pow(base, exp[, mod])`

返回 `base` 的 `exp` 次幂；如果 `mod` 存在，则返回 `base` 的 `exp` 次幂对 `mod` 取余（比 `pow(base, exp) % mod` 更高效）。 两参数形式 `pow(base, exp)` 等价于乘方运算符: `base**exp`。

```python
import math

math.sqrt(144)
12.0

144 ** .5
12.0

pow(144, .5)
12.0
```