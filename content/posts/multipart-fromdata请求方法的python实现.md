---
title: multipart_fromdata请求方法的python实现
date: 2021-02-24 02:31:10
tags: Python
---

遇见这种请求方式好几次了，都是现查现用，原理啥的都不了解，用了就忘。以前觉得键值对的json/data(application/x-www-form-urlencoded)就够了，现在看来这玩意儿还挺重要的，应用挺广泛，有必要记录学习一下了。

<!-- more -->

multipart_fromdata将要传输的内容分为多个部分，各部分内容用一个统一的分隔符标识开始（这个分隔符可以是任意内容，只要能起唯一标识的分隔作用即可），直到遇到下一部分内容的分隔符，而最后一部分内容的分隔符需要加上`--`以示结束。各部分内容都有许多参数，有的是自动生成，有的是固定用法（规定要这么写），其中最主要的name和value，相当于传统的www-form中的键值对 `{name : value }` 

具体形式如下：

```http
-----------------------------Separator-character
Content-Disposition: form-data; name="part1"

value1
-----------------------------Separator-character
Content-Disposition: form-data; name="part2"

value2
-----------------------------Separator-character
Content-Disposition: form-data; name="the-last-part"

value3
-----------------------------Separator-character--
```

基于实用原则，这里只给出最简单的实现方法，具体原理不细揪，multipart_fromdata的格式直接用现成的python第三方库requests_toolbelt来实现。话不多说，直接上两个例子。

## 案例一：智慧树

智慧树个人头像更换时，上传图片就是用的multipart/fromdata。通过浏览器抓包，可以看到其发送的内容。

首先，查看其请求头部分：

![](https://cdn.jsdelivr.net/gh/ghcdn/img/20210305234017.png)

从上图可看出，这个请求使用multipart_fromdata编码格式来传输内容，且各部分内容间的分隔符为`-----------------------------15883549271175967036310302373`



其次，查看请求部分的payload数据：

![image-20210305232720034](https://cdn.jsdelivr.net/gh/ghcdn/img/20210305233128.png)
![](https://cdn.jsdelivr.net/gh/ghcdn/img/20210305233234.png)

从中可以看出，其payload的内容就只有一个，即imgFile，只包含二进制图片文件。其中，Content-Disposition、Content-Type和filename根据被打开的文件确定，name对应的imgFile则是这个参数的名称。

具体python实现代码如下：

```python
import requests
from requests_toolbelt import MultipartEncoder

def upload(file):
    url = 'https://base1.zhihuishu.com/able-commons/cut/imgupload'
    payload = {'imgFile': (file, open(file, 'rb'), 'image/png')}
    m = MultipartEncoder(payload)
    headers = {'Content-Type': m.content_type}
    r = requests.post(url, headers=headers, data=m)
    print(r.json())


if __name__ == '__main__':
    upload('xxx.png')
```

## 案例二：超星云盘

超星云盘网页版上传文件也是用的multipart_fromdata编码格式。

同样地，先看其请求头：

![](https://cdn.jsdelivr.net/gh/ghcdn/img/20210305235820.png)

可以看出，其使用multipart_fromdata编码格式来传输内容，且各部分内容间的分隔符为`---------------------------1682890927123165149599605516` 

再看其请求部分的payload：

![](https://cdn.jsdelivr.net/gh/ghcdn/img/20210306000414.png)

![](https://cdn.jsdelivr.net/gh/ghcdn/img/20210306000533.png)

可以看出，其包含多个参数folderId，puid，id，name，type，size等等，最后一个参数是上传的二进制图片。

具体python实现代码如下：

```python
def upload(filePath, name, folderId):
    payload = {'folderId': folderId, 'puid': uid, 'id': 'WU_FILE_2', 'name': name, 'type': 'image/png', 'lastModifiedDate': '', 'size': '', 'file': (name, open(filePath, 'rb'), 'image/png')}
    cookies = {} # 需要添加cookies认证
    m = MultipartEncoder(payload)
    header = {'Content-Type': m.content_type}
    p = requests.post('https://pan-yz.chaoxing.com/opt/upload', data=m,
                      cookies=cookies, headers=header)
    print(name, p.json()['msg'])
    return p.json()['msg']
```

## 参考

https://stackoverflow.com/questions/4526273/what-does-enctype-multipart-form-data-mean

https://stackoverflow.com/questions/8659808/how-does-http-file-upload-work

https://pypi.org/project/requests-toolbelt/