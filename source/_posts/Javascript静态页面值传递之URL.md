title: Javascript静态页面值传递之URL
date: 2016-03-03 14:45:44
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
### JavaScript能通过URL进行传值.把要传递的信息接在URL上.

post.html
``` bash
<form action="read.html" method="get">
  <input type="text" name="username">
  <input type="text" name="sex">
  <input type="submit" value="Post">
</form>
<script language="javascript" >
function Post(){
  //单个值 Read.html?username=baobao;
  //多全值 Read.html?username=baobao&sex=male;
  url = "Read.html?username="+escape(document.all.username.value);
  url += "&sex=" + escape(document.all.sex.value);
  location.href=url;}
</script>
```


read.html
``` bash
<script language="javascript" >
/*
*--------------- Read.html -----------------
* Request[key]
* 功能:取得URL字符串,Request("AAA")
* 参数:key,字符串.
* 实例:alert(Request["AAA"])
*--------------- Request.html -----------------
*/
var url=location.search;
var Request = new Object();
if(url.indexOf("?")!=-1){
  var str = url.substr(1)　//去掉?号
  strs = str.split("&");
  for(var i=0;i<strs.length;i++){
    Request[strs[i ].split("=")[0]]=unescape(strs[ i].split("=")[1]);
  }
}
console.log(Request["username"])
console.log(Request["sex"])
</script>


<!-- <script>
String.prototype.getQuery = function(name){
  var reg = new RegExp("(^|&)"+ name +"=([^&]*)(&|$)");
  var r = this.substr(this.indexOf("?")+1).match(reg);
  if (r!=null) return unescape(r[2]); return null;
}
var str ="blog.xieliqun.com/index.html?a=1&b=1&c=测试测试";
alert(str.getQuery("a"));
alert(str.getQuery("b"));
alert(str.getQuery("c"));
</script> -->
```
<span style="color:red">优点:取值方便.可以跨域.</span>
<span style="color:red">缺点:值长度有限制</span>

