title: 前端JavaScript分页
date: 2016-03-10 12:03:17
tags:
  - 前端分页
categories:
  - JavaScript
---
<Excerpt in index | 首页摘要>
如今的前端开发越来越考虑用户体验和性能优化。今天有点时间，研究了下前端分页和后端分页的实现，以及两种分页的性能和优劣势···
<!-- more -->
<The rest of contents | 余下全文>
### 前端分页

``` bash
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>分页 pagination</title>
</head>
<body>
	<ul class="pagination" style="display:none;">
		<li class="item">A . hello Tsrot</li>
		<li class="item">B . hello Tsrot</li>
		<li class="item">C . hello Tsrot</li>
		<li class="item">D . hello Tsrot</li>
		<li class="item">E . hello Tsrot</li>
		<li class="item">F . hello Tsrot</li>
		<li class="item">G . hello Tsrot</li>
		<li class="item">H . hello Tsrot</li>
		<li class="item">I . hello Tsrot</li>
		<li class="item">J . hello Tsrot</li>
		<li class="item">K . hello Tsrot</li>
		<li class="item">L . hello Tsrot</li>
		<li class="item">M . hello Tsrot</li>
		<li class="item">N . hello Tsrot</li>
		<li class="item">O . hello Tsrot</li>
		<li class="item">P . hello Tsrot</li>
		<li class="item">Q . hello Tsrot</li>
		<li class="item">R . hello Tsrot</li>
		<li class="item">S . hello Tsrot</li>
		<li class="item">T . hello Tsrot</li>
		<li class="item">U . hello Tsrot</li>
		<li class="item">V . hello Tsrot</li>
		<li class="item">W . hello Tsrot</li>
		<li class="item">X . hello Tsrot</li>
		<li class="item">Y . hello Tsrot</li>
		<li class="item">Z . hello Tsrot</li>
	</ul>
	<div class="page-list"></div>
<script src="http://apps.bdimg.com/libs/jquery/1.9.1/jquery.min.js"></script>
<script>
	$(function(){
		var pagination = $(".pagination");
		var page_item = $(".item");
		var len = page_item.length;  //获取总的数据条数
		var page_len = 5;  //每页显示几条
		var pages = Math.ceil(len/page_len);  //总的页数
		// 初始化
		var init = function(){
			for(var i=1;i<=pages;i++){
				pagination.show(); //控制数据加载页面闪烁
				// 分页页码，字符串拼接
				var html = '<a href="#" style="margin-right:5px;">第'+i+'页</a>';
				$(html).appendTo(".page-list");

				for(var j=0;j<len;j++){

					if(j>=page_len){
						$($(".item")[j]).hide();  //显示第一页
					}
				}
			}
		};

		init();  //执行初始化
		var page_list = $(".page-list a");
		page_list.click(function(){
			var index = $(this).index();
			// console.log(index);
			for(var k=0;k<len;k++){
				// 判断第 index 页，显示数据 index*page_len < k < (index+1)*page_len
				if(k>=index*page_len && k<(index+1)*page_len){
					$($(".item")[k]).show();
				}else{
					$($(".item")[k]).hide();
				}
			}
		})
	})
</script>
</body>
</html>
```
<span style="color:red">优点:方便快速（数据量较少的时候）.</span>
<span style="color:red">缺点:只适用于数据量较少的时候.</span>
### 后端分页
时间不够，改天再分享···