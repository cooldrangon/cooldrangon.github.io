---
title: 用cli引接口从数据库渲染文件
date: 2018-08-15 15:56:32
tags: vue
---
1.首先在 visula studio code 中把git里的文件下载下来 （也可以自己写代码把网站数据爬下来） 
2.创建数据库 引入文件中的SQL文件 （注意字符集之类的格式） 
3.在文件中有 aritcle文件下的index文件里面有接口 
<!-- more -->
4.创建自己的vue-cli文件 并下载 axios vue-axios 并在main.js中引入 引入后用vue.use(vueaxios,axios)声明下 
5.要跨域 在创建的vue-cli文件下有个config文件里面的index.js文件
```
proxyTable{ 
  '/api': { 
  target: 'http://localhost:3000', //目标接口域名 
  changeOrigin: true, //是否跨域 
  pathRewrite: { 
  '^/api': '/api' //重写接口 
  } 
}, 
} 
```
这样配置跨域 配置完成后可以在index.html中简单的测试一下 
6.在main.js中设置axios.defaults.baseURL = ‘http://localhost:8080/api’ 这样可以默认请求的就是这个地址 
7.配置路由和组件配置组件跟路由的时候要分析好 结构 当组件模块中引入别的组件模块时注意局部注册
8.当请求数据时可以使用同步请求几个数据的方式来请求数据 例:
```
 methods: {
    getArticleDetail () {
      const id = this.$route.params.id
      // return this.axios.get(`/article/${id}`)
      this.axios.get(`/article/${id}`).then(res => res.data).then(data => {
        if (data.res_code === 200) {
          this.article = data.res
          // 再发起一个请求
          // return this.axios.get(`/articles/${data.res.type_id}/rand`)
          return this.axios.all([this.axios.get(`/articles/${data.res.type_id}/rand`), this.axios.get(`/type/${data.res.type_id}`)])
        }
      }).then(this.axios.spread(({data: articlesRand}, {data: type}) => {
        this.articles = articlesRand.res
        this.type = type.res
      }))

      this.axios.get(`/comments/${id}/page/1`).then(res => res.data).then(data => {
        this.comments = data.res
      })
    },
    
  },
  watch: {
    '$route': {
      handler: 'getArticleDetail',
      immediate: true
    }
  }
  ```
  9.当需要登陆验证时
    1.需要在main.js里面写一个
    ```
       axios.defaults.headers.common['Authorization'] = (()=>{
         return 'Bearer '//注意这里有个空格 + localStorage.getItem('token')
       })()
       ```
    2.创建登录组件并在组件中写上
    ```
      Login() {
      this.axios.post(`/user/${this.email}`,{password:this.password}).then((res)=>res.data).then(data=>{
        if(data.res_code===200){
          localStorage.setItem('user',JSON.stringify(data.res.user))
          localStorage.setItem('token',data.res.token)
          const path = this.$route.query.returnURL
          this.$router.push(path?path:'/')
        }else{
          alert(data.res_msg)
        }
      }
    }
    ```
    10.当主页面都渲染完成后在主页面的子组件内给需要的地方套上router-link标签 里面的 :to="{name:'路径名'，params:{id:article.id}}"
    然后在methods里面写axios请求

    再然后在底下写监听路由变化的 watch:{
      '$route':{
        handler:'刚才在methods中封装的axios函数名'
        immediate:true
      }
    }
    11.添加评论功能首先先把评论的组件建好 然后引到需要评论的地方 利用 axios传id渲染已经评论过的内容 想要添加评论内容可以用bus.js实现 想要删除也是点击删除按钮传id删除数据库中的对应的评论信息 然后再删除前端的

