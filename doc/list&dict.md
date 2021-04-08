# 列表和字典

## 列表

列表是可变序列，通常用于存放同类项目的集合。

下面是常用的列表字面量和操作：

```python
L = [] # 空列表
L = [123, 'abc', 1.23, {}] # 四项，索引为0到3
L = ['Bob', 40.0, ['dev', 'mgr']] # 嵌套的子列表
L = list('spam') # 一个可迭代对象元素的列表
L = list(range(-4, 4)) # 连续整数的列表
L[i] # 索引
L[i][j] # 索引的索引
L[i:j] # 分片
len(L) # 求长度
L1 + L2 # 拼接
L * 3 # 重复
for x in L: print(x) # 迭代
3 in L # 成员关系
L.append(4) # 尾部添加
L.extend([5,6,7]) # 尾部扩展
L.insert(i, x) # 插入
L.index(x) # 索引
L.count(x) # 统计元素出现次数
L.sort() # 排序
L.reverse() # 反转
L.copy() # 复制（python3.3及以上）
L.clear() # 清除（python3.3及以上）
L.pop(i) # 删除i处元素，并将其返回
L.remove(x) # 删除元素x
del L[i] # 删除i处元素
del L[i:j] # 删除i到j处的片段
L[i:j] = [] # 删除i到j的片段
L[i] = 3 # 索引赋值
L[i:j] = [4, 5, 6] # 分片赋值
L = [x**2 for x in range(5)] # 列表推导和映射（见第4章，第14章和第20章）
list(map(ord, 'spam'))
```

### 列表的实际应用

* 列表是序列，和字符串一样也有 `+` 和 `*` 操作，代表拼接和重复，结果是一个新的列表：

```python
len([1, 2, 3])
3

[1, 2, 3] + [4, 5, 6]
[1, 2, 3, 4, 5, 6]

['Ni!'] * 4
['Ni!', 'Ni!', 'Ni!', 'Ni!']
```

`+` 两边必须是相同类型的序列，否则会出现类型错误。

* 列表推导只是通过对序列中每一项应用一个表达式来构建一个新的列表：

```python
res = [c * 4 for c in 'SPAM']
res
['SSSS', 'PPPP', 'AAAA', 'MMMM']
```

内置函数 map 能实现类似的效果：

```python
list(map(abs, [-1, -2, 0, 1, 2]))
[1, 2, 0, 1, 2]
```

* 列表索引操作返回指定偏移处的对象，而列表分片会返回新的列表

```python
L = ['spam', 'Spam', 'SPAM!']
L[2]
'SPAM!'

L[-2]
'Spam'

L[1:]
['Spam', 'SPAM!']
```

* 索引赋值和分片赋值都是原位置修改，直接对原列表修改而不生成新的。

```python
L = ['spam', 'Spam', 'SPAM!']
L[1] = 'eggs'

L
['spam', 'Spam', 'SPAM!']

L[0:2] = ['eat', 'more']
L
['eat', 'more', 'SPAM!']
```

分片赋值最好分成两步来理解：

1. 删除。删除等号左边指定的分片。
2. 插入。将等号右边可迭代对象中的片段插入旧分片被删除的位置

```python
L = [1, 2, 3]
L[1:2] = [4, 5] # 删除元素后插入新元素
L
[1, 4, 5, 3]

L[1:1] = [6, 7] # 插入新元素
L
[1, 6, 7, 4, 5, 3]

L[1:2] = [] # 删除元素
L
[1, 7, 4, 5, 3]

L = [1]
L[:0] = [2, 3, 4] # 在列表头部拼接
L
[2, 3, 4, 1]

L[len(L):] = [5, 6, 7] # 在列表尾部拼接
L
[2, 3, 4, 1, 5, 6, 7]

L.extend([8, 9, 10]) # 在列表尾部拼接
L
[2, 3, 4, 1, 5, 6, 7, 8, 9, 10]
```

* `append` 是很常用的列表方法，它将一个单项添加至列表尾部，`L.append(x)` 和 `L + [x]` 的效果类似，但是 `L.append(x)` 会修改原列表，而 `L + [x]` 会生成新的列表。

```python
L = ['eat', 'more', 'SPAM!']
L.append('please')
L
['eat', 'more', 'SPAM!', 'please']

L.sort()
L
['SPAM!', 'eat', 'more', 'please']
```

* `sort` 方法有两个参数，`reverse` 参数允许排序以降序而不是升序，`key` 参数是一个有单个参数的函数，它返回在排序中使用的值。`sort`在原位置对列表进行排序。

```python
L = ['abc', 'ABD', 'aBe']
L.sort()
L
['ABD', 'aBe', 'abc']

L = ['abc', 'ABD', 'aBe']
L.sort(key=str.lower)
L
['abc', 'ABD', 'aBe']

L = ['abc', 'ABD', 'aBe']
L.sort(key=str.lower, reverse=True)
L
['aBe', 'ABD', 'abc']
```

注意：混合类型排序在 python 2.x 是行得通的，python 2.x 在不同类型之间定义了一个固定的顺序。而 python 3.x 中混合类型排序则会发生异常。<br>

`append` 和 `sort`都在原位置修改列表并且没有返回值，而内置函数 `sorted` 会返回新的列表： 

```python
L = ['abc', 'ABD', 'aBe']
sorted(L, key=str.lower, reverse=True)
['aBe', 'ABD', 'abc']

L = ['abc', 'ABD', 'aBe']
sorted([x.lower() for x in L], reverse=True)
['abe', 'abd', 'abc']
```

* `reverse` 原位置反转列表，`extend` 在末尾插入多个元素，`pop` 在末尾删除一个元素。`reversed`内置函数返回新的列表，但是它必须包装在一个 list 调用中，因为它返回一个迭代器。

```python
L = [1, 2]
L.extend([3, 4, 5])
L
[1, 2, 3, 4, 5]

L.pop()
5

L
[1, 2, 3, 4]
L.reverse()
L
[4, 3, 2, 1]

list(reversed(L))
[1, 2, 3, 4]
```

`pop` 方法也可以接收一个参数，要被删除返回的元素的偏移量（默认值是最后一个元素的偏移量，也就是-1）。`remove` 删除某元素，`insert` 插入某元素，`count` 计算某元素出现次数，`index` 查找某元素的偏移量。

```python
L = ['spam', 'eggs', 'ham']
L.index('eggs')
1

L.insert(1, 'toast')
L
['spam', 'toast', 'eggs', 'ham']

L.remove('eggs')
L
['spam', 'toast', 'ham']

L.pop(1)
'toast'
L
['spam', 'ham']

L.count('spam')
1
```

* `del` 语句在原位置删除某项或者某片段：

```python
L = ['spam', 'eggs', 'ham', 'toast']
del L[0]
L
['eggs', 'ham', 'toast']

del L[1:]
['eggs']
```

## 字典

