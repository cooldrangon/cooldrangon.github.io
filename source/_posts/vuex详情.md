---
title: vuex详情
date: 2018-08-20 11:57:55
tags:
---
vuex中的几个属性的示例以及解析
vuex - state
在这个示例中主要通过 const store 创建了一个新的new Vuex.Store实例然后放在了 app中 所以app中的computed就可以用this.$store来调用里面的东西 所以在div中就可以的到数据 state就是专门用来保存各组件的数据的
  示例:
<!-- more -->
   ```
   <div id = "app">
     {{msg}}
     也可以写{{$store.state.msg}}
   </div>

    const store = new Vuex.Store({
      state:{
        msg:数据
      }
    })

    const app = new Vue({
      el:'#app',
      computed:{
        msg(){
          return this.$store.state.msg
        }
      }
    })
   ```

   2.vuex - getter的作用
     getter 的作用是对state进行处理,并得到新的数据
     ```
     getter 接受 state作为其第一个参数 目的方便获取state
     getter 接受 getters作为其第二个参数
      例如:getters:{
        watchedMovies(state,getters){
          return .....
        }
      }
      具体事例
      getters: {
        // getters和computed类似
        watchedMovies (state) {
          return state.movies.filter(movie => movie.watched)
          movies是state中的数组 在这里可以直接通过传参的形式调用 并获取里面的movies数据并通过过滤器返回里面的看过的电影的名字
        },
        noWatchMovies (state) {
          return state.movies.filter(movie => !movie.watched)
        },
        注意这里面的参数的形式
        watchedMoviesLength (state, getters) {
          return getters.watchedMovies.length
        },
        getMovieById (state) {
          console.log(2)
          return (id) => {
            return state.movies.filter(movie => movie.id === id)[0]
          }
        }
      }
      ```
     getter不止可以从state中
     获取新的数据 也可以从其他getter中获取新的数据
     getter在正常使用时,获取到的直接就是对应的数据。有些时候需要获取对应条件的数据  不管是state还是getters都只是用来存储数据
     在getter中是用函数的形式对state中的数据进行处理 
     computed 用法是像data一样直接使用 写法是像methods一样

     总结:const store = new Vuex.Store({})中两个属性 state是存储数据的 getters是对数据进行处理后返回新的数据的但是不管是不是处理后的数据都需要经过computed里面返回 方便调用

  vuex - mutation

    mutation中接受两个参数 state作为其一个参数 因为mutation就是改变state payload作为其第二个参数 载荷 尽量为对象
    mutation 需要commit触发 $store.commit()
    mutation想要使用,就在对应组件的methods触发的函数中,调用this.$store.commit('mutation名字',载荷)
    相当于自定义事件中的$emit 创建mutation的位置相当于自定义事件$on
    ```
    mutations:{
      //mutation就是修改state数据
      mutation名字(state,payload){
        //payload就是commit mutation时传递过来的数据
        //根据payload修改state
      }
    }
    ```
    在对应的组件中设置处罚函数,一般是原生事件函数
    methods:{
      原生事件函数名(){
        //触发mutation
        this.$store.commit('mutation名字',payload)
      }
    }
    在提交mutation时可以使用
      this.$store.commit('mutation名字')
    还可以使用对象形式的commit
      this.$store.commit({
        type: 'mutation名字',
        数据1:数据值,
        数据2:数据值
      })

      在接收到payload就包含了对象中所有的信息
      mutation就是用来直接设置state的（只能同步设置）

      vuex - actions
        所有的异步操作都应该放在action中
        action就是通过调用mutation里的函数实现异步加载
        action不能去修改state 所以最后的执行代码永远是commit（mutation）
        参数context就是个阉割版的store
        context中包含有
        commit //触发mutation
        dispatch
        getters
        state 不要再action中修改 只能获取其中的值
        action中也可以附带载荷,使用方法和mutation一致
        示例:
        ```
        const store = new Vuex.Store({
      state: {
        count: 1
      },
      mutations: {
        addCount (state) {
          state.count++
        }
      },
      actions: {
        addCountAsyncAction ({commit, state}) {
          // action不能去修改state 所以最后的执行代码永远是commit(mutation)
          /* 
            参数context就是个阉割版的store
            context中包含有
              commit
              dispatch
              getters
              state  不要在action中修改，只能获取其中的值
            action中也可以附带载荷，使用方法和mutation一致
          */

          // console.log(context)
          setTimeout(() => {
            // 调用commit触发mutation
            commit('addCount')
          }, 2000)
        }
      }
    })

    const app = new Vue({
      el: '#app',
      store,
      computed: {
        count () {
          return this.$store.state.count
        }
      },
      methods: {
        addCountAsync () { // 同步
          this.$store.commit('addCount') // 触发mutation
        },
        addCountSync () { // 异步
          this.$store.dispatch('addCountAsyncAction')          
        },
      }
    })
        ```

        




