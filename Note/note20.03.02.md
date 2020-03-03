# kiven的python学习笔记

## 20.03.02

### 1.deque-一个可以设置长度的队列（列表）

``` python
from collections import deque

# 创建一个队列，宽度为8
q = deque(maxlen=8)
# 将1插入到队列（右边）
q.append(1)
print(q)
# 将2插入到队列（右边）
q.append(2)
print(q)
# 将0插入到队列（左边）
q.appendleft(0)
print(q)
# 将队尾元素移除，并返回这个元素
q.pop()
print(q)
# 将队首元素移除，并返回这个元素
q.popleft()
print(q)
```

**输出：**

``` 
deque([1], maxlen=8)
deque([1, 2], maxlen=8)
deque([0, 1, 2], maxlen=8)
deque([0, 1], maxlen=8)
deque([1], maxlen=8)
```

首先，通过collections引入deque，然后就就可以通过deque声明队列对象。

deque的操作基本和列表相同，其最大特点是可以设置长度，超过长度将会把多余的元素挤出，可以用于保存最新的N个数据。

**注：**这个挤出的方法是通过append()和appendleft()来实现的（联系一下实际），虽然insert()方法可以用于deque，但是当超过maxlen后，将不能使用，原因：无法判断该挤出哪个元素

另外的，如果在声明deque对象时，不设置队列的大小，会得到一个无限大小的队列，还有通过*q是不会得到maxlen = 8的。

**注注注!:**<u>准确的说deque是一个双向链表，在开头和末尾插入时间复杂度都是O(1)，这与列表不同，列表在开头插入时O(N)，末尾插入为O(1)。</u>

### 2.heapq

* **nlargest() 和 nsmallest() **

两个函数用于返回序列中的最小的n个元素：

```python
import heapq

q = [24, 12, 43, 23, 57, 253, -34]
print('q中最大的三个元素是：', heapq.nlargest(3, q))
print('q中最小的三个元素是：', heapq.nsmallest(3, q))
# 将列表进行堆调整
heapq.heapify(q)
print('q=%s' % q)
# 将堆顶元素弹出
a = heapq.heappop(q)
print('a=%s' % a)
print('q=%s' % q)
```

**输出：**

```
q中最大的三个元素是： [253, 57, 43]
q中最小的三个元素是： [-34, 12, 23]
q=[-34, 12, 24, 23, 57, 253, 43]
a=-34
q=[12, 23, 24, 43, 57, 253]
```

heaplify用于将一个列表进行堆的初始化，默认为小为堆顶，之后heappop()弹出堆顶元素就是最小元素。

nlargest()和nsmallst()还可以接受一个关键词用于更复杂的数据结构：

``` python
heapq.largest(size, arr, key)
```

