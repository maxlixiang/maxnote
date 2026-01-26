```python
print("1024*768=",1024*768)

print("I\'m ok")

print(r"\n\t\\")

print('''line1

line2

line3

这里添加什么可以

只要最后是结束的三个单引号\'\'\'

''')
name = "小明"
age = 20
print(f"名字是{name}，年龄是{age}。")  # 输出: 名字是小明，年龄是20。



```


这里有3个地方值得注意
	1. \是转移符
	2. r""，表示双引号内部的内容不会被转义，r''，两个单引号也可以表示同样的意思
	3. 三个单引号'''
	4. print(f)的用法
```python
s4 = r'''Hello,

I'm here.

Bob!'''
print(s4)
```

这种变量本身类型不固定的语言称之为**动态语言**，与之对应的是**静态语言**。静态语言在定义变量时必须指定变量类型，如果赋值的时候类型不匹配，就会报错。例如Java是静态语言，赋值语句如下（// 表示注释）：
```java
int a = 123; // a是整数类型变量
a = "ABC"; // 错误：不能把字符串赋给整型变量
```


请不要把赋值语句的等号等同于数学的等号。比如下面的代码：

```python
x = 10
x = x + 2
```

如果从数学上理解`x = x + 2`那无论如何是不成立的，在程序中，赋值语句先计算右侧的表达式`x + 2`，得到结果`12`，再赋给变量`x`。由于`x`之前的值是`10`，重新赋值后，`x`的值变成`12`。

最后，理解变量在计算机内存中的表示也非常重要。当我们写：

```python
a = 'ABC'
```

时，Python解释器干了两件事情：

1. 在内存中创建了一个`'ABC'`的字符串；
2. 在内存中创建了一个名为`a`的变量，并把它指向`'ABC'`。

也可以把一个变量`a`赋值给另一个变量`b`，这个操作实际上是把变量`b`指向变量`a`所指向的数据，例如下面的代码：

```python
a = 'ABC'
b = a
a = 'XYZ'
print(b)
```

最后一行打印出变量`b`的内容到底是`'ABC'`呢还是`'XYZ'`？如果从数学意义上理解，就会错误地得出`b`和`a`相同，也应该是`'XYZ'`，但实际上`b`的值是`'ABC'`，让我们一行一行地执行代码，就可以看到到底发生了什么事：

执行`a = 'ABC'`，解释器创建了字符串`'ABC'`和变量`a`，并把`a`指向`'ABC'`：

![py-var-code-1](https://liaoxuefeng.com/books/python/basic/data-types/step-1.png)

执行`b = a`，解释器创建了变量`b`，并把`b`指向`a`指向的字符串`'ABC'`：

![py-var-code-2](https://liaoxuefeng.com/books/python/basic/data-types/step-2.png)

执行`a = 'XYZ'`，解释器创建了字符串'XYZ'，并把`a`的指向改为`'XYZ'`，但`b`并没有更改：

![py-var-code-3](https://liaoxuefeng.com/books/python/basic/data-types/step-3.png)

所以，最后打印变量`b`的结果自然是`'ABC'`了。



对变量赋值`x = y`是把变量`x`指向真正的对象，该对象是变量`y`所指向的。随后对变量`y`的赋值_不影响_变量`x`的指向。



# 字符串和编码

## 编码
- ASCII = 一本只收录**英文字母、数字、标点**的小字典，因为最初计算机只能使用英文，所以这是最早的编码方案
- Unicode = 一本收录**全球所有语言字符**的大字典，包含所有语言
- UTF-8 = 这本大字典的**精简排版版本**（按需占用空间），为了节省空间，比如如果都是传输英文，那么这里用unicode就不划算了

计算机内存中使用的是Unicode，计算机存储的时候通常用UTF-8

### 三者的核心关联总结

1. **子集关系**：ASCII 是 Unicode 的**子集**，所有 ASCII 字符的 Unicode 码点与 ASCII 编码值完全一致。
2. **实现关系**：UTF-8 是 Unicode 的**编码实现方式**，Unicode 是字符集标准，UTF-8 是存储传输的具体方案。在UTF-8中，英文占 1 字节，中文占 3 字节
3. **进化关系**：ASCII → Unicode（解决多语言兼容）→ UTF-8（解决存储效率）。


==在最新的Python 3版本中，字符串是以Unicode编码的，也就是说，Python的字符串支持多语言==

## ord()和chr()函数

### 1. ord () 函数：字符 → Unicode 码点

- **作用**：接收一个**单个字符**（长度为 1 的字符串）作为参数，返回该字符对应的 Unicode 十进制整数（码点）。
- **语法**：`ord(c)`，其中 `c` **必须是长度为 1 的字符串。**
```python
# 英文字符
print(ord('A')) # 输出：65（A的ASCII/Unicode码点）
print(ord('z')) # 输出：122

# 中文字符 
print(ord('中')) # 输出：20013（对应Unicode码点U+4E2D的十进制值） 
print(ord('国')) # 输出：22269



```
### 2. chr () 函数：Unicode 码点 → 字符

- **作用**：接收一个**Unicode 十进制码点**（整数）作为参数，返回该码点对应的字符。
- **语法**：`chr(i)`，其中 `i` 是 0 到 1114111（`0x10FFFF`）之间的整数。


如何理解这句话：由于Python的字符串类型是`str`，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把`str`变为以字节为单位的`bytes`。
python中的字符串在内存中用Unicode表示，如果想存储在磁盘中，必须编码encode()成字节，这个过程是从Unicode->UTF-8
同样的，python从磁盘中读取文件，必须解码decode()成Unicode字符


### 总结

1. Python 的 `str` 是**内存中**的字符抽象，基于 Unicode 统一管理所有语言字符，方便编程处理；
2. 网络传输 / 磁盘存储只认二进制字节，因此必须通过 `encode()` 将 `str` 转成 `bytes`（指定编码如 UTF-8）；
3. 读取 / 接收数据时，需通过 `decode()` 将 `bytes` 转回 `str`（==编码格式必须和编码时一致，否则会乱码==）。

简单记：`str` 是给程序员看的 “字符”，`bytes` 是给机器 / 网络看的 “二进制数据”，二者通过编码 / 解码转换。

```python
# 1. 内存中的 str（Unicode 表示）
s = '中国'
print(type(s))  # 输出：<class 'str'>
print(len(s))   # 输出：2（按“字符数”统计，而非字节）

# 2. 编码：str → bytes（指定 UTF-8 编码，网络/存储常用）
b = s.encode('utf-8')
print(type(b))  # 输出：<class 'bytes'>
print(b)        # 输出：b'\xe4\xb8\xad\xe5\x9b\xbd'（UTF-8 字节序列）
print(len(b))   # 输出：6（每个汉字占 3 字节，2*3=6）

# 3. 解码：bytes → str（必须和编码格式一致，否则乱码）
s2 = b.decode('utf-8')
print(s2)       # 输出：中国（还原成原字符串）

# 错误示例：用错误编码解码会报错/乱码
try:
    b.decode('gbk')
except UnicodeDecodeError as e:
    print("解码错误：", e)  # 输出：UnicodeDecodeError: 'gbk' codec can't decode...
```



https://liaoxuefeng.com/books/python/basic/string-encoding/index.html 


## 格式化

```python
name = "张三"
age = 10
print("姓名是%s，年龄是%d" %(name,age))

print("姓名是{name}，年龄是{age}" )
```


## list和tuple




## for循环中的解包

问题：
*python的for循环可以使用两个变量，我不太理解下面的程序，请解释*
*for x, y in [(1, 1), (2, 4), (3, 9)]:*
    *print(x, y)*
回答：
Python for 循环中**同时遍历两个变量**的用法，这个写法核心是「序列解包」，Python 支持「把一个可迭代对象（比如元组）的元素，一次性赋值给多个变量」，这个过程叫**解包**。循环的执行过程就像逐次执行下面的赋值语句：
![500](assets/Day1%20输入输出函数、=赋值、变量/file-20260126212937235.png)