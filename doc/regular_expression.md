# 正则表达式

正则表达式（Regular Expression）是一种文本模式，用来匹配某个模式的字符串，是一种专门处理字符串的工具。

## 匹配字符

大多数情况下，普通字符只会匹配自己，例如正则表达式`'test'`将完全匹配字符串`test`。除了普通字符以外，正则表达式的语法中定义了一些元字符（metacharacters），它们不会匹配自己，而代表了特殊的含义。学习正则表达式主要就是学习使用这些元字符的功能。

下面是最常用的元字符列举：

```
 [ ] \ . ^ $ * + ? { } | ( )
```

* `[]`代表字符集合，它是你希望匹配的一组字符。可以单独列出字符，也可以使用`-`连接两个字符代表一系列字符。例如：

```python
import re
re1 = re.compile('[012]') # re1 匹配一个字符，它是0、1 或者 2
re2 = re.compile('[0-2]') # re2 和 re1 等价
re3 = re.compile('[a-zA-Z0-9]') # res 匹配所有英文字母和数字
```

⚠注意：如果将 元字符 写在字符集合中，那么元字符将失去特殊含义，变成普通的字符。

```python
import re
re1 = re.compile('[.]') # .是一个元字符，写在[]之中失去了特殊含义，只会去匹配一个普通的点

print(re1.search('1'))
# None
print(re1.search('.'))
# <re.Match object; span=(0, 1), match='.'>
```

如果在`[]`里`最前面`加上 `^` 符号，代表除了这个集合以外其他所有的字符，相当于`取反`的意思。

```python
import re
re3 = re.compile('[^0-9]') # 匹配所有非数字字符
```

注意，如果 `^` 不写在集合的首位，那么将失去`取反`的含义，就只表示匹配 `^` 这个字符。

```python
import re
re3 = re.compile('[0-9^]') # 匹配所有数字和 ^ 符号
```

* `\` 反斜杠，一些以 `\` 开头的特殊序列代表了一些预定义的字符集合。

```python
import re

re1 = re.compile('\d') # 匹配任何十进制数字；这等价于类 [0-9]
re2 = re.compile('\D') # 匹配任何非数字字符；这等价于类 [^0-9]
re3 = re.compile('\s') # 匹配任何空白字符；这等价于类 [ \t\n\r\f\v] 空格，\t横向制表符，\n换行符，\r回车符，\f换页符，\v纵向制表符
re4 = re.compile('\S') # 匹配任何非空白字符；这相当于类 [^ \t\n\r\f\v]
re5 = re.compile('\w') # 匹配任何字母与数字字符与下划线；这相当于类 [a-zA-Z0-9_]
re6 = re.compile('\W') # 匹配任何非字母与非数字字符与非下划线；这相当于类 [^a-zA-Z0-9_]
```

这些特殊的字符集合可以包含在`[]`中，例如：

```python
import re
re1 = re.compile('[\d,.]') # 匹配所有数字和逗号还有点 等价于 '[0-9,.]'
```

* `.` 点号，含义是匹配除了换行符之外的任何字符。python 中有一个 `re.DOTALL` 模式，如果开启了，则 `.` 点号将匹配任何字符，包括换行符。

### 量词

有几个元字符的含义是表示某个模式或字符需要重复多少次，这几个元字符就是量词。

* `*` 指定前一个字符匹配 `零次或多次`。 `x >= 0`
* `+` 指定前一个字符匹配 `一次或多次`。 `x >= 1`
* `?` 指定前一个字符匹配 `零次或一次`。 `x >= 0 并且 x <= 1`

```python
import re
re.compile('ca*t') # 匹配 ct 或者 cat 或者 caaaa...t
re.compile('ca+t') # 匹配 cat 或者 caaaa...t
re.compile('ca?t') # 匹配 ct 或者 cat
```

* `{m, n}` 其中 `m` 和 `n` 是十进制整数。 这个限定符意味着必须至少重复 `m` 次，最多重复 `n` 次。

```python
import re
re.compile('ca{0, 3}t') # 匹配 ct 或者 cat 或者 caat 或者 caaat
```

如果花括号 `{}` 里只写一个数字，那就代表重复固定的次数，例如 `{n}` 代表重复 `n` 次。

```python
import re
re.compile('ca{2}t') # 匹配 caat
```

⚠注意：当使用 `{m, n}` 这样的形式的时候，`m` 或者 `n` 可以省略，省略 `m` 被解释为 `0` 下限，而省略 `n` 则为无穷大的上限。因此我们就可以发现：

```python
# {0,} 与 * 相同， {1,} 相当于 + ， {0,1} 和 ? 相同
import re
re.compile('ca{0,}t')
re.compile('ca*t') # 它们俩等价

