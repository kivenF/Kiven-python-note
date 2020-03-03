# kiven的python学习笔记

## 20.03.01

### 1.快速交换法和，星号解包法

* **快速交换两个元素**：

  ```python
  a = 1
  b = 2
  a, b = b, a
  print('a=%d' % a)
  print('b=%d' % b)
  ```

  **运行输出：**

  ``` python
  a=2
  b=1
  ```

  这里的a , b相当于是（a,b)，一个元组

  

### 2.深入理解 * 和**

* **函数形参**

  ``` python
  def func(A, *B , **C):
  	...
  ```

  在函数func的参数中 *b 表示给函数传递一个元组或者列表，而 ** 表示给函数传递一个字典。

  **注意**：

  1. 在不知道向函数传递参数个数或很多参数时采用这种方式。
  2. 如果同时使用 \*b 和 \*\*c 时，必须 *b 参数列要在 \*\*c 之前。

  **实例**：

  ``` python
  def func(A, *B, **C):
      print('A=%s' % A)
      print('B=%s' % (B,))
      print('C=%s' % (C,))
  
  
  func(1, 2, 3, 4, 5, fruit='aplle', price=3)
  ```

  **输出**：

  ``` python
  A=1
  B=(2, 3, 4, 5)
  C={'fruit': 'aplle', 'price': 3}
  ```

  但是，如果将列表和字典作为实参传递给func会产生什么情况呢？

  **实例**：

  ``` python
  def func(A, *B, **C):
      print(A)
      print(B)
      print(C)
  
  
  a = 1
  b = [1, 2, 3, 4, 5, 6, 7, 8, 9]
  c = {'weight': 50, 'height': 170, 'country': 'China'}
  
  func(a, b, c)
  ```

  **输出**：

  ``` python
  A=1
  B=([1, 2, 3, 4, 5, 6, 7, 8, 9], {'weight': 50, 'height': 170, 'country': 'China'})
  C={}
  ```

  ​	可以看到，此时B时一个元组，内部包含了一个列表和一个字典，而C是一个空字典。这是为什么呢？因为在传递参数的过程中需要用到：

  ``` python
  keyname = value
  ```

  这样的格式表明我们传递的是一个字典的内容，而我们将一个字典作为参数直接传入，表示我们传递是一个整体。

* **函数实参**

  如果我们定义了一个函数：

  ``` python
  def func(A, B, C):
      ...
  ```

  这个函数中的形参没有 * 和 ** 代表它是一个定长的函数。那么我们可以调用 *a 和 \*\*b 对元组接触引用。         **单个 \*\* 的用法:**

  ``` python
  # 用于列表
  def func(A, B, C):
      print('A=%s' % A)
      print('B=%s' % B)
      print('C=%s' % C)
  
  
  a = [1, 2, 3]
  
  func(*a)
  
  ```

  **输出**：

  ``` python
  A=1
  B=2
  C=3
  ```

  将列表或元组解除引用后对应函数中的形参，形参数要与解包后数量对应。可以结合形参 * 法：

  ``` python
  # 将函数定义为：
  def func (*A):
      ...
  ```

  **下面是对字典的的** * ：

  ``` python
  # 字典
  def func(A, B, C):
      print('A=%s' % A)
      print('B=%s' % B)
      print('C=%s' % C)
  
  
  a = {'A':1, 'B':2, 'C':3}
  
  func(*a)
  ```

  **输出**：

  ``` python
  A=A
  B=B
  C=C
  ```

  可以看到，对字典进行单次 * 的解除引用，得到的是键(key)

  

  **下面是\*\* 作为实参：**

  ``` python
  # 字典
  def func(A, B, C):
      print('A=%s' % A)
      print('B=%s' % B)
      print('C=%s' % C)
  
  
  a = {'A':1, 'B':2, 'C':3}
  
  func(**a)
  ```

  使用 ** 作为实参传入需要注意：传入的值是根据键——值对应匹配的，如果不对应导致实参与形参不对应，将抛出错错误。

* **解包**：

  ```python
  p = [123, 324, 123, 34, 6, 568, 24, 475]
  a, *b, c = p
  print('a=%s' % a)
  print('b=%s' % b)
  print('c=%s' % c)
  ```

  **运行输出：**

  ``` python
  a=34
  b=[324, 6, 475, 568, 24]
  c=123
  ```

  这里将 *b 是表示一个列表，在这里a和c占用了最前面和最后一个位置，python自动把中间剩余的数据赋给了 *b

<u>注意：没有**解包的用法</u>



### 3.TypeError: not all arguments converted during string formatting

**实例**：

``` python
strs = (1, 2, 3, 4, 5)
print('strs=%s' %strs)
```

将会报错弹出TypeError: not all arguments converted during string formatting，原因在于% 操作符只能直接用于字符串(‘123’)，列表([1,2,3])和元组(a,)。当用于元组时，**需要显示地指出**后面这个是一个元组。当%strs时，会将strs当作一个元组进行解包，将格式化。那么将会产生5个字符，这5字符需要5个%s来插入前面的字符串。

所以解决的方法有下面两种：

**方法一：**

``` python
strs = (1, 2, 3, 4, 5)
print('strs=%s%s%s%s%s' %strs)
```

**输出**：

``` python
strs=12345
```

**方法二：**

``` python
strs = (1, 2, 3, 4, 5)
print('strs=%s' % (strs,))
```

**输出：**

```
strs=(1, 2, 3, 4, 5)
```

第一种方法采用拆包，第二种方法将 strs 加入到一个元组中。
