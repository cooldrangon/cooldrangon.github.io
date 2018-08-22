---
title: vuex模块总结
date: 2018-08-21 11:55:32
tags:
---
vuex模块总结 模块化的写法
<!--more-->
模块创建
new Vuex.Store({
  modules:{
    模块名:{

    }
  }
})

分开写
const module = {}
new Vuex.Store({
  modules:{
    模块名:module
  }
})

每个模块中都可以写
state getters mutations actions modules


 vuex 模块创建详情
  1.依然是普通的vue-cli创建 创建完成后需要什么插件就下载什么 如 axios vuex必下
  2.在src 下创建一个store文件夹 在里面创建index.js文件 当然 名称可以自定义 想起什么名字起什么名字 无所谓 只要导入导出正确就行
  3.在store文件夹下的index.js文件内引入 vue vuex  axios  如下所示 在Vuex.Store中填入state getters mutations actions 并导出store 在main.js中接收
  4.在store文件夹下的index.js中先写上axios.defaults.baseURL = 'http://localhost:8000/api'
  5.大概流程是这样的 在list里面有一个methods里面的函数里面用dispatch调用了actions里面的函数 并通过watch监听id的变化 把id传了过去 在index.js里的actions中写上axios请求的函数函数中得有两个参数第一个是{{commit}}是为了调用mutations里的函数 第二个参数是 id 是手动修改的 在list里有监听函数 如果修改了 就把最新的id 传进去 发送axios 请求 当接收到数据后 通过commit调用 第一个参数是 mutation中的函数名 第二个参数是 载荷就是传过去的数据 当把接收到的数据传过去state后在list中有...mapState(['数据名'])相当于把数据提到了这里
  ```
  import Vue from 'vue'
import Vuex from 'vuex'
import axios from 'axios'

axios.defaults.baseURL = 'http://localhost:8080/api'
Vue.use(Vuex)

const state = {
  articles: []
}

const getters = {

}

const mutations = {
  setArticles (state, articles) {
    state.articles = articles
  }
}

const actions = {
  /* getArticles ({commit}, id) {
    axios.get(`/articles/${id}/page/1`).then(res => {
      // console.log(res)
      commit('setArticles', res.data.res.articles)
    })
  } */
  这里是同步的写法
  async getArticles ({commit}, id) {
    const res = await axios.get(`/articles/${id}/page/1`)
    commit('setArticles', res.data.res.articles)
  }
}


const store = new Vuex.Store({
  state, getters, mutations, actions  
})

export default store
  ```
  vuex就是数据在state中存储 在getters中修改数据并返回新的数据 在mutations中直接修改数据 actions是异步的想要修改数据也得通过mutations修改

  单个模块当做一个store即可
 state
  this.$store.state.模块名
 getters actions mutations 直接会到全局的store中
 如果给模块加了 namespaced:true 那么对应的 getters actions mutations会变成 模块名/名字 的形式
  同时,在模块中的action中直接commit('mutation') dispatch('action') 指的是模块中的mutation和action
  如果想要在模块中调用全局的action mutation 需要使用 commit('mutation',payload,{root:true})
  dispatch('mutation',payload,{root:true})
  辅助函数如果要使用具有命名空间模块中的内容
  mapState({
    msg:state => state.模块名.msg
  })
  mapState('模块名',['msg'])