re.compile('ca{1,}t')
re.compile('ca+t') # 它们俩等价

re.compile('ca{0,1}t')
re.compile('ca?t') # 它们俩等价
```

如果是这种情况，建议直接使用 `* + ?` ，因为它们更简洁。

## 使用正则表达式

### 编译正则表达式

正则表达式被编译成`正则表达式对象`，`正则表达式对象`具有各种操作的方法。

```python
import re
re1 = re.compile('ca{2}t') # re1 变量存储的就是正则表达式 `ca{2}t` 对应的正则表达式对象
```

### 反斜杠灾难

正则表达式中使用 `\` 反斜杠代表特殊形式和 python 的字符串中的转义字符的使用互相冲突。

例如，如果你想匹配字符串 `'\'`，那么传递给re.compile的字符串必须是 `'\\'` ，因为反斜杠本身是元字符，必须在它前面再加一个反斜杠表示匹配它自身，而要将`'\\'`表示为 python 字符串，则要将反斜杠用转义字符表示，最终写成 `'\\\\'`。

简而言之，要匹配文字反斜杠，必须将 `'\\\\'` 写为正则字符串。解决方案是使用前缀为 `'r' `的字符串，这是 python 的原始字符串表示法。

```python
import re
re1 = re.compile(r'\\')
```

### 应用匹配

使用 `re.compile` 将正则表达式字符串编译成 `正则表达式对象`后，就可以调用`正则表达式对象`身上的各种方法了。也可以直接调用 `re` 模块上的各种方法。

```python
import re
re1 = re.compile('ca{2}t')

re1.match('caaat')
re1.search('caat')
re1.findall('caatcaat')
re1.finditer('caatcaat')

# 或者像下面这样调用

re.match('ca{2}t', 'caaat')
re.search('ca{2}t', 'caat')
re.findall('ca{2}t', 'caatcaat')
re.finditer('ca{2}t', 'caatcaat')
```

使用 `re.compile` 生成正则对象后再调用各种方法，和直接调用 `re` 模块上的方法还是有一些区别的。先看看直接调用 `re` 模块上的方法。

* `re.match(pattern, string, flags=0)`

如果 `string` 开始的0或者多个字符匹配到了`pattern`，就返回一个相应的 `匹配对象` 。 如果没有匹配，就返回 None ；注意它跟零长度匹配是不同的。`re.match` 方法一定会从字符串的开头开始匹配，相当于在正则起初位置强制加了 `^` 元字符。

```python
import re

re.match('ca{2}t', 'bcaat')
# 返回 None 因为 re.match 从开头开始匹配，所以字符串 'bcaat' 不符合要求，返回 None

re.match('ca{2}t', 'caat')
# <re.Match object; span=(0, 4), match='caat'>
```

* `re.search(pattern, string, flags=0)`

查找 `string` 的所有位置，返回第一个匹配 `pattern` 的`匹配对象`。<br>
⚠注意：`re.search` 和 `re.match` 的区别是，默认情况下`re.match`一定会从头开始匹配一个字符串，而 `re.search` 默认不会从头匹配。

