---
title: python函数中的参数传递
abbrlink: 15489
date: 2020-04-16 20:47:33
tags: Python
---

python函数中传入数值或者字符串等不可变类型时，在函数内修改传入参数的值，传入的参数的在原始值并不会跟着变，但是当你传入的是列表、字典等可变类型的数据时，在函数内修改他们的值，这些变量的原始值会跟着一块变。

<!-- more -->

```python
a = 3
b = [1, 2, 3]
def foo(x1,x2):
    print(id(x1),id(x2))
    x1 += 1
    x2.append(4)
    print(f"{id(x1)} {x1} \n{id(x1)} {x2}")

print(id(a),a)
print(id(b),b)
foo(a,b)
print(id(a),a)
print(id(b),b)

'''
140715638874832 3
1566903090312 [1, 2, 3]
140715638874832 1566903090312
140715638874864 4 
140715638874864 [1, 2, 3, 4]
140715638874832 3
1566903090312 [1, 2, 3, 4]
'''
```

python函数中的参数传入既不是传数值也不是传引用，而是直接传入name，也就是标识符。

参考：

https://www.zhihu.com/question/20591688

https://www.quora.com/Are-arguments-passed-by-value-or-by-reference-in-Python

http://effbot.org/zone/python-objects.htm