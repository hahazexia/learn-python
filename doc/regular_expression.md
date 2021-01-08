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
re5 = re.compile('\w') # 匹配任何字母与数字字符；这相当于类 [a-zA-Z0-9_]
re6 = re.compile('\W') # 匹配任何非字母与数字字符；这相当于类 [^a-zA-Z0-9_]
```

这些特殊的字符集合可以包含在`[]`中，例如：

```python
import re
re1 = re.compile('[\d,.]') # 匹配所有数字和逗号还有点 等价于 '[0-9,.]'
```

* `.` 点号，含义是匹配除了换行符之外的任何字符。python 中有一个 `re.DOTALL` 模式，如果开启了，则 `.` 点号将匹配任何字符，包括换行符。

* `^` 代表在字符串的开头匹配，`$` 代表在字符串的结尾匹配。python 中有一个`re.MULTILINE` 模式，如果开启了 `MULTILINE` 多行模式，那么 `^` 匹配字符串开始位置以及 `/n`换行符 或 `/r`回车符 之后的位置，而 `$` 匹配字符串结束位置以及 `/n`换行符 或 `/r`回车符 之前的位置。

例如：

```python
import re
re1 = re.compile('^1[345789]\d{9}$') # 匹配11位手机号码 从字符串开头匹配到结尾
```

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

