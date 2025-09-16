---
title: 使用jsdom解决用PyExecJS执行含有document、window等对象的js代码时出现的问题
date: 2020-09-17 10:40:17
---

## 解决dom依赖

首先，全局安装jsdom，并获取安装路径。

```bash
npm i jsdom -g
npm root -g
```

其次，在要执行的js代码的最前面加上如下代码。

```js
const jsdom = require("jsdom");
const { JSDOM } = jsdom;
const dom = new JSDOM(`<!DOCTYPE html><p>Hello world</p>`);
window = dom.window;
document = window.document;
XMLHttpRequest = window.XMLHttpRequest;
```

最后，指定jsom路径并执行js代码。

```python
js = execjs.compile(js_text,cwd=r'C:\Users\w001\AppData\Roaming\npm\node_modules')
result = js.call(function_name,arg_1,arg_2,...,arg_n)
```

##  解决atob依赖

首先，安装atob依赖。

```bash
npm i atob -g
```

其次，在要执行的js代码最前面加上如下代码。

```js
const atob = require('atob');
```

最后，执行js代码即可

参考：

https://www.cnblogs.com/huchong/p/11044238.html

https://segmentfault.com/q/1010000015660773