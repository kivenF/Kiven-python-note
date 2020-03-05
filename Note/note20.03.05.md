# Kiven的python学习笔记

## 20.03.05

### 1.给切片命名

给切片命名方便在代码很长时便于理解。

```python
items = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17]
a = slice(3, 14)
print(items[3:14])
print(items[a])
```

输出：

```
[4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14]
[4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14]
```

slice()会创建一个切片对象，可以用于任何运行进行切片操作的地方。这个对象有 start()  stop()  steep()方法：

```python
a = slice(2, 10, 3)
print(a.start)
print(a.stop)
print(a.step)
```

输出：

```
2
10
3
```

依次是切片的起始点、末位、步长。

### 2.BFS&DFS

两者的对比：

|       项目       |                BFS                 |   DFS    |
| :--------------: | :--------------------------------: | :------: |
|     算法思路     | 一条路走到死，碰壁返回上一层再前进 | 逐层递进 |
| 运用到的数据结构 |                队列                |    栈    |

#### 1.BFS

![“BFS”的图片搜索结果](https://ds055uzetaobb.cloudfront.net/brioche/uploads/37D5TDURR1-mm.png?width=1200)

在python中描述图，可以用字典映射。

```python
graph = {
    'A': ['I', 'F', 'B'],
    'B': ['A', 'E', 'C'],
    'C': ['E', 'D', 'B'],
    'D': ['C', 'G', 'H'],
    'E': ['B', 'C', 'G'],
    'F': ['A', 'G'],
    'G': ['E', 'F', 'D'],
    'H': ['G'],
    'I': ['A'],
}


def BFS(graph, s):
    queue = []
    queue.append(s)
    seen = set()
    seen.add(s)
    while(len(queue) > 0):
        vertext = queue.pop(0)
        nodes = graph[vertext]
        for w in nodes:
            if w not in seen:
                queue.append(w)
                seen.add(w)
        print(vertext)


BFS(graph, 'A')
```

输出：

```
A
I
F
B
G
E
C
D
H
```

**队列的作用：**

​	队列从后进前出，保证能够把一层的结点处理完后再处理之后的结点

### 2.DFS

代码和BFS大同小异，唯一的区别就是把队列改给栈。

```python
graph = {
    'A': ['I', 'F', 'B'],
    'B': ['A', 'E', 'C'],
    'C': ['E', 'D', 'B'],
    'D': ['C', 'G', 'H'],
    'E': ['B', 'C', 'G'],
    'F': ['A', 'G'],
    'G': ['E', 'F', 'D'],
    'H': ['G'],
    'I': ['A'],
}


def DFS(graph, s):
    stack = []
    stack.append(s)
    seen = set()
    seen.add(s)
    while(len(stack) > 0):
        vertext = stack.pop()
        nodes = graph[vertext]
        for w in nodes:
            if w not in seen:
                stack.append(w)
                seen.add(w)
        print(vertext)


DFS(graph, 'A')
```

输出：

```
A
B
C
D
H
G
E
F
I
```

**使用栈的目的：**

​	结点只能不断的压进栈，从栈顶取出，每次弹出的都是最新的结点（最深），从而保证深度优先。

### 3.python中的不可变与可变对象

不可变对象：int , string , float , tuple

可变对象：list , dict

```python
a = 22
b = 22
print('a id:'+str(id(a)))
print('b id:'+str(id(b)))
print(a is b)

b+=1
print('a id:'+str(id(a)))
print('b id:'+str(id(b)))
```

输出：

```
a id:140709412198720
b id:140709412198720
True
a id:140709412198720
b id:140709412198752
```

int 是一个不可变的对象，不可变的意思是不对对象指向的内存修改，当修改这类对象时，变量名将指向一个新的内存地址，原来的内存地址将被解除引用。

```python
a = []
b = a
print('a id:'+str(id(a)))
print('b id:'+str(id(b)))
print(a is b)

b.append(1)
print('a =%s' % a)
print('b =%s' % b)
print('a id:'+str(id(a)))
print('b id:'+str(id(b)))
```

输出：

```
a id:2370467737216
b id:2370467737216
True
a =[1]
b =[1]
a id:2370467737216
b id:2370467737216
```

可以看到，将a赋值给b后，对b进行操作时，也影响了a，因为两者的内存指向是相同的。list和dict都是可变的对象，这以为着在生存周期内他们的内存地址将不会改变。

如何才能正确copy一个不可变对象呢?笔记第四点4

**总结**

​	1.在python中对变量赋值实际上是让这个变量名指向值的地址

2. 对不可变对象的值进行修改时内存地址随之改变
3. 可变对象的地址在生存周期内不会改变



### 4.浅复制和深赋值

需要用到copy模块

**浅复制：**

```python
from copy import copy
a = [1, 2, 3, 4, 5]
b = copy(a)
print('a =%s' % a)
print('b =%s' % b)
print('a id:'+str(id(a)))
print('b id:'+str(id(b)))

b.append(6)
print('a =%s' % a)
print('b =%s' % b)
print('a id:'+str(id(a)))
print('b id:'+str(id(b)))
```

输出：

```
a =[1, 2, 3, 4, 5]
b =[1, 2, 3, 4, 5]
a id:1830593900160
b id:1830573255552
a =[1, 2, 3, 4, 5]
b =[1, 2, 3, 4, 5, 6]
a id:1830593900160
b id:1830573255552
```

可以看到浅复制可以返回一个副本，那深复制是干什么的呢？别急先看一段代码

```python
from copy import copy
a = [1, 2, [3, 4, 5]]
b = copy(a)
print('a =%s' % a)
print('b =%s' % b)
print('a id:'+str(id(a)))
print('b id:'+str(id(b)))

b[2][1] = 66666
print('a =%s' % a)
print('b =%s' % b)
print('a id:'+str(id(a)))
print('b id:'+str(id(b)))
```

输出：

```
a =[1, 2, [3, 4, 5]]
b =[1, 2, [3, 4, 5]]
a id:2242071447808
b id:2242071447552
a =[1, 2, [3, 66666, 5]]
b =[1, 2, [3, 66666, 5]]
a id:2242071447808
b id:2242071447552
```

可以看到，我们改变了b列表中嵌套的列表的第二个元素，a也随之改变了，这是因为当我们利用copy拷贝副本给b时只把最外的一层拷贝了副本，里面一层的列表依然是指向与a中的列表相同的地址：

```python
print('a[2] id:'+str(id(a[2])))
print('b[2] id:'+str(id(b[2])))
```

输出：

```
a[2] id:2987793919552
b[2] id:2987793919552
```

这时如果我们想拷贝一个独立的列表我们就可以使用深复制deepcopy

```python
from copy import deepcopy
a = [1, 2, [3, 4, 5]]
b = deepcopy(a)
print('a =%s' % a)
print('b =%s' % b)
print('a id:'+str(id(a)))
print('b id:'+str(id(b)))

b[2][1] = 66666
print('a =%s' % a)
print('b =%s' % b)
print('a id:'+str(id(a)))
print('b id:'+str(id(b)))
```

输出：

```
a =[1, 2, [3, 4, 5]]
b =[1, 2, [3, 4, 5]]
a id:1399641688384
b id:1399641524800
a =[1, 2, [3, 4, 5]]
b =[1, 2, [3, 66666, 5]]
a id:1399641688384
b id:1399641524800
```











