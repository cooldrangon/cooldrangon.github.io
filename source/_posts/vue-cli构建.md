---
title: vue-cli的构建
date: 2018-08-15 15:02:05
tags: vue
---
1.刚开始的时候 在 命令行输入 cnpm install -g vue-cli 然后会得到vue命令 可以用vue list 展示所有的模板

vue init 模板 目录 模板是使用什么模板 目录就是项目目录 如果有项目文件了 就不用写 例：vue init webpack demo 会创建一个demo文件夹 一路回车 但是到ESlint 要 no 最后会有三个选项 选最后一个
<!-- more -->

cd 创建的文件名

cnpm install el：“#app” 和 template如果在一起 后者覆盖前者
npm start 会生成一个 8080

export default 导出组件配置对象
import 模块名 from ‘路径’

总结就是如果想把组件放进父组件中 就从本文件中导出 在父组件文件中导入 并写标签 因为创建vue文件的时候 自然生成了export default配置对象 在里面写的任何东西都可以在别的vue文件中接收到

所有的东西都是模块化引入 不管是 js 还是别的需要的文件 都可以在 cnpm 下载完成模块后引入
当创建完成后在src文件中会有三种文件 这三种文件各自有各自的作用

1.是src 文件下的components文件 在这个文件中是存放vue组件的文件这个文件中存放了所有的 建立项目所需要的组件 的模板 和函数 数据 并最终把他们全部导出，
2.是src文件下的 router文件中的index.js 在这个文件中接收了components文件中所有的组件导出的内容并把他们在其中的routes中与路由绑定在一起 最后导出

3.是 src文件下的 index.js 这个文件主要的作用就是引入在命令窗口下载项目所需要的模块 同时还引入了index.js导出的所有组件和路由配置 如果是专门为vue制作的模块在import引入后还需要用 vue.use(模块名) 声明下 最后就是创建了new vue组件对象把app这个最大的组件放进去 同时把从 index.js组件中导出的组件和路由配置的文件放进去（就一句话而已 呜呜 看了半天才理清楚关系）

最后总结 用cli就是玩模块 把需要的模块导入 然后用 把需要导出的模块导出就可以