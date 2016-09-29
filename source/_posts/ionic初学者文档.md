title: ionic初学者文档
date: 2016-07-17 13:07:06
tags:
  - 移动端
  - ionic
categories:
  - JavaScript框架
---
<Excerpt in index | 首页摘要>
本以为按照官网的getting started 就能很快的生成一个WEB APP，看来我是想多了，里
面的坑只有经历过的人才知道，这真是坑死人不偿命啊！本着极客精神，经过一天时间，终
于把getting started 跑通了，接下来就简单多了。（注：1、先安装翻墙软件吧，否则过程好
久（推荐xx-net，github 上自己搜）。2、没有极客精神的人慎入）
<!-- more -->
<The rest of contents | 余下全文>
下面我简单介绍一下我的入坑经历：
1、这步还算顺利（前提是你安装了nodeJS，过程可能有点慢，看网络环境和人品了。如果
长时间停留在某个地方，Ctrl+C 暂停它，再运行一次）
```bash
$ npm install -g cordova ionic
```
2、这步也还算顺利，就是有点慢。
```bash
$ ionic start myApp tabs
```
如果你仅仅作为一个前端开发者，走到这步就可以在browser 运行起来了，但作为一个极
客，怎能止于这步。
```bash
$ cd myApp
$ ionic serve
```
3、接下来就进入大坑了
```bash
$ cd myApp
$ ionic platform add android //这里是个大坑，先安装Android SDK
$ ionic build android $ ionic emulate android
```
4、接下来就是漫长且苦逼的填坑经历了。
...........................................................................
### 接下来看我的github的吧，懒得写了！！！
github地址：[ionic初学者文档](https://github.com/tsrot/ionic-environment-configuration)
或者你可以直接clone
```bash
git clone https://github.com/tsrot/ionic-environment-configuration.git
```