```python
import re

re.search('ca{2}t', 'bcaat')
# <re.Match object; span=(1, 5), match='caat'>   默认情况下 re.search 不需要从开头匹配，所以只要字符串中有符合模式的情况，就会返回匹配对象

re.search('^ca{2}t', 'bcaat')
# 返回 None  因为正则加了 ^ 元字符，所以会从开头开始匹配， 这时候字符串 'bcaat' 就不符合要求了
```

* `re.findall(pattern, string, flags=0)`

对 `string` 返回一个不重复的 `pattern` 的匹配列表， `string` 从左到右进行扫描，匹配按找到的顺序返回。如果`pattern`里存在一到多个`组`，就返回一个`组合列表`。如果`pattern`里有超过一个组合的话，就返回一个`元组`的列表。

```python
import re

re.findall('a+b', 'aaab  b  aaaaab  ab')
# ['aaab', 'aaaaab', 'ab']

re.findall('(a+)b', 'aaab  b  aaaaab  ab')
# ['aaa', 'aaaaa', 'a'] 只有一个()分组，findall返回的列表的每个元素是()分组捕获到的结果

re.findall('(a+)(b)', 'aaab  b  aaaaab  ab')
# [('aaa', 'b'), ('aaaaa', 'b'), ('a', 'b')] 有多个()分组，findall返回的列表的每个元素是每次完整匹配的多个()分组匹配结果组成的元组

re.findall('((a+)b)', 'aaab  b  aaaaab  ab')
# [('aaab', 'aaa'), ('aaaaab', 'aaaaa'), ('ab', 'a')] 同上
```

* `re.finditer(pattern, string, flags=0)`

`pattern` 在 `string` 里所有的非重复匹配，返回为一个迭代器 `iterator` 保存了 `匹配对象` 。 `string` 从左到右扫描，匹配按顺序排列。空匹配也包含在结果里。

```python
import re

iterator1 = re.finditer('a+b', 'aaab  b  aaaaab  ab')
for i in iterator1:
    print(i)
'''
<re.Match object; span=(0, 4), match='aaab'>
<re.Match object; span=(9, 15), match='aaaaab'>
<re.Match object; span=(17, 19), match='ab'>
'''

iterator2 = re.finditer('(a+)b', 'aaab  b  aaaaab  ab')
for i in iterator2:
    print(i)
    print(i.group())
    print(i.group(0))
    print(i.group(1))

'''
<re.Match object; span=(0, 4), match='aaab'>
aaab
aaab
aaa
<re.Match object; span=(9, 15), match='aaaaab'>
aaaaab
aaaaab
aaaaa
<re.Match object; span=(17, 19), match='ab'>
ab
ab
a
'''
```

***

现在来看看，使用 `re.compile` 编译正则后，使用正则对象调用方法是什么样的，先看看 `re.compile`

* `re.compile(pattern, flags=0)`

`re.compile` 将正则表达式的字符串编译成一个`正则表达式对象`。通过这个`正则对象`可以调用 `match()` 或者 `search()` 等方法。

* `Pattern.search(string[, pos[, endpos]])`

这样可以看出和直接调用 `re.search` 的区别了，使用正则对象调用 `search()` 方法的时候，多了两个参数，匹配的起始位置 `pos` 和 `endpos`。
可选的第二个参数 `pos` 给出了字符串中开始搜索的位置索引；默认为 `0`。可选参数 `endpos` 限定了字符串搜索的结束；它假定字符串长度到 `endpos` ， 所以只有从 `pos` 到 `endpos - 1` 的字符会被匹配。如果 `endpos` 小于 `pos`，就不会有匹配产生。

```python
pattern = re.compile('d')
pattern.search('dog')
<re.Match object; span=(0, 1), match='d'>
pattern.search('dog', 1)  # None 从字符串索引1的位置开始匹配，所以没有匹配到索引0位置的字母d

pattern = re.compile('^d')
pattern.search('dog', 1) # None
```

* `Pattern.match(string[, pos[, endpos]])`
`pos` 和 `endpos` 参数的含义同上

