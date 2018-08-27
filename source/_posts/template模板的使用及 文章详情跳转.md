---
title: template模板的使用及文章详情跳转
date: 2018-08-27 16:45:55
tags: 微信小程序
---
wx:for只能在当前页面循环当前页面的数据 用template可以解决
template是一个模版 创建一个模版文件夹 在里面创建html css文件就可以了
在template文件的html中创建template标签 添加name属性 把模版编写好
想要在别的文件中用这个模版 <import src="相对路径.wxml" />

想要在那个地方使用的话 用 block标签包裹<template is ="template name" data="{{block标签中的循环item}}" /> 如果在item前加上... 就可以去掉模板文件中的item了
想要引入 template中的样式 在想要引用的 文件的.wxss中@import "相对路径"

如果想要在template中绑定事件 不能直接在template中绑定 可以在template外面包一个view标签 把事件绑定到view标签上
可以用自定义属性 data-postid = "{{item.id}}"
凡是前面加data的属性就是自定义属性 当点击时吧id传过去
 点击事件上:function(event){
    event 代表了系统给的外面的一层数据
    var 变量名 = event.currentTarget.dataset.postId 
    currentTarget 表示点击的组件
    dataset表示前缀有data的全部的自定义属性的值
    postid就代表了设置的想要的
 }

 文章详情页面 根据文章的id来跳转到不同的文章
   如果一个页面的子页面最好写在父页面目录下
   
