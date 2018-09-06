---
title: tab选项卡
date: 2018-08-28 16:02:11
tags: 微信小程序
---
微信小程序文档在框架中有介绍
tabBar只能配置最少两个最多五个 
如果想让哪个文件出现tabBar必须把这个文件放在tabBar中的list数组下
   以上说的都是微信小程序中给的tabBar这个配置的框架 
下面说一说 自定义的tab选项功能的实现
  tab选项功能实现 
    首先要把样式写出来 让后在heml标签上面加上自定义的id data-id 加上点击事件 当点击的时候获取 属性中的id值 但是要用e.currentTarget.id比较稳定
     e.target.id e.target 代表的是，实际触发了点击事件的组件。 
     e.currentTarget 代表的是，注册了监听点击事件的组件。
    然后在js中把点击的当前的id值给data wxml页面通过获取data中的属性的值判断添加类名 实现点击的样式的切换 在在下面写上几个页面 同时也获取data中的状态实现隐藏显示
      具体代码示例如下所示
       ```
       wxml的代码
         <view class='backbone-tab'>
            <text class='{{tabArr.currentId==1?"active":""}}' id='1' data-id='1' bindtap='tab'>笔记</text>
            <text class='{{tabArr.currentId==2?"active":""}}' id='2' data-id='2' bindtap='tab'>关于我</text>
            <text class='{{tabArr.currentId==3?"active":""}}' id='3' data-id='3' bindtap='tab'>收藏</text>
            <text class='{{tabArr.currentId==4?"active":""}}' id='4' data-id='4' bindtap='tab'>活动</text>
            </view>
            <view class='corresponding'>
            <view class='item {{tabArr.currentBdid==1?"active":""}}'>1</view>
            <view class='item {{tabArr.currentBdid==2?"active":""}}'>2</view>
            <view class='item {{tabArr.currentBdid==3?"active":""}}'>3</view>
            <view class='item {{tabArr.currentBdid==4?"active":""}}'>4</view>
        </view>
        wxss的代码
           .backbone-tab{
            height: 147rpx;
            width: 100%;
            display: flex;
            justify-content: space-evenly;
            align-items: center;
            border-bottom: 1px solid #eee; 
            }
            .backbone-tab text{
            font-size: 25rpx;
            border: 2px solid transparent;
            }
            .backbone-tab .active{
            border-bottom: 2px solid #ffc037;
            }
            .corresponding .item{
            display: none;
            }
            .corresponding .item.active{
            display: block;
            }
        js代码
          data: {
            tabArr:{
            currentId:0,
            currentBdid:0
            },
            oneselfurl:"../../assets/tabs/add.png"
        },
        tab:function(e){
            var dataid=e.currentTarget.id
            var obj = {}
            obj.currentId = dataid
            obj.currentBdid = dataid
            this.setData({
            tabArr : obj
            })
        },

  