* `Pattern.findall(string[, pos[, endpos]])`
`pos` 和 `endpos` 参数的含义同上

* `Pattern.finditer(string[, pos[, endpos]])`
`pos` 和 `endpos` 参数的含义同上

### 匹配对象

调用 `match()` 和 `search()`后，如果匹配成功，则会返回`匹配对象`，这个对象有什么属性和方法呢？

* `group()` 返回正则匹配的字符串
* `start()` 返回匹配的开始位置
* `end()` 返回匹配的结束位置
* `span()` 返回包含匹配 (start, end) 位置的元组

```python
import re

re1 = re.compile('a+')
result = re1.search('aaabaaacaaabaaa')
# <re.Match object; span=(0, 3), match='aaa'>

result.group() # 'aaa'
result.group(0) # 'aaa'
result.start() # 0
result.end() # 3
result.span() # (0, 3)

re1 = re.compile('a+(b)')
result = re1.search('aaabaaacaaabaaa')
# <re.Match object; span=(0, 4), match='aaab'>

result.group() # 'aaab'
result.group(0) # 'aaab'
result.group(1) # 'b'
result.start() # 0
result.end() # 4
result.span() # (0, 4)
```

### 编译标志 flags

`编译标志`会改变正则表达式的行为。`编译标志`有两种互相等价的形式，一种是完整的英文单词，例如 `re.IGNORECASE`，一种是缩写，例如 `re.I`。下面是常用的一些 `编译标志`。

* `DOTALL, S` 使 `.` 匹配任何字符，包括换行符。
* `IGNORECASE, I` 大小写不敏感。

```python
import re
re.match('[a-z]*', 'AAAA', re.I)
#<re.Match object; span=(0, 4), match='AAAA'> # 设置了 flags 为 re.I，大小写不敏感，所以也能匹配到大写字母
re.match('[a-z]*', 'AAAA')
#<re.Match object; span=(0, 0), match=''> # 不设置 flags，匹配不到大写字母了
```

* `MULTILINE, M` 多行匹配，会影响元字符 `^` 和 `$`。如果开启了 `MULTILINE` 多行模式，那么 `^` 匹配字符串开始位置以及 `/n`换行符 或 `/r`回车符 之后的位置，而 `$` 匹配字符串结束位置以及 `/n`换行符 或 `/r`回车符 之前的位置。简而言之，就是如果开启了多行，`^` 和 `$` 就会匹配每一行的开头和结尾。

* `VERBOSE, X` 启用详细的正则，可以更清晰，更容易理解。指定此标志后，将忽略正则字符串中的空格，除非空格位于字符类中或前面带有未转义的反斜杠；这使你可以更清楚地组织和缩进正则。 此标志还允许你将注释放在正则中，引擎将忽略该注释；注释标记为 '#' 既不是在字符类中，也不是在未转义的反斜杠之前。

```python
charref = re.compile(r"""
 &[#]                # 实体字符的开头 &#
 (
     0[0-7]+         # 八进制数字
   | [0-9]+          # 十进制数字
   | x[0-9a-fA-F]+   # 十六进制数字
 )
 ;                   # 结尾分号
""", re.VERBOSE)
```

### 更多元字符

这里讨论的元字符是一些`零宽度断言` (zero-width assertions)。就像它的名字一样，是一种零宽度的匹配，它匹配到的内容不会保存到匹配结果中去，最终匹配结果只是一个位置而已。

* `|` 或者的意思。`A|B` 将匹配与 `A` 或 `B` 符合的字符串。

```python
# 这是 stackoverflow 上查到的有网友提供的匹配 url 的正则表达式
import re

urlReg = re.compile('(https?|ftp|file)://[-A-Za-z0-9+&@#/%?=~_|!:,.;]+[-A-Za-z0-9+&@#/%=~_|]')

```

注意上面这个例子里，`(https?|ftp|file)` 这里就代表匹配 `http` 或 `https` 或 `ftp` 或 `file`。


