title: html2json与json2html的实现
date: 2016-03-22 13:15:03
tags:
  - html2json
  - json2html
categories:
  - 转载博客
---
<Excerpt in index | 首页摘要>
在这满天由用户创造内容的时代，各个地方穿插着可以让用户输入的入口。如：写文章，写博客等。但由于用户输入的内容完全是不安全的，所以在存储和展现的时候需要对内容进行过滤，避免用户内容产生XSS和钓鱼等各种危害行为。html2json:一种新的富文本数据传输方案。（转载前端大神welefen的[html2json&json2html的实现](http://www.welefen.com/post/html2json-and-json2html-2.html)）
<!-- more -->
<The rest of contents | 余下全文>

### 前端实现：html2json
前端实现本来准备使用已经有的html2json库，但具体看了下代码。发现这个库是基于词法分析来做的，并且依赖[htmlparser.js](http://ejohn.org/files/htmlparser.js)这库。一方面JS代码量太大，另一方面执行起来也有性能问题。

于是解决浏览器的DOM解析和构建接口，几十行代码就搞定了html2json的过程。

实现原理其实很简单：
> * 创建一个div
> * 将text内容innerHTML到div
> * 获取div的childNodes
> * 遍历节点，并通过node.attributes属性获取到属性名和值
> * 在递归获取字节点的属性等
> * 将对象转为JSON

具体代码见：
https://github.com/welefen/html2json/blob/master/src/html2json.js

整个代码也就100行左右。

不过IE6下通过node.attributes获取属性名和属性值会有一些问题，它会打印出该元素的书有属性，而不是仅仅是显性属性。目前解决整个问题的方案是通过配置属性白名单进行的，并且对于style属性，需要通过node.getAttribute('style').cssText进行。

使用方式：

``` bash
var jsonResult = html2json(text);
```
JSON数据格式
``` bash
[
    {
        "text": "welefen",
        "tag": "div",
        "attr": {
            "id": "welefen",
            "name": "suredy"
        },
        "child": [
            {
                "text": "child",
                "tag": "span",
                "attr": {},
                "child": []
            },
            {}
        ]
    },
    {
        "text": "suredy",
        "tag": "p",
        "attr": {
            "style": "font-size: 12px",
            "id": "test1"
        },
        "child": []
    },
    {}
]
```
包含text,tag,attr,child等几部分。

### 后端实现：json2html
后端需要将接收到的json转为html，如果不考虑安全因素的话是非常简单的，只要进行简单的拼接就可以了。由于考虑各种安全问题，所以相对要复杂点，但对象于传统的通过词法分析然后过滤要简单的多了。

需要过滤或者数据校验的数据有：

> * 需要对tag进行过滤，配置白名单，不在白名单内的tag直接过滤掉
> * tag属性过滤，对不支持的tag属性直接过滤掉，不管属性值是什么。
> * 对tag属性进行过滤，如对src值的过滤，href值的过滤。
> * 对style的属性值进行分析，设置css属性白名单，设置css属性值的校验规则。
> * 添加有些标签必须有某些属性规则，如：embed标签必须有type="application/x-shockwave-flash"，并且allowscriptaccess的值需要确定
> * 添加有些标签的子元素必须有哪些属性，如：object标签的子标签param标签，必须要有allowscriptaccess属性，并且属性值是一个配置的特定值。

具体代码见:
https://github.com/welefen/html2json/blob/master/src/json2html.class.php

由于每个使用场景的白名单不一样，所以需要进行很多配置。
