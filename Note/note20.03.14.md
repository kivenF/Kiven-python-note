# Kiven 的python学习笔记

## 20.03.14

### 1.多行正则表达式

当用正则表达式进行通配时，使用 .* 但是句点并不能匹配换行符，这时有两种方法可以解决。

#### 方法一：

​	添加句点对换行符的支持

```python
import re
text = '''/* this is a
multiline comment */
'''
print(re.findall(r'/\*(.*?)\*/', text))
print(re.findall(r'/\*((?:.|\n)*?)\*/', text))
```

OUT:

```
[]
[' this is a\nmultiline comment ']
```

(?:.|\n) 将换行符加入到句点的支持当中，这不是指定一个捕获组！

#### 方法二：

​	用re.compile()添加标记----re.DOTALL使.可以匹配任何所有字符

```python
import re
text = '''/* this is a
multiline comment */
'''
a = re.compile(r'/\*(.*?)\*/', re.DOTALL)
print(a.findall(text))
```

OUT:

```
[' this is a\nmultiline comment ']
```

### 2.从字符串中去掉不需要的字符

通常使用str.strip() str.lstrip() str.rstrip() 去除空白，还可以给他们传递参数来指定去除的字符

```python
s = '    -----hello     world========     \n'
print(s.strip())
print(s.lstrip())
print(s.rstrip())
print(s.strip().strip('-='))
print(s.lstrip().strip('-'))
```

OUT:

```
-----hello     world========
-----hello     world========

    -----hello     world========
hello     world
hello     world========
```

strip只会去除两端的多余字符，不会改变中间

如果想要对中间进行操作或更复杂的修改可以用 str.replace() 和 re.sub()

```python
from re import sub
s = '    -----hello     world========     \n'
print(s.replace(' ', '').replace('-', '').replace('=', '').strip())
print(sub(r'[\s\-\=]+', ' ', s).strip())
```

OUT:

```
helloworld
hello world
```

还可以使用str.translate()

```python
from re import sub
s = '    -----hello     world========     \n'
rmap = {
    ord(' '): None,
    ord('-'): '',
    ord('='): '',
}

a = s.translate(rmap)
print(a)
```

OUT:

```
helloworld
```

### 3.对齐输出

#### （1）使用 str.ljust() str.rjust() str.center()

```python
s = 'World'
print(s.ljust(20))
print(s.rjust(20))
print(s.center(20, '*'))
```

OUT:

```
World
               World
*******World********
```

#### （2）使用format 或者 f-string

```python
s = 'World'
print(f'{s:=>20}')
print(f'{s:*^20}')
print('{:-<20}'.format(s))
```

OUT:

```
===============World
*******World********
World---------------
```

简而言之 ：> 是右对齐, < 是左对齐 ， ^是居中 ，这三个符号前的就是占位符，默认空格