* `^` 代表在字符串的开头匹配，`$` 代表在字符串的结尾匹配。python 中有一个`re.MULTILINE` 模式，如果开启了 `MULTILINE` 多行模式，那么 `^` 匹配字符串开始位置以及 `/n`换行符 或 `/r`回车符 之后的位置，简言之就是`每一行的开头`；而 `$` 匹配字符串结束位置以及 `/n`换行符 或 `/r`回车符 之前的位置，简言之就是`每一行的结尾`。

例如：

```python
import re
re1 = re.compile('^1[345789]\d{9}$') # 匹配11位手机号码 从字符串开头匹配到结尾
```

* `\A` 仅匹配字符串开始位置。没有开启 `re.MULTILINE` 多行模式时，`\A` 和 `^` 是等价的。开启多行模式后，`\A` 仍然只匹配字符串开头，而 `^` 匹配`每一行`的开头。

* `\Z` 只匹配字符串尾。在 `re.M` 多行模式下和 `\A` 的效果类似，仍然只匹配字符串结尾。

* `\b` 字边界。 仅在单词的开头或结尾处匹配。英文单词是以空格或者其他符号分隔开的，所以`\b`就是这个意思。

```python
p = re.compile(r'\bclass\b')
print(p.search('no class at all'))
# <re.Match object; span=(3, 8), match='class'>
print(p.search('the declassified algorithm'))
# None
print(p.search('one subclass is'))
# None
```
⚠注意：在 python 的普通字符串中，`\b` 是转义字符，代表退格符（Backspace）。如果想在正则中作为字边界使用，需要使用`原始字符串(raw strings)`，即上面的例子中在字符串前加字母 `r`，这样字符串将不会对反斜杠进行转义。

* `\B` 这与 `\b` 相反，仅在当前位置不在字边界时才匹配。

### 分组（捕获分组）

正则表达式可以分成子组来匹配字符串，这样匹配结果就能捕获到不同的分组内容，匹配结果对象可以通过 `group()` 方法获取不同的分组。这其实就是`捕获分组`。

```python
import re

re1 = re.compile('(\d{4})-(\d{1,2})-(\d{1,2})')
result = re1.search('今天的日期是2021-1-9')

result.group()
# '2021-1-9'
result.group(0)
# '2021-1-9'
result.group(1)
# '2021'
result.group(2)
# '1'
result.group(3)
# '9'
result.groups()
# ('2021', '1', '9')
```

从上面的例子可以看出，获取到匹配结果对象后，调用 `group()` 方法，不传递参数或者传递参数 `0`，则返回匹配到的整个结果字符串，如果依次传递 `1` `2` `3` …………，则返回的就是第 `1` `2` `3` 个分组的内容；如果调用 `groups()` ，则返回所有分组内容组成的元组。<br>

当我们学会了捕获分组之后，就可以使用`反向引用`来在正则中继续匹配之前捕获的分组。使用 `\1` 这样的形式，也就是 `\number`，反斜杠加数字，数字代表了匹配和`第几个`分组内容完全相同的字符串。

```python
import re

re1 = re.compile(r'([a-zA-Z]+)\s+\1')
result = re1.findall('i i am a coder, i like like coding and my my friend like sports !')
result
# ['i', 'like', 'my']
```

上面例子通过 `\1` 反向引用了它之前的捕获分组 `([a-zA-Z]+)`，这样就找到了一句话中连续重复的单词。<br>

⚠注意：python 在普通字符串中 `\1` 代表转义字符八进制数字 1，所以想要在正则中使用反向引用，需要使用`原始字符串(raw strings)`，即上面的例子中在字符串前加字母 `r`，这样字符串将不会对反斜杠进行转义。

### 非捕获分组 命名分组

有时候你想使用分组来表示正则的一部分，但是并不需要捕获这个分组的内容，那么就可以使用 `(?:)` 非捕获分组。

