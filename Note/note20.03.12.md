# kiven的python学习笔记

## 20.03.12

### 1.encode()和decode()

​	字符串在python的内部采用的unicode的方式编码，因此在编码转换时，通常需要用unicode作为中间编码，也就是先用decode将其他字符串转换为unicode，再用encode将字符串转换为另一种编码

​	总结如下：

* 对一个非unicode进行encode会报错
* 对一个一个unicode再进行decode()将会报错
* 可以使用isintance()判断是否是指定类型
* 代码中字符串的默认编码与代码文件本身的编码一致

### 2.random模块

#### （1）random()

返回一个0-1的随机浮点数

```python
import random
print(random.random())
```

输出：

```
0.23229654187011317
```

#### （2）uniform(a,b)

返回一个a和b之间的浮点数，a和b的大小不定

```python
from random import *
print(uniform(1, 2))
print(uniform(2,1))
```

输出：

```
1.2452799172185849
1.9829131996944231
```

#### （3）randint(a,b)

和uniform一样，接受两个参数，不过randint()要求a必须小于等于b，返回一个整数

```python
from random import *
print(randint(1,9))
```

输出

```
1
```

#### （4）randrange(a,b,c)

接受一个范围a~b，递增量为c

```python
from random import *
print(randrange(1, 9, 3))
```

OUT:

```
7
```

#### (5) choice(a)

接受一个序列a，从序列a中随机抽取一个元素

```python
from random import *
num = [111, 222, 333, 444, 555]
print(choice(num))
```

OUT:

```
555
```

#### (6) shuffle(a)

接受一个序列a，打乱a的顺序

```python
from random import *
num = [111, 222, 333, 444, 555]
shuffle(num)
print(num)
```

OUT:

```
[222, 555, 444, 111, 333]
```

#### (7) sample(a,n)

接受一个序列a和截取长度n，从a中随机截取长度为n的片段并返回

```python
from random import *
num = [111, 222, 333, 444, 555]
print(sample(num,3))
```

OUT:

```
[333, 555, 111]
```

### 3.str.replace(a,b,n)

对于字符串进行一些简单的文字替换可以使用replace()方法,将字符串中的a替换为b，n为可选参数，表示替换的次数，不选表示全部。

```python
my_str = 'I am doing my homework at this time yesterday.'
my_str.replace('am', 'was')
print(my_str)
print(my_str.replace('am', 'was'))
```

OUT:

```
I am doing my homework at this time yesterday.
I was doing my homework at this time yesterday.
```

**注：**str.replace()并不会改变原string的内容，只是返回一个修改后的对象。

### 4.re.sub()

对于复杂的调整就需要用到re模块了

```python
from re import sub
my_str = 'Date : 12/03/2020'
Date = sub(r'(\d)+/(\d+)/(\d+)', r'\3-\2-\1', my_str)
print(Date)
```

OUT:

```
Date : 2020-03-2
```

\加数字就代表前面捕获组的序号，返回一个修改过后的对象。

同样的我们也可以用compile()将模式编译

```python
import re
my_str1 = 'Date : 12/03/2020'
my_str2 = 'Date : 11/11/2021'
Date_change = re.compile(r'(\d)+/(\d+)/(\d+)')
print(Date_change.sub(r'\3-\2-\1', my_str1))
print(Date_change.sub(r'\3-\2-\1', my_str2))
```

OUT:

```
Date : 2020-03-2
Date : 2021-11-1
```



