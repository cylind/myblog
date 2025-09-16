---
title: 利用PyExecJS在python中执行js函数
date: 2020-09-17 10:29:12
---

1. 安装：`pip install PyExecJS`

2. 运行时，execjs会自动使用当前电脑上的运行时环境（建议用nodejs）

3. 简单使用

   ```python
   js = execjs.compile(js_text)
   result = js.call('function_name',arg_1,arg_2,...,arg_n)
   ```

参考：

https://blog.csdn.net/weixin_40539892/article/details/88982716

