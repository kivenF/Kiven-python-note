# Kiven的python学习笔记

## 20.03.15

### 1.字符串的连接与合并

```python
A = ['I', 'love', 'programming', '!']
print(' '.join(A))
```

OUT:

```
I love programming !
```

在一些简单拼接上，可以使用+法，但是这样的效率很低

但是join只能用于字符串操作，如果想对列表中的数字也进行操作就需要进行转换：

```python
A = ['APPLE', 100, 20, 30]
b = (' '.join(str(i) for i in A))
print(b)
```

OUT:

```
APPLE 100 20 30
```

对于一些字符串的连接还可以使用print的内置参数sep:

```python
print('a'+':'+'b'+':'+'c')          # 丑
print(':'.join(['a', 'b', 'c']))    #还是很丑
print('a', 'b', 'c', sep=':')       #这是好的
```

OUT:

```
a:b:c
a:b:c
a:b:c
```

**写入文件时选择哪种拼接方式更高效**

```python
# version 1
f.write(str1+str2)
# version 2
f.write(str1)
f.write(str2)
```

如果str1和str2都是比较小的字符串选择第一种，因为一次I/O系统调用的固有开销很高。但是如果字符串很大就应该选择第二种，这样可以避免先创建一个大的临时结果，也没有对大块内存进行拷贝。

当然也可以考虑生成器

### 2.以固定的列数打印

```python
import textwrap

s = "********************\
*********************\
*********************\
*********************\
*********************"
print(textwrap.fill(s, 10))
```

OUT:

```
**********
**********
**********
**********
**********
**********
**********
**********
**********
**********
****
```

### 3.精确到某位----round(a,b)

```python
a = 13.14159

print(round(a,1))
print(round(a,-1))
```

OUT:

```
13.1
10.0
```

当某个值恰好为两个整数间的一半时，取整操作会取值到最接近的偶数，如1.5和2.5精确到2。