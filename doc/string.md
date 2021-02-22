# 字符串

## 字符串基础

python 中的字符串是不可变`序列`，字符串所包含的字符存在从左到右的顺序，并且它们不可以在原位置修改。`序列`还包括列表和元组，因此，序列的操作对它们也有效。<br>

下面是常见的字符串字面量和操作：

```python
s = '' # 空字符串
s = "spam's" # 双引号和单引号相同
s = 's\np\ta\x00m' # 转义字符
s = '''...multiline...''' # 三引号字符串
s = r'\temp\spam' # 原始字符串（不转义）
b = b'sp\xc4m' # python 2.6 2.7 和 3.x 中的字节串
u = u'sp\u00c4m' # python 2.x 和 3.3+ 中的 unicode 字符串
s1 + s2 # 拼接
s * 3 #重复
s[i] #索引
s[i: j] # 分片
len(s) #长度
'a %s parrot' % kind # 字符串格式化表达式
'a {0} parrot'.format(kind) # python 2.6 2.7 和 3.x 中字符串格式化方法
s.find('pa') # 字符串方法 搜索
s.rstrip() # 移除右侧空白
s.replace('pa', 'xx') # 替换
s.split(',') # 用分隔符分组
s.isdigit() # 检查字符串是否只由数字组成
s.lower() # 所有字符转换成小写
s.endswith('spam') # 是否以指定字符串结尾
'spam'.join(strlist) # 生成指定字符为分隔符连接序列元素后的新字符串
s.encode('latin-1') # unicode 编码
s.decode('utf8') # unicode 解码
for x in s: print(x) # 迭代
'spam' in s # 成员关系
map(ord, s) # ord 返回单个字符的 ASCii 序号
re.match('sp(.*)am', line) # re模块 正则表达式
```

## 字符串字面量

* 单引号和双引号是一样的。空格隔开的字符串字面量会被自动拼接起来，和用 `+` 连接是一样的效果。
* 反斜杠用于引入特殊的字符，这叫转义序列。下面是转义序列表：

```python
'\<newline>'  # 忽略换行
'\\' # 反斜杠
'\'' # 单引号
'\"' # 双引号
'\a' # 响铃
'\b' # 退格
'\f' # 换页
'\n' # 换行
'\r' # 回车
'\t' #水平制表符
'\v' # 垂直制表符
'\xhh' # 十六进制 hh 的字符 必须为2个十六进制数字
'\ooo' # 八进制 ooo 的字符 最多3个八进制数字
'\N{id}' # unicode字符
'\uxxxx' # 16 位十六进制数 xxxx 码位的字符 必须为 4 个十六进制数码
'\Uxxxxxxxx' # 32 位 16 进制数 xxxxxxxx 码位的字符 任意 Unicode 字符。必须为 8 个十六进制数码。
```

* 原始字符串会阻止转义。但是 r'xxx\' 不是一个有效的字符串常量，因为不能以反斜杠结尾，仍然需要将结尾的反斜杠转义。

* 三引号是多行字符串，换行处会嵌入换行符 `\n`。常用来作为注释使用。

## 实际应用中的字符串

* 使用 `+` 拼接字符串，使用 `*` 重复字符串。`len()`获取字符串长度。`for in` 循环字符串获取每个字符。

```python
'abc' + 'def'
'abcdef'

'ha' * 3
'hahaha'

len('123')
3

for a in 'zzz': print(a)
z
z
z
```

* 字符串是字符的有序集合，所以可以通过索引来获取指定位置的字符。

```python
'abc'[0]
'a'
```

偏移量是从0开始的，以字符串长度小1的偏移量结束。还支持负偏移量，一个负偏移量和字符串的长度相加会得到正偏移量。可以把负偏移量看成是从结尾处反向计数。

```python
s = 'spam'
s[-2]
'a'
```

分片将返回新的字符串。`s[i:j:k]`，其中`i`是起始位置，`j`是结束位置。最终结果`包含i`但`不包含j`。`i`和`j`的默认值是0和分片对象的长度。`k`是步长，步长会加到索引值上，也就是每隔`k`个元素索引一次。`k`默认是1。当`k`为负数时，表示分片将从右至左进行，实际效果是将字符串反转。

