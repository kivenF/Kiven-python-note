# Kiven的python学习笔记

## 20.03.03

### 1.实现优先级队列

**实例：**

``` python
import heapq


class PriorityQueue:
    '''一个实现优先级队列的类'''

    def __init__(self):
        self._queue = []
        self._index = 0

    def push(self, item, priority):
        # 将item按照优先级插入到_queue中
        heapq.heappush(self._queue, (-priority, self._index, item))
        # 这里对priority取负是因为堆默认为从小到大排列，
        # 此处的优先级比较方法为先按照元组里第一个元素进行比较，然后再根据第二个元素进行比较，依次类推
        # 此处先比较优先级，再比较插入顺序，再比较字符的大小
        self._index += 1

    def pop(self):
        return heapq.heappop(self._queue)[-1]
        # 此处的负一目的是只打印最后的字符，否则打印出来的为(5, 1, Item('bar'))

# 更改字符格式
class Item:
    def __init__(self, name):
        self.name = name

    def __repr__(self):
        return 'Item({!r})'.format(self.name)

q = PriorityQueue()
q.push(Item('foo'), 1)
q.push(Item('bar'), 5)
q.push(Item('spam'), 4)
q.push(Item('grok'), 1)

for i in range(3):
    print(q.pop())
```

**输出：**

```
Item('bar')
Item('spam')
Item('foo')
```



**从中学到了什么：**

1. 两个元组是可以比较大小的，比较的规则是从左往右依次比较

   ```python
   a = (111, 33, 22)
   b = (222, 11, 22)
   c = (111, 22, 33)
   print(a > b)
   print(a > c)
   ```

   输出：

   ```
   False
   True
   ```

   但是，我们定义的Item类是不能比较的，所以在在第一个元素优先级相同的情况下，用index来对区分相同优先级就显得尤为重要。

2. 这里的\_\_repr\_\_是一个魔法方法，用于可以在调用\_\_str\_\_时默认调用，这样打印Item对象时不至于打印出内存地址

### 2.zip()

**zip()** 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的对象(一个迭代器),可以用list()来转换成列表输出。

下面是一个查找字典最大值的程序，里面用到zip()来生成一个新的元组（交换了原来的键和值）

```python
prices = {
    'ACME': 45.23,
    'APPLE': 612.78,
    'IBM': 205.55,
    'HPQ': 37.20,
    'FB': 10.75,
}

price_and_names = zip(prices.values(), prices.keys())
print(max(price_and_names))
```

**输出：**

```
(612.78, 'APPLE')
```

zip()是一个迭代器，迭代器只运行前进不允许后退，下面一个程序可以说明这一点

```python
prices = {
    'ACME': 45.23,
    'APPLE': 612.78,
    'IBM': 205.55,
    'HPQ': 37.20,
    'FB': 10.75,
}

price_and_names = zip(prices.values(), prices.keys())
print(price_and_names)
print(max(price_and_names))
print(min(price_and_names))
```

**输出：**

```
<zip object at 0x0000023E5E600BC0>
(612.78, 'APPLE')
ValueError: min() arg is an empty sequence
```

可以看到，zip()函数返回的是一个对象，它的内容只允许被消费一次。当我们第二次访问时，抛出了ValueError的错误。
