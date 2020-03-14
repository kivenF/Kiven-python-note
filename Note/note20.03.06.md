# Kiven的python学习笔记

## 20.03.06

### 1.找出序列中出现次数最多的元素

在一个元素序列中找到出现次数最多的元素可以用到collections模块中的Counter类

```python
from collections import Counter

nums = [
    12, 123, 33, 22, 111, 666,
    666, 3331, 12, 23, 44, 12,
    12, 33, 22, 11, 44, 55, 66,
    111, 12, 1, 12, 55, 66, 33,
]

a = Counter(nums)
print(a.most_common(3))
# 打印出现次数最多的三个元素及它们出现的次数
print(a[66])
# 打印66出现的次数
print(a)

b = [12, 66, 66, 66]
a.update(b)
# 将b中的元素计算加入到a
print(a)

```

输出：

```
[(12, 6), (33, 3), (22, 2)]
2
Counter({12: 6, 33: 3, 22: 2, 111: 2, 666: 2, 44: 2, 55: 2, 66: 2, 123: 1, 3331: 1, 23: 1, 11: 1, 1: 1})
Counter({12: 7, 66: 5, 33: 3, 22: 2, 111: 2, 666: 2, 44: 2, 55: 2, 123: 1, 3331: 1, 23: 1, 11: 1, 1: 1})
```

Counter的底层实现是一个字典，Counter包含 most_common() ，update()方法，同时它也支持集合的操作，字典是不支持集合操作的。

### 2.根据字段将记录分组

需要用的itertools模块中的groupby()函数，他接受一个对象和一个关键字，根据关键字进行分组，不过在此之前需要先按需要分组的项对对象进行排序，goupby()<u>只能检测连续的项。</u>

```python
from itertools import groupby

rows = [
    {'address': '5412 N CLARK', 'date': '07/01/2012'},
    {'address': '5148 N CLARK', 'date': '07/04/2012'},
    {'address': '5800 E 58TH', 'date': '07/02/2012'},
    {'address': '2122 N CLARK', 'date': '07/03/2012'},
    {'address': '5645 N RAVENSWOOD', 'date': '07/02/2012'},
    {'address': '1060 W ADDISON', 'date': '07/02/2012'},
    {'address': '4801 N BROADWAY', 'date': '07/01/2012'},
    {'address': '1039 W GRANVILLE', 'date': '07/04/2012'},
]

rows.sort(key=lambda a: a['date'])
for date, items in groupby(rows, key=lambda a: a['date']):
    # 返回一个value和一个子迭代器，该迭代器可以产生该分组内具有该值的项
    print(date)
    for i in items:
        print(' ', i)
```

输出：

```
07/01/2012
  {'address': '5412 N CLARK', 'date': '07/01/2012'}
  {'address': '4801 N BROADWAY', 'date': '07/01/2012'}
07/02/2012
  {'address': '5800 E 58TH', 'date': '07/02/2012'}
  {'address': '5645 N RAVENSWOOD', 'date': '07/02/2012'}
  {'address': '1060 W ADDISON', 'date': '07/02/2012'}
07/03/2012
  {'address': '2122 N CLARK', 'date': '07/03/2012'}
07/04/2012
  {'address': '5148 N CLARK', 'date': '07/04/2012'}
  {'address': '1039 W GRANVILLE', 'date': '07/04/2012'}
```

### 3.defaultdict()创建一键多值数组

```python
from collections import defaultdict
a = defaultdict(list)
a['x'].append(1)
a['x'].append(2)
a['y'].append(4)

b = defaultdict(set)
b['x'].add(1)
b['x'].add(2)
b['y'].add(4)

print('a =', a)
print('b =', b)
```

输出：

```
a = defaultdict(<class 'list'>, {'x': [1, 2], 'y': [4]})
b = defaultdict(<class 'set'>, {'x': {1, 2}, 'y': {4}})
```

可以看到使用defaultdict()时，括号内传入一个对象类型，这个类型就是字典键--值中值的类型。

我们之后只需不断地像其中添加元素即可。

那么，我们在2.中的分组问题是可以用一键多值字典来解决的。

```python
from collections import defaultdict

rows = [
    {'address': '5412 N CLARK', 'date': '07/01/2012'},
    {'address': '5148 N CLARK', 'date': '07/04/2012'},
    {'address': '5800 E 58TH', 'date': '07/02/2012'},
    {'address': '2122 N CLARK', 'date': '07/03/2012'},
    {'address': '5645 N RAVENSWOOD', 'date': '07/02/2012'},
    {'address': '1060 W ADDISON', 'date': '07/02/2012'},
    {'address': '4801 N BROADWAY', 'date': '07/01/2012'},
    {'address': '1039 W GRANVILLE', 'date': '07/04/2012'},
]

sorted_row = defaultdict(list)
for row in rows:
    sorted_row[row['date']].append(row)

for row in sorted_row:
    print(row, ':')
    for r in sorted_row[row]:
        print(r)
```

输出：

```
07/01/2012 :
{'address': '5412 N CLARK', 'date': '07/01/2012'}
{'address': '4801 N BROADWAY', 'date': '07/01/2012'}
07/04/2012 :
{'address': '5148 N CLARK', 'date': '07/04/2012'}
{'address': '1039 W GRANVILLE', 'date': '07/04/2012'}
07/02/2012 :
{'address': '5800 E 58TH', 'date': '07/02/2012'}
{'address': '5645 N RAVENSWOOD', 'date': '07/02/2012'}
{'address': '1060 W ADDISON', 'date': '07/02/2012'}
07/03/2012 :
{'address': '2122 N CLARK', 'date': '07/03/2012'}
```

也可以做到分组的目的，不过输出的日期不是按照一定顺序的，这是因为字典并不保证有序。

### 4.生成器和列表推导式

|   项目   |          列表推导式          |       生成器       |
| :------: | :--------------------------: | :----------------: |
| 创建方法 | 在[ ]中添加for循环来添加元素 |  在()中添加表达式  |
| 运行区别 |        运行完所有结果        | 每次调用时生成一个 |

生成器相当于是一个算法，按照这个算法我们可以用next()函数推算出下一个元素，而列表是一次性推算出所有结果放在列表中，这很占内存，所以在处理大量数据时多用生成器。

