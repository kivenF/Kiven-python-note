# Kiven的python学习笔记

## 20.03.09

### 1.f-string格式化输出

对于格式化的输出我们有%法和format方法，但是还有一个在python3.6提出的更方便的方法，使用f-string

形式：

```python
name = 'Kiven'
print(f'{name} is the best!')
```

输出：

```
Kiven is the best!
```

就是在字符串前加上f

另外我们也可以用它来进行格式化小数点的位数：

```python
a = 1
b = 3
print(f'1/3 ={a/b:.2f}')
```

输出:

```
1/3 =33.33
```

可以看到{}内是可以计算的，：后就是输出的格式，先是位数后面加转换形式。

既然括号内可以计算那么它还可以放lambda表达式，只不过放表达式时：号会冲突，所以需要加上一个()：

```python
print( f'{(lambda x : x**2 ) (3) }')
```

输出：

```
9
```

### 2.re.split()

字符串对象的split()方法只能处理非常简单的情况，而且不支持多个分隔符，对分隔符周围可能存在的空格也无能为力，所以当遇到一些更为复杂的字符串时应该使用re.split()

首先需要引入re模块：

```python 
import re
line = 'Apple , IBM , SAMSUNG ; HUAWEI Google'
band = re.split(r'\s*[;,\s]\s*', line)
print(band)
```

输出：

```
['Apple', 'IBM', 'SAMSUNG', 'HUAWEI', 'Google']
```

### 3.在字符串的开头或结尾做文本匹配

例如我们在检查文件的拓展名或者url协议等情况时会用到

**方法**：str.startswith()和str.endswith()

```python
filename = 'README.md'
print(filename.endswith('.text'))
print(filename.endswith('.md'))
```

```
False
True
```

另外如果我们需要进行多个选项的匹配，可以给starswith和endswith传递一个元组，但是他们两个不支持传递列表或者集合，如果要传递列表或集合需要前用tuple()进行转换。

```python
filename = ['README.md', 'list.text', 'xxx.mp3', 'abc.pdf']
form = ['.text', '.mp4', '.mp3']

for name in filename:
    if name.endswith(tuple(form)):
        print(name)
```

输出：

```
list.text
xxx.mp3
```

另外 str.find()	返回出现字符次数

### 4.re的一些方法

#### （1）match()

​	<u>从开头匹配</u>，如果匹配到则返回符合的匹配对象，若无则返回None

```python
import re
print(re.match(r'.*.text', 'abcdef list.text abcd'))
```

输出：

```
<re.Match object; span=(0, 16), match='abcdef list.text'>
```

返回的是一个匹配对象，要想获取匹配对象的文本可以使用匹配对象的**.group**方法

**match配合捕获组**

```python
import re
my_str = '2020/03/09，。。。。'
date = re.match(r'(\d*)/(\d+)/(\d*)',my_str)
for i in range(4):
    print(date.group(i))

print(date.groups())
```

输出：

```
2020
03
09
('2020', '03', '09')
```

#### （2）compile()

​	当需要用同一种模式进行多次匹配，那么先预编译一个模式对象是好的。

```python
import re

filename = 'list.text'
a = re.compile(r'.*.text')
print(a.match('abcdef list.text abcd'))
```

输出：

```
<re.Match object; span=(0, 16), match='abcdef list.text'>
```

#### （3）findall() & finditer()

都是搜索整个文本返回匹配对象，findall()以列表的形式返回。

finditer()返回一个迭代器，可以用迭代的方法来找出匹配项。