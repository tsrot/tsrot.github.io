title: '关于angular的$emit,$broadcast,$on通信机制'
date: 2016-07-01 14:35:49
tags:
  - angular
categories:
  - JavaScript
---
<Excerpt in index | 首页摘要>
好久没写博客了，从今天开始恢复更新！angular中两个同级之间进行通信，很常见的一种方式就是使用$emit,$broadcast,$on进行event，data通信。但是它有些限制，下面就来介绍详细用法和我遇到的一些坑。
<!-- more -->
<The rest of contents | 余下全文>
### 首先介绍一下我一开始使用时遇到的坑
``` bash
<div ng-controller='parentCtrl'>
	<input type="text" ng-model="age">
	<hr>
	<div ng-controller='childCtrl1'>
		controller 1 <p style="color:red">{{name}}  --  {{age}}</p>
		<input type="text" ng-model="age">
	</div>
	<br>
	<div ng-controller='childCtrl2'>
		controller 2 <p style="color:green">{{name}}  --  {{age}}</p>
		<input type="text" ng-model="age">
	</div>
</div>
<script>
	var app = angular.module('myapp',[]);
	app.controller('parentCtrl', ['$scope', function($scope){
		$scope.name = 'hello parent controller';
		$scope.age = '23';
		$scope.$on('ageChange',function(event,data){
			console.log("收到childCtrl1冒泡过来的数据",event);
			$scope.age = data;
			$scope.$broadcast('ageChangeA',$scope.age);
		});
	}]);
	app.controller('childCtrl1', ['$scope', function($scope){
		$scope.name = 'hello child1 controller';
		$scope.age = '25';
		$scope.$watch('age',function(){
			console.log($scope.age);
		});
		$scope.$emit('ageChange',$scope.age);
	}]);
	app.controller('childCtrl2', ['$scope', function($scope){
		$scope.name = 'hello child2 controller';
		$scope.age = '30';
		$scope.$on('ageChangeA',function(event,data){
			$scope.age = data;
			console.log("收到parentCtrl广播过来的数据",event)
		})
	}]);
</script>
```
1，childCtrl1可以触发ageChange事件并向父级传递。
2，父级parentCtrl也可以监听到ageChange事件，然后向子级广播ageChangeA事件（现在问题来了）。
3，childCtrl2不能够监听到ageChangeA事件（这是为什么呢？）。
-> 收到childCtrl1冒泡过来的数据 Object {name: "ageChange", targetScope: a, defaultPrevented: false, currentScope: g}

### 正确使用并填坑
``` bash
<div ng-controller='parentCtrl'>
	<input type="text" ng-model="age">
	<hr>
	<div ng-controller='childCtrl1'>
		controller 1 <p style="color:red">{{name}}  --  {{age}}</p>
		<input type="text" ng-model="age" ng-change="change()">      //ng-change事件触发
	</div>
	<br>
	<div ng-controller='childCtrl2'>
		controller 2 <p style="color:green">{{name}}  --  {{age}}</p>
		<input type="text" ng-model="age">
	</div>
</div>
<script>
	var app = angular.module('myapp',[]);
	app.controller('parentCtrl', ['$scope', function($scope){
		$scope.name = 'hello parent controller';
		$scope.age = '23';
		$scope.$on('ageChange',function(event,data){
			console.log("收到childCtrl1冒泡过来的数据",event);
			$scope.age = data;
			$scope.$broadcast('ageChangeA',$scope.age);
		});
	}]);
	app.controller('childCtrl1', ['$scope', function($scope){
		$scope.name = 'hello child1 controller';
		$scope.age = '25';
		$scope.$watch('age',function(){
			console.log($scope.age);
		});
		$scope.change = function(){
			$scope.$emit('ageChange',$scope.age);
		}
	}]);
	app.controller('childCtrl2', ['$scope', function($scope){
		$scope.name = 'hello child2 controller';
		$scope.age = '30';
		$scope.$on('ageChangeA',function(event,data){
			$scope.age = data;
			console.log("收到parentCtrl广播过来的数据",event);
		})
	}]);
</script>
```
对比上面两段代码细心的人应该已经发现了问题的所在。
$emit，$broadcast其中一个必须要有事件触发，否则子级中的$on将不能识别该事件。
-> 收到childCtrl1冒泡过来的数据 Object {name: "ageChange", targetScope: a, defaultPrevented: false, currentScope: g}
-> 收到parentCtrl广播过来的数据 Object {name: "ageChangeA", targetScope: g, defaultPrevented: false, currentScope: a}