```python
import re

m = re.match('([abc]+)', 'abc')
m.groups() # ('abc',)

m = re.match('(?:[abc]+)', 'abc')
m.groups() # ()
```

上面的例子可以看到 `(?:)` 只匹配了分组，并没有捕获分组的内容。<br>

当我们使用捕获分组的时候，取出分组内容需要调用匹配对象的 `group()` 方法，参数为数字，这样获取起来很麻烦，毕竟使用数字不是很清晰明了，所以可以使用更方便的`命名分组`，也叫具名组。命名组的语法是Python特定的扩展之一: `(?P<name>)`。

```python
import re

m = re.match('(?P<first>\w+) (?P<last>\w+)', 'Jane Doe')

m.group(1) # 'Jane'
m.group('first') # 'Jane'

m.group(2) # 'Doe'
m.group('last') # 'Doe'
```

从上面的例子可以看出使用 `(?P<name>)` 命名组之后，既可以使用数字参数来获取分组内容，也可以使用自定义的组名。<br>

并且还有 `groupdict()` 方法将命名组提取为字典格式：

```python
import re

m = re.match('(?P<first>\w+) (?P<last>\w+)', 'Jane Doe')
m.groupdict()
# {'first': 'Jane', 'last': 'Doe'}
```

对于和捕获分组对应的反向引用，使用 `\1` 匹配和捕获分组内容相同的字符，而对于命名分组，也有对应的 `(?P=name)` 再次匹配对应命名分组的内容：

```python
import re

re1 = re.compile('(?P<repeat>[a-zA-Z]+)\s+(?P=repeat)')
result = re1.findall('i i am a coder, i like like coding and my my friend like sports !')
result
# ['i', 'like', 'my']
```

### 先行断言 先行否定断言 后行断言 后行否定断言

* `(?=…)` `先行断言（lookahead assertion）` 

它的意思是`?=`后面包含的正则表达式在当前位置匹配成功了，才会去继续往后匹配，否则匹配就不再进行。举个例子，如果需要匹配 x 后面一定有 y，那么使用`先行断言`就需要这么写：'x(?=y)'，匹配结果里只有x，而不会包含y。

```python
import re

re1 = re.compile('x(?=y)')

re1.search('xssssxyddd')
# <re.Match object; span=(5, 6), match='x'>

re1.search('xssssxddd')
# None
```

有了`先行断言`，我们就可以使用它来做一些预判。比如现在有一个匹配密码的需求：8-16位数字字母组合，可输入特殊字符。这个需求的意思是必须含有字母和数字，但是特殊字符可选。

```python
import re

passwordReg = re.compile('^((?=.*[a-zA-Z])(?=.*\d)|(?=.*\d)(?=.*[a-zA-Z]))[a-zA-Z\d#@!~%^&*]{8,16}$')

```

上面的正则使用了`先行断言`来预判后面的8-16位字符中必定会含有数字和字母，而其他特殊字符是可选的，这样就达到了要求。

* `(?!…)` `先行否定断言（negative lookahead assertion）`

它的意思和`先行断言`相反，如果`?!`后面的正则在当前位置匹配成功了，那么就不再继续往后匹配。举个例子，如果需要匹配 x 后面一定没有 y，那么就使用`先行否定断言`这么写：'x(?!y)'。<br>

比如有个需求是匹配所有不在百分号之前的数字：

```python
import re

re1 = re.compile('\d+(?!\d*%)')
re1.findall('There are 40 people. 20% of them are from other country. They are all between 25 to 35 years old.')
# ['40', '25', '35']

```

* `(?<=…)` `后行断言（lookbehind assertion）`

它与正常的正则匹配的顺序不一样，它会先匹配`(?<=…)`之后的内容，如果匹配成功，再回来看当前位置之前是否含有后行断言指定的内容。举个例子，如果想匹配美元符后面的数字，就这么写：'(?<=\$)\d+'

```python
import re

re1 = re.compile('(?<=\$)\d+')

re1.search('Benjamin Franklin is on the $100 bill')
# <re.Match object; span=(29, 32), match='100'>
```

