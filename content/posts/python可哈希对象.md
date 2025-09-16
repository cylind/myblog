---
title: python可哈希对象
tags: Python
abbrlink: 36300
date: 2020-04-16 20:45:38
---

一个对象在其生命周期内，如果保持不变，就是hashable（可哈希的），常见的可哈希的数据类型有数值、字符串、列表；而内容可更改的，比如列表、字典、集合等则不是。

<!-- more -->

官方解释：

An object is hashable if it has a hash value which never changes during its lifetime (it needs a __hash__() method), and can be compared to other objects (it needs an __eq__() or __cmp__() method). Hashable objects which compare equal must have the same hash value.

Hashability makes an object usable as a dictionary key and a set member, because these data structures use the hash value internally.

All of Python’s immutable built-in objects are hashable, while no mutable containers (such as lists or dictionaries) are. Objects which are instances of user-defined classes are hashable by default; they all compare unequal, and their hash value is their id().
