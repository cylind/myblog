---
title: 循环语句中的break、continue和else
abbrlink: 58629
date: 2020-04-16 20:46:46
tags: Python
---

python中循环语句有两大类 ，for循环和while循环。for循环常用于循环次数已知的情况，而且常常和range函数搭配使用；而while循环常用于循环次数未知的情况，利用条件判断来进行循环，只要条件为真则进行下去。

else、break和continue是常常搭配循环语句一起使用的，其中break和continue常常和if语句构成一个判断来改变循环的执行情况；else则是当循环体正常结束（break没有发生）时，提供一个最终的判断执行，通俗地说就是“直到……才”。

<!-- more -->

## break

用于跳出for循环或while循环。对于多重循环情况，跳出最近的那重循环。

例，判断1~n内的所有素数。

```python
n = eval(input())
for i in range(2,n+1):
    for j in range(2,int(i**0.5)+1):
        if i%j==0:
            break
    else:
        print(i)
```

## continue

用于结束本次循环并开始下一次循环。对于多重循环情况，作用于最近的那重循环。

## else

在for循环和while循环后面可以跟着else分支，当for循环<mark>已经遍历完列表中所有元素</mark>或while循环的条件为False时，就会执行else分支。

例，输出100以内的素数。

```python
for n in range(2,101): #n在2~100之间取值
    m=int(n**0.5) #m等于根号n取整
    i=2
    while i<=m:
        if n%i==0: #如果n能够被i整除
            break #跳出while循环
        i+=1
    if i>m: #如果i>m，则说明对于i从2到m上的取值、都不能整除n，所以n是素数
        print(n,end=' ') #输出n
```

for循环的例子在break那部分已经给出一个了，实际上，判断素数这个用for循环会更简洁，不用自己在判断一次是否已经遍历了所有元素。