* `(?<!…)` `后行否定断言（negative lookbehind assertion ）`

它和`后行断言`刚好相反，它会先匹配`(?<!…)`之后的内容，如果匹配成功，再回来看当前位置之前是否没有指定的内容。举个例子，如果想匹配除了美元符之外其他符号后面的数字，就这么写：`(?<!\$)\d+`

```python
import re

re1 = re.compile('(?<!\$)\d+')

re1.search('it’s is worth about €90')
# <re.Match object; span=(21, 23), match='90'>
```

### 分割字符串 替换字符串

* `pattern.split(string[, maxsplit=0])` 或 `re.split(pattern, string, maxsplit=0, flags=0)`

 在每一次 `pattern` 匹配处将字符串分割，结果返回分割的字符串组成的列表。如果正则中有`捕获分组`，则分组的内容也将在结果列表里。`maxsplit` 是最大分割次数。分割结束后，剩余的字符串将是列表的最后一个元素。

 ```python
import re

re.split(r'\W+', 'Words, words, words.')
# ['Words', 'words', 'words', '']

re.split(r'(\W+)', 'Words, words, words.')
# ['Words', ', ', 'words', ', ', 'words', '.', '']

re.split(r'\W+', 'Words, words, words.', 1)
# ['Words', 'words, words.']

re.split('[a-f]+', '0a3B9', flags=re.IGNORECASE)
# ['0', '3', '9']
 ```

⚠注意：如果正则匹配到了字符串的开头或结尾，那么列表的开头或结尾就会有一个空字符串。

```python
import re

re.split('\W+', '...words, Words...')
# ['', 'words', 'Words', '']
```

pattern的空匹配将分开字符串，但只在不相临的状况生效。

```python
re.split(r'\b', 'Words, words, words.')
# ['', 'Words', ', ', 'words', ', ', 'words', '.']

re.split(r'\W*', '...words...')
# ['', '', 'w', 'o', 'r', 'd', 's', '', '']

re.split(r'(\W*)', '...words...')
# ['', '...', '', '', 'w', '', 'o', '', 'r', '', 'd', '', 's', '...', '', '', '']
```

* `pattern.sub(replacement, string[, count=0])` 或 `re.sub(pattern, repl, string, count=0, flags=0)`

将`pattern`匹配到的字符串替换成`replacement`，`replacement`可以是字符串，也可以是函数。

```python
import re

re.sub('(blue|white|red)', 'colour', 'blue socks and red shoes')
# 'colour socks and colour shoes'

re.sub('(blue|white|red)', 'colour', 'blue socks and red shoes', count=1)
# 'colour socks and red shoes'
```

下面的例子 `replacement` 是一个函数：

```python
def hexrepl(match):
    "Return the hex string for a decimal number"
    value = int(match.group())
    return hex(value)

p = re.compile(r'\d+')
p.sub(hexrepl, 'Call 65490 for printing, 49152 for user code.')

# 'Call 0xffd2 for printing, 0xc000 for user code.'
```

上面的例子通过一个函数把字符串中的十进制数字替换成对应的十六进制的数字。<br>

使用 `sub()` 替换字符串的时候，`replacement` 字符串中可以使用 `\1` 或者 `\g<1>` 或者 `\g<name>` 来引用正则中的捕获分组或者命名分组。

```python
p = re.compile('section{ (?P<name> [^}]* ) }', re.VERBOSE)

p.sub(r'subsection{\1}','section{First}')
# 'subsection{First}'

p.sub(r'subsection{\g<1>}','section{First}')
# 'subsection{First}'

p.sub(r'subsection{\g<name>}','section{First}')
# 'subsection{First}'
```

仅当空匹配与前一个空匹配不相邻时，才会替换空匹配。:

```python
p = re.compile('x*')

p.sub('-', 'abxd')
# '-a-b--d-'
```

### 贪婪与非贪婪

