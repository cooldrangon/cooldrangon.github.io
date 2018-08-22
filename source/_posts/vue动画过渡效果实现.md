---
title: vue动画过渡效果实现
date: 2018-08-17 12:14:23
tags:
---
1.首先在父路由中添加transition标签 并给其添加动态的:name属性 然后用transition标签包裹router-view标签 然后在data中return出来动态:name 在style中写 name-enter,name-enter-to,name-enter-active的样式 分别代表刚进入 元素插入前生效的状态 和 在整个过渡效果的状态 和离开前一帧状态
<!-- more -->
但是如果没有定位的话 两个组件间出现过渡时会出现一个把另一个挤走或变形的状态 所以可以在两个active中添加定位属性
2.同时生效的进入和离开的过度不能满足所有的要求 所以vue提供了过渡模式
  in-out:新元素先进行过渡 完成之后当前元素过渡离开
  out-in:当前元素先进行过渡 完成之后新元素过渡进入
  示例代码如下 此示例是两个路由组件之间切换的过渡
  ```
<template>
 <div>
  <ArticleNav></ArticleNav>
   //首先 给 router-view 套上transition标签 router-view代表了所有的子路由组件渲染的结果
   //在给其起一个动态名字
  <div class="container">
    <transition :name="slide">
      <router-view></router-view>
    </transition>
  </div>
 </div>
</template>

<script>
import ArticleNav from './ArticleNav'
export default {
  components:{
    ArticleNav
  },
//返回一个动态名字
  data(){
    return{
      slide:'slide-right'
    }
  }
}
</script>

<style>
  .container{
    overflow-x:hidden;
    position:relative;
    min-height:100vh;
  }
  给其加过渡效果
  .slide-right-enter{
    transform:translate3d(100%, 0, 0);
  }
  .slide-right-enter-to{
    transform:translate3d(0, 0, 0);
  }
  .slide-right-leave{
    transform:translate3d(0, 0, 0);
  }
  .slide-right-leave-to{
    transform:translate3d(-50%, 0, 0);
  }
  .slide-right-enter-active, .slide-right-leave-active{
    background: #fff;
    transition: transform .5s;
    min-height:100vh;
    position:absolute;
    top: 0;
    left: 15px;
    right: 15px;
  }
</style>

  ```
  (https://www.jianshu.com/p/b03a8d7b1719){target="_blank"}