```python
s = 'abcdefghijklmn'
s[0:2]
'ab'

s[::2]
'acegikm'

s[::-1]
'nmlkjihgfedcba'

s[5:1:-1]
'fedc'
```

* python中字符串和数字相加会报错，这样的操作是不允许的。这时就需要将字符串转换成数字，或者把数字转换成字符串。

```python
'42' + 1
TypeError: can only concatenate str (not "int") to str

int('42'), str(42)
(42, '42')
```

* `ord()` 内置函数可以获取单个字符的 unicode 码点，`chr()`执行相反操作，返回整数 unicode 码点对应的字符的字符串。

```python
ord('好')
22909

chr(22909)
'好'
```

下面是一个二进制转换十进制的例子：

```python
B = '1101'
I = 0
while B != '':
    I = I * 2 + (ord(B[0]) - ord('0'))
    B = B[1:]

I
13
```

这里`(ord(B[0]) - ord('0'))` 只是判断当前位是 0 还是 1，然后加上当前位，每次的结果乘以 2 表示二进制高位往低位退一位，直到没有位数可退为止。

* 字符串不能通过索引直接修改指定位置的字符，会报错。如果想修改字符串，需要调用字符串方法，或者使用分片去生成新的字符串。

## 字符串方法

字符串方法很多，这里只介绍一些最常用的方法，其他方法请去查阅标准库手册。<br>

* 如果想要替换字符串中的子串，可以使用 replace 方法。

```python
s = 'spammy'
s = s.replace('mm', 'xx')
s
'spaxxy'


'aa$bb$cc$dd'.replace('$', 'SPAM')
'aaSPAMbbSPAMccSPAMdd'

'aa$bb$cc$dd'.replace('$', 'SPAM', 1)
'aaSPAMbb$cc$dd'
```

* 如果需要替换可能在任意位置出现的子串，就先用 find 找到子串的位置，然后使用分片：

```python
s = 'xxxxSPAMxxxxSPAMxxxx'
where = s.find('SPAM')
where
4

s = s[:where] + 'EGGS' + s[(where+4):]
```

* 如果要对一个长字符串进行多处修改，可以将字符串转换成一个列表，修改结束后再调用 join 方法连接成字符串。

```python
s = 'spammy'
l = list(s)
l
['s', 'p', 'a', 'm', 'm', 'y']

l[3] = 'x'
l[4] = 'x'
l
['s', 'p', 'a', 'x', 'x', 'y']

s = ''.join(l)
s
'spaxxy'
```

* split 方法将一个字符串从分隔符处分割成一系列子串，生成一个列表。

```python
line = 'aaa bbb ccc'
cols = line.split()
cols
['aaa', 'bbb', 'ccc']

line = 'bob,hacker,40'
line.split(',')
['bob', 'hacker', '40']
```

* rstrip 清除末尾的空白，upper 所有字符转换成大写，isalpha 判断字符串的字符是否都是字母，endswith 是否以指定的子串结束，startswith 是否以给定的子串开始。

```python
line = 'The knights who say Ni!\n'
line.rstrip()
'The knights who say Ni!'

line.upper()
'THE KNIGHTS WHO SAY NI!\n'

line.isalpha()
False

line.endswith('Ni!\n')
True

line.startswith('The')
True
```

### 原始 string 模块

python 最初的时候只提供一个标准库模块，名为 string。其中大部分函数相当于目前的字符串对象方法集。以为有很多人写了如此多的依赖原始 string 模块的代码，所以 string 模块被保留下来，以便向后兼容。<br>

如今，你应该只使用字符串对象方法，而不是最初的 string 模块。如今字符串方法的原始模块调用形式已经从 python 3.x 中完全删除了。<br>

因此，在代码中你会看到以下这两种不同的调用模式：

