---
title: template模板的使用及文章详情跳转 及数据的传递
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
    当触发点击事件时获取点击的这个列表的id
    var postid = event.currentTarget.dataset.postid
    var 变量名 = event.currentTarget.dataset.postId 
    currentTarget 表示点击的组件
    dataset表示前缀有data的全部的自定义属性的值
    postid就代表了设置的想要的自己定义的文章的id值
    (2).  wx.navigateTo({
        url:"跳转的相对路径/文件?id = postid"
      })
 }
  上面讲了点击获得当前文章列表的postid 怎么把postid传递到子页面中的js中 让子页面根据不同的列表id生成不同的文章详情
   首先 父页面传递的数据应该是(2)所示 因为是父页面跳到子页面所以用navigateTo
    在子页面接收数据时应该 在子页面的js中写 onload:function(option){
      var postid = option.id 因为在父页面传数据?后面写的是id 所以这里就是option.id
    }
 文章详情页面 根据文章的id来跳转到不同的文章
   如果一个页面的子页面最好写在父页面目录下

  文章详情页面点击收藏或取消收藏
    当前是使用storage 缓存 如果用户不手动清除缓存 缓存永久存在
    缓存的写法是wx.setStorage("key",{
      也可以不写对象
    })
  获取缓存的方法是 函数名:function(event){
    var 函数名 = wx.getStorageSync("key")
    Sync后缀是同步的写法
  }
  删除用removeStorage
  缓存的上限最大不能超过10MB
    如果出现了undefind的错误

  文章点击收藏图片动态切换
    可以用wx:if else 方法 
