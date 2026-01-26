1. 切片
- 元组的切片仍然是元组，不可改变，答案是A
![500](assets/Day3%20高级特性-切片和迭代/file-20260126212938943.png)
2. . 迭代
		- 要注意下面程序中print(d.keys()),返回的内容
```python

d = {'a': 1, 'b': 2, 'c': 3, 'd': 4,'e': 5,'f': 6,'g': 7,'h': 8,'i': 9,'j': 10}

print(d.keys())  # 返回：dict_keys(['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j'])

print("--------------------------------")

for key in d.keys():

    print(key)
print(d.items()) # 注意这里返回的是一个元组
```


- 	- 如何判断是否是迭代对象呢?方法是通过`collections.abc`模块的`Iterable`类型判断，关于可迭代对象，还有一个重要的知识点是“解包” [Day1 输入输出函数、=赋值、变量](Day1%20输入输出函数、=赋值、变量.md)]]
- 
	- 
	```python
```plain
>>> from collections.abc import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```
	```
- 如果要对`list`实现类似Java那样的下标循环怎么办？Python内置的`enumerate`函数可以把一个`list`变成索引-元素对，这样就可以在`for`循环中同时迭代索引和元素本身
```python

```plain
>>> for i, value in enumerate(['A', 'B', 'C']):
...     print(i, value)
...
0 A
1 B
2 C
```