```python
# python 3.x

s = 'a+b+c+'
x = s.replace('+', 'spam')
x
'aspambspamcspam'

# python 2.x
import string
s = 'a+b+c+'
y = string.replace(s, '+', 'spam')
y
'aspambspamcspam'
```

## 字符串格式化表达式

字符串格式化有两种形式：

* `'...%s...' % (values)`

这种方式类似于 c 语言的 `printf` 模型

* `'...{}...'.format(values)`

这是 python 2.6 3.0 新增的方法，起源于 c# 中同名的工具。

<br>

格式化表达式需要使用`%`运算符，这也被称为字符串的`格式化`或`插值`运算符。写成 `format % values` 的形式，`format`是一个字符串，它包含多个`%`和字母组合的转换标记符，这些标记转换符会被替换为 `values` 中的值。这种语法类似c语言的 printf 语句。<br>

如果 `format` 中只有一个转换目标，则 `values` 为一个对象，如果有多个转换目标，`values` 为一个元组，其中的值和转换目标一一对应。

```python
'That is %d %s bird!' % (1, 'dead')
'That is 1 dead bird!'

'%d %s %g' % (1, 'spam', 4.0)
'1 spam 4'

'%s -- %s -- %s' % (42, 3.14159, [1,2,3])
'42 -- 3.14159 -- [1, 2, 3]'

```

下表是字符串格式化的类型码，也就是跟在 `%` 后面的符号

* s 字符串(或任何对象的str(x)字符串)
* r 与s相同，但使用 repr()，而不是 str()
* c 单个字符串，接受整数或其他字符
* d 有符号十进制整数
* i 有符号十进制整数。
* u 与 d 相同，已经废弃
* o 八进制整数
* x 十六进制整数（小写）
* X 十六进制整数（大写）
* e 浮点数指数形式（小写）
* E 浮点数指数形式（大写）
* f 十进制浮点数
* F 十进制浮点数
* g 如果指数小于 -4 或不小于精度（默认总位数精度为6）则使用 %e，否则使用%f
* G 如果指数小于 -4 或不小于精度（默认总位数精度为6）则使用 %E，否则使用%F
* % %字面量（不转换参数，输出一个'%'）

格式化字符串，表达式左侧支持多种转换操作，语法结构如下：<br>

`%[(keyname)][flags][width][.precision]typecode`

<br>

上面列表的类型码出现在这个语法中的 `typecode` 处，你可以在 % 和 类型码之间进行如下操作：

* 为索引在表达式右侧的字典结构提供键名
* 罗列说明格式的标签，如左对齐（-），数值符号（+），正数前的空白以及负数前的空格和用零填充。
* 为被替换的文本给出总的最小字段宽度
* 为浮点数设置小数点后显示的位数（精度）

下面是一些例子：

```python
x = 1234
res = 'integers: ...%d...%-6d...%06d' % (x, x, x)
res
'integers: ...1234...1234  ...001234'
# %-6d 左对齐 宽度为6   %06d 零填充 宽度为6

x = 1.23456789

'%e | %f | %g' % (x,x,x)
'1.234568e+00 | 1.234568 | 1.23457'

'%E' % x
'1.234568E+00'

'%6.2f | %05.2f | %+06.1f' % (x,x,x)
'  1.23 | 01.23 | +001.2'

'%s' % x, str(x)
('1.23456789', '1.23456789')

'%f, %.2f, %.*f' % (1/3.0, 1/3.0, 4, 1/3.0)
'0.333333, 0.33, 0.3333'
# 格式化字符串中的 * 指定计算得出宽度和精度，它会从%右边的的项中获取，这里元组中的4指定了精度
```

字符串格式化允许左边的转换目标引用右边的字典中的键来提取对应的值。

```python
'%(qty)d more %(food)s' % {'qty': 1, 'food': 'spam'}
'1 more spam'

reply = '''
... Greetings...
... Hello %(name)s!
... Your age is %(age)s
... '''

values = {'name': 'Bob', 'age': 40}
print(reply % values)

Greetings...
Hello Bob!
Your age is 40
```

## 字符串格式化方法 format

