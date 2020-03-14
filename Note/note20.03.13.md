# kiven的python学习笔记

## 20.03.13

### 1.在sub中指定一个替换回调函数

当我们需要对文本中进行更为复杂的替换时，可以指定一个替换回调函数。

```python
from calendar import month_abbr
from re import sub


def change(m):
    mon_name = month_abbr[int(m.group(2))]
    return f'{m.group(1)} {mon_name} {m.group(3)}'


my_str = 'Today is 12/03/2020.'
Date = sub(r'(\d)+/(\d+)/(\d+)', change, my_str)
print(Date)
```

OUT:

```
Today is 2 Mar 2020.
```

指定了一个change()函数来对匹配文本进行处理再返回，用来替换匹配内容。

这个程序中使用了canlendar模块，它是一个包含日期处理的模块，引入了一个month_abbr用来把数字月份转换为英语月份。

### 2.以不区分大小写的方式对文本做调查和替换

要不区分大小，只需对re的各个操作加上re.IGNORECASE标记

```python
import re
my_str = 'README.text'
print(re.match('.*.TEXT', my_str, flags=re.IGNORECASE))
```

OUT:

```
<re.Match object; span=(0, 11), match='README.text'>
```

### 3.取消匹配贪婪

```python
import re
str_pat = re.compile(r'\"(.*)\"')
str_1 = 'Lihua says "no".'
str_2 = 'Lihua says "no" ,but Bob say "yes".'
print(str_pat.findall(str_1))
print(str_pat.findall(str_2))
```

OUT:

```
['no']
['no" ,but Bob say "yes']
```

在默认状况下，如果我们使用像 a* a+ 之类的，正则表达式的匹配尽量按包含的内容多为标准，这有时不是我梦想要的，这就需要取消贪婪。

要取消贪婪很简单，只需要在这类形式后加上？如 a*? a+?

```python
import re
str_pat = re.compile(r'\"(.*?)\"')
str_1 = 'Lihua says "no".'
str_2 = 'Lihua says "no" ,but Bob say "yes".'
print(str_pat.findall(str_1))
print(str_pat.findall(str_2))
```

OUT:

```
['no']
['no', 'yes']
```

### 4.迭代对象和迭代器

​	一个迭代对象不一定时迭代器，迭代器时迭代对象。有\_\_iter\_\_()的就是可迭代对象，可迭代对象被调用时，不一定返回self，它也可以用迭代器类来创建一个它内部属性的迭代器，只要是迭代器就有\_\_next\_\_方法,next方法里面有记录上一次计算的结果，可以在下次调用next时使用，在for语句中，先调用一次\_\_iter\_\_()再不断调用next，next中有一个 raise StopIteration,发出这个报错就会自动停止迭代

```python
class A:  # 定义一个迭代器
    def __init__(self, num):
        self.num = num

    def __next__(self):
        if self.num == 0:
            raise StopIteration
        self.num -= 1
        return self.num

    def __iter__(self):
        return self


class B:  # 定义一个可迭代对象
    def __init__(self, num):
        self.num = num

    def __iter__(self):
        return C(self.num)  # 创建一个num属性的迭代器对象


class C:  # 定义一个迭代器对象
    def __init__(self, num):
        self.num = num

    def __next__(self):
        if self.num == 0:
            raise StopIteration
        self.num -= 1
        return self.num

    def __iter__(self):
        return self


a = A(10)
b = B(10)

for i in a:
    print(i,end=' ,')

print('\n')

for i in b:
	print(i,end =' ,')
```

OUT:

```
9 ,8 ,7 ,6 ,5 ,4 ,3 ,2 ,1 ,0 ,

9 ,8 ,7 ,6 ,5 ,4 ,3 ,2 ,1 ,0 ,
```

**下面用while来模拟迭代器**

```python
a = [1,2,3,4,5]
b = iter(a)

for i in a:
	print(i,end = ' ,')

print('\n')

while True:
	try:
		print(next(b),end=' ,')

	except StopIteration:
		break
```

OUT:

```
1 ,2 ,3 ,4 ,5 ,

1 ,2 ,3 ,4 ,5 ,
```

**模拟列表迭代**

```python
class A:
    def __init__(self, my_list):
        self.MAX = len(my_list)
        self.my_list = my_list
        self.index = 0

    def __next__(self):
        if self.index == self.MAX:
            raise StopIteration

        self.index += 1
        return self.my_list[self.index-1]

    def __iter__(self):
        return self


b = [1, 2, 3, 4, 5]
a = A(b)

for i in a:
    print(i)
```

OUT:

```
1
2
3
4
5
```

