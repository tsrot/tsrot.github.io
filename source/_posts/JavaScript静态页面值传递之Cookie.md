title: JavaScript静态页面值传递之Cookie
date: 2016-03-04 14:05:48
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
Cookie是浏览器存储少量命名数据.
它与某个特定的网页或网站关联在一起.
Cookie用来给浏览器提供内存,
以便脚本和服务器程序可以在一个页面中使用另一个页面的输入数据.
<span style="color:red">注：此案例请放在服务器环境测试.</span>

post.html
``` bash
<input type="text" name="txt1">
<input type="button" onclick="setCookie('txt1',document.all.txt1.value);" value="Post">

<script language="javascript" >
function setCookie(name,value)
{
/*
*--------------- setCookie(name,value) -----------------
* setCookie(name,value)
* 功能:设置得变量name的值
* 参数:name,字符串;value,字符串.
* 实例:setCookie('username','baobao')
*--------------- setCookie(name,value) -----------------
*/
   var Days = 30; //此 cookie 将被保存 30 天
　　var exp　= new Date();
   exp.setTime(exp.getTime() + Days*24*60*60*1000);
   document.cookie = name + "="+ escape (value) + ";expires=" + exp.toGMTString();
   location.href = "read.html"; //接收页面.
}
</script>
```
read.html
``` bash
<script language="javascript" >
function getCookie(name)
{
/*
*--------------- getCookie(name) -----------------
* getCookie(name)
* 功能:取得变量name的值
* 参数:name,字符串.
* 实例:alert(getCookie("baobao"));
*--------------- getCookie(name) -----------------
*/
   var arr = document.cookie.match(new RegExp("(^| )"+name+"=([^;]*)(;|$)"));
   if(arr !=null) return unescape(arr[2]); return null;
}
alert(getCookie("txt1"));
</script>
```
<span style="color:red">优点:可以在同源内的任意网页内访问.生命期可以设置.</span>
<span style="color:red">缺点:值长度有限制.</span>