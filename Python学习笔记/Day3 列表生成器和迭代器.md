
## 列表生成器
list(range(10))
```python

L = [x*x for x in range(1,11)]

print(L)
```


## 迭代器

提到迭代器`Iterator`，就要想到它是惰性的，只能用`next()`调用

凡是可作用于`for`循环的对象都是`Iterable`类型；

凡是可作用于`next()`函数的对象都是`Iterator`类型，它们表示一个惰性计算的序列；

集合数据类型如`list`、`dict`、`str`等是`Iterable`但不是`Iterator`，不过可以通过`iter()`函数获得一个`Iterator`对象。

Python的`for`循环本质上就是通过不断调用`next()`函数实现的
