# Kiven的python学习笔记

## 20.03.04

### 1.字典中寻找相同点

**代码：**

``` python 
a = {
    'x': 1,
    'y': 2,
    'z': 3,
}

b = {
    'w': 10,
    'x': 11,
    'y': 2,
}

print('a和b中共有的key:%s' % (a.keys() & b.keys()))
print('a和b中共有的key-value:%s' % (a.items() & b.items()))
print('a比b多的key:%s' % (a.keys()-b.keys()))

c = {key: a[key] for key in a.keys() - {'z', 'w'}}
print(c)
```

```
a和b中共有的key:{'x', 'y'}
a和b中共有的key-value:{('y', 2)}
a比b多的key:{'z'}
{'x': 1, 'y': 2}
```

字典就是一系列键和值之间的映射集合，所以它支持集合中的交集、并集等操作。但是.values()不能保证所以的值是唯一的，所以values()不支持集合操作。

其中最后的：

``` python
c = {key: a[key] for key in a.keys() - {'z', 'w'}}
```

相当于：

```python
c = {key: a[key] for key in (a.keys() - {'z', 'w'})}
```

是先用a.keys()-set得到一个set，然后再由for循环进行键值配对生成新字典，最后由c变量指向这个字典。

最开始，不知道运算顺序，折腾半天看不懂（现在知道多代码中多加括号的好处了。。。。）

### 2.lambda表达式

​	lambda一个匿名函数，在python中万物皆对象，一个函数也是如此

``` python
def f(a):
    print(a)

b = 1
f(1)
g = f
g(1)
```

输出：

```
1
1
```

可以看到语句 g = f 让g也指向了这个函数对象。

那么lambda表达式也是如此，它返回一个函数对象。上面的代码还可以写成：

```python
f = lambda a:print(a)
f(1)
```

输出：

```
1
```

lambda表达式中, ' : '是分割线，分割线前为参数传给分割线后，分割线后相当于def函数的内部。但是lambda表达式只能用一行表达式，当然它也是为了这样的使用场景而生的。

lambda表达式常用于排序时给函数传递一个key参数。

### 3.从序列中移除重复项目且保持元素间顺序不变

**（1）哈希表**

​	要想去除重复元素，可以用到哈希表，可以set() （集合）

​	在python中，如果一个对象是可哈希的，那么他的生存期内必须是不可变的，它需要有一个\_\_hash\_\_()方法。整数、浮点数、字符串、元组都是不可变的。

​	不可哈希的对象：如列表、字典。

​	**(2)代码**

```python
def dedupe(items, key=None):    # 带有默认参数的函数，适用于可哈希对象和不可哈希对象
    seen = set()                # 设置一个哈希表（集合）
    for item in items:          # 将列表或字典中的元素一一访问
        val = item if key is None else key(item)
        # 如果key是None（也就是没有传入一个函数对象，这时items是一个可哈希对象）
        # 否则用key函数（配合lambda)将items转换为可哈希对象
        if val not in seen:
            yield item
            # 核实不在哈希表中，将item返回给外面的list，list是有插入顺序关系的
            seen.add(val)
            # 将val加入哈希表


a = [{'x': 1, 'y': 2}, {'x': 1, 'y': 3}, {'x': 1, 'y': 2}, {'x': 2, 'y': 4}]
print(list(dedupe(a, key=lambda d: (d['x'], d['y']))))
# lambda表达式给key一个函数对象，该函数可以根据字典返回一个包含字典的元组（元组是可哈希的）
print(list(dedupe(a, key=lambda d:d['x'])))
```

**输出：**

```
[{'x': 1, 'y': 2}, {'x': 1, 'y': 3}, {'x': 2, 'y': 4}]
[{'x': 1, 'y': 2}, {'x': 2, 'y': 4}]
```

从中学到了什么：

​	对于一个不可以哈希的对象，我们可以将它放入到元组中，再把这个元组放入哈希表