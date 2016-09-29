title: JavaScript静态页面值传递之Window.open
date: 2016-03-06 12:47:52
tags:
  - 前端总结
  - 个人笔记
categories:
  - JavaScript
---
<Excerpt in index | 首页摘要>
现在PC配置越来越好，有些东西可以在前端做，并且也能够实现很好的性能。有些数据传递我们可以把它放到前端来操作（当然这些数据应该是无关安全性），所以今天我要在这分享关于前端页面值传递的方法之一···
<!-- more -->
<The rest of contents | 余下全文>
这两窗口之间存在着关系.父窗口parent.htm打开子窗口son.htm
子窗口可以通过window.opener指向父窗口.这样可以访问父窗口的对象.
<span style="color:red">注：此案例请放在服务器环境测试.</span>

parent.html
``` bash
<!DOCTYPE html>
<html>
<head>
	<title>parent页面</title>
</head>
<body>
<input type="text" name="maintext">
<input type="button" onclick="window.open('son.html');" value="Open">

</body>
</html>
```

son.html
``` bash
<!DOCTYPE html>
<html>
<head>
	<title>son页面</title>
</head>
<body>
<script type="text/javascript">
	var parentText = window.opener.document.all.maintext.value;
	console.log(parentText);

</script>
</body>
</html>
```

<span style="color:red">优点:取值方便.只要window.opener指向父窗口,就可以访问所有对象.不仅可以访问值,还可以访问父窗口的方法.值长度无限制.</span>
<span style="color:red">缺点:两窗口要存在着关系.就是利用window.open打开的窗口.不能跨域.</span>