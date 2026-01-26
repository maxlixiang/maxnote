## 函数定义和调用
```python
def myfun(x):

    '''这是一个求绝对值的函数''' 

    if x >= 0:

        return x

    else:

        return -x

  
  

print(myfun(-100))

print(myfun.__doc__)
```

1. 函数执行到return即结束；如果没有return，则返回none
2. 函数的文档字符串用的实现
3. 函数名其实就是指向一个函数对象的引用，完全可以把函数名赋给一个变量，相当于给这个函数起了一个“别名”
4. 空函数，空函数也是有用的，如果编程过程中这一部分没有想好，可以先留下一个空函数，后面补充。
```python
def nop():
    pass
```
5. 函数的返回值可以是多个，Python的函数返回多值其实就是返回一个**tuple**
6. 


## 函数的参数

1. 位置参数也叫必选参数
	- 这是最基本的函数参数类型，调用的时候必须按照**顺序**传入相应参数
	```python
	def info(name,age):

    print(f"name:{name},age:{age} ")

    return name,age


info("小明",10)  #结果打印出 name:小明,age:10 
	```
2. 默认参数
	- 你可以为参数设置默认值。如果调用时没有提供该参数，则使用默认值。这能大大简化函数的调用。
	- **大师提示：** 永远不要使用**可变对象**（如列表 `[]` 或字典 `{}`）作为默认参数，否则会产生意想不到的“记忆效应”Bug。
	- **默认参数必须指向不变对象！**
	```python
	def power(x,n=2):

    return x**n

  

print(power(2))

print(power(2,3))

print(power(2,5))
	```
![500](assets/Day2%20函数/file-20260126144920909.png)
3. 可变参数
	- 用于接收**不确定数量**的参数，是实现灵活函数的核心：
	- `*args`：可变位置参数，接收任意数量的**位置参数**，打包成元组；
	- `**kwargs`：可变关键字参数，接收任意数量的**关键字参数**，打包成字典；
	- 注意：`args` 和 `kwargs` 是约定俗成的命名，可换名，但不推荐。
![500](assets/Day2%20函数/file-20260126144003632.png)
4. 关键字参数
	- **关键字参数发生在函数调用的过程中**
	- 优点是不需要记住参数的顺序了
![500](assets/Day2%20函数/file-20260126144446133.png)
![500](assets/Day2%20函数/file-20260126152605391.png)
![500](assets/Day2%20函数/file-20260126150550184.png)
![500](assets/Day2%20函数/file-20260126145124144.png)
	
	```python
	def enroll(name, age, city="北京"):
    print(f"姓名:{name}, 年龄:{age}, 城市:{city}")

	enroll(age=20, name="张三") # 函数开始调用，而且顺序是和定义的时候不一样的，但是用了关键字参数就没关系
	```
	
5. 命名关键词参数
	- 如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收`city`和`job`作为关键字参数。这种方式定义的函数如下：
```python
def person(name, age, *, city, job):
    print(name, age, city, job)

```


![500](assets/Day2%20函数/file-20260126152915642.png)
![500](assets/Day2%20函数/file-20260126152937853.png)
