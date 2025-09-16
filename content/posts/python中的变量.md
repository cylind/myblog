---
title: python中的变量
tags: [Python]
date: 2020-04-21 13:27:46
---

python中分好多变量，全局变量、局部变量、静态变量（类变量）、实例变量，整得人有点懵，今天就腾出时间给它来一次总结。

<!-- more -->

## 定义

1、全局变量：在模块内、在所有函数外面、在class外面

2、局部变量：在函数内、在class的方法内（未加self修饰的)  

3、静态变量：在class内的，但不在class的方法内的 （也叫类变量）

4、实例变量：在class的方法内的，用self修饰的变量

python是面向对象编程的语言，在python里一切皆对象，从这个角度来看，上述变量都是不同对象的属性

a、全局变量：属于模块的属性

b、局部变量：属于方法或函数的属性

c、 类变量：属于类的属性

d、 实例变量：属于对象的属性

## 简介

下面开始用实际例子来说明一下全局变量和局部变量、静态变量和实例变量以及关键字global和nonlocal的使用。

### 全局变量与局部变量

全局变量与局部变量的作用域不同，全局变量在整个模块（也就是整个py文件）内起作用，而局部变量只在某个具体的函数内起作用。

全局变量与局部变量由于作用域不同，二者并无直接关系，即使是使用相同变量的变量名，这两个变量仍然是属于不同的变量。

在函数内获取并修改全局变量时，要使用global声明，如果仅仅只是获取全局变量，可以不进行global声明，但是规范上是建议大家这么做的。

```python
x = 1
def foo():
    global x # 如果不声明global，会报错
    x = x + 2 # 获取并修改全局变量x
    print(id(x),x)
    
print(id(x),x)
foo()
print(id(x),x)

#140703138079376 1
#140703138079440 3
#140703138079440 3
```

```python
x = 1
def foo():
    y = x + 2 #仅获取全局变量x
    print(y)
foo() #输出为3
```

```python
x = 1
def foo():
    print(id(x),x) #仅获取全局变量x

print(id(x),x)
foo()
print(id(x),x)

#140703138079376 1
#140703138079376 1
#140703138079376 1
```

如果在未声明global的前提下，在函数内创建一个与全局变量同名的变量，创建的变量默认为局部变量。

```python
x = 1
def foo():
    x = 2
    print(id(x),x) #仅获取全局变量x

print(id(x),x)
foo()
print(id(x),x)

#140703138079376 1
#140703138079408 2
#140703138079376 1
```

### global与nonlocal

global上面已经讲到过了，其作用域是整个模块，这里主要讲一下nonlocal的用法。

nonlocal，顾名思义，就是非局部的意思，它是用在嵌套函数的内层函数里的，否则会报错，它的意义跟global几乎一样，用来声明变量为上一层函数的变量（注意，是上一层），若果要获取并修改上一层的变量，必须要进行nonlocal声明，只是获取上层变量的话，可以不进行nonlocal声明（实际上不声明的话，默认就是获取上一层的变量）。

同样的，由于不同层的变量作用域不同，它们之间并无直接关系，即使是使用相同变量的变量名，这些变量仍然是属于不同的变量。如果在未声明nonlocal的前提下，在函数内创建一个与全局变量同名的变量，创建的变量默认为局部变量。（由于和global用法一样，所以直接抄global部分的说明。。。）

```python
#不进行nonlocal声明情况下，函数内创建的变量默认就是作用域为该函数范围内的新变量
x = 1
def outer():
    x = "outer"
    print(id(x),x)
    def inner():     #如果直接修改x = x + "sometext",则报错
        x = "inner"  #如果把x="inner"注释掉，则inner()获取上一层的x="outer"
        print(id(x),x)
        def mostinner():
            x = "mostinner"
            print(id(x),x)
        mostinner()
    inner()

print(id(x),x)
outer()
print(id(x),x)

#140703138079376 1
#1589698876448 outer
#1589690252624 inner
#1589698638192 mostinner
#140703138079376 1
```

```python
# 进行nonlocal声明后，可以获取修改上层变量
x = 1
def outer():
    x = "outer"
    print(id(x),x)
    def inner():
        nonlocal x
        x = x +'_inner'
        print(id(x),x)
        def mostinner():
            x = "mostinner"
            print(id(x),x)
        mostinner()
    inner()
    print(id(x),x)

print(id(x),x)
outer()
print(id(x),x)

#140703138079376 1
#2570221048472 outer
#2570220811056 outer_inner
#2570220760688 mostinner
#2570220811056 outer_inner
#140703138079376 1
```

总结一下，global和nonlocal的用法是一样的，global用于声明全局变量，nonlocal用于声明上一层函数的变量；不加声明的话，则默认获取全局或上层变量（如果没有，则创建作用于本函数的局部变量）；不加声明修改则报错。

### 静态变量与实例变量

* 类变量： 类变量就是定义在类中，但是在函数体之外的变量。通常不使用`self.变量名`赋值的变量。类变量通常不作为类的实例变量的，类变量对于所有实例化的对象中是公用的。

* 实例变量： 实例变量是定义在方法中的变量，使用`self`绑定到实例上的变量，只是对当前实例起作用。

```python
class foo:
    all = 0   
    def add(self):
        foo.all += 1
    
i1 = foo() #实例化对象
i2 = foo()
print(i1.all,i2.all,foo.all) #0 0 0

i1.add()
print(i1.all,i2.all,foo.all) #1 1 1 

i2.add()
print(i1.all,i2.all,foo.all) #2 2 2 

foo().add()
print(i1.all,i2.all,foo.all) #3 3 3
```

未完待续。。。

参考：

https://blog.csdn.net/cadi2011/article/details/52457754

https://www.jianshu.com/p/9fefb52ca91b

https://www.imooc.com/article/14652


