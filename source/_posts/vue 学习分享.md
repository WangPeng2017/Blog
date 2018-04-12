---
title: vue学习分享
date: 2018-04-10 23:30:56
tags: vue
---

ps: 因为公司业务需求，最近学习了一段时间Vue+node，关于哪个前端框架更好，仁者见智，
这里列一下前端各框架性能对比 [Vue vs React / Vue vs AngularJs](https://cn.vuejs.org/v2/guide/comparison.html)
 
### 关于vue全家桶 vue-cli  + vue-router  +  vuex + axios
一、 脚手架新建项目 [vue-cli](https://cn.vuejs.org/v2/guide/installation.html)
```
# 全局安装 vue-cli
$ npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 安装依赖，走你
$ cd my-project
$ npm run dev
```

<!-- more -->

![项目目录](https://upload-images.jianshu.io/upload_images/5903867-7a26c2cf4afc74a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 二、 Vue的路由  [vue-router ](https://router.vuejs.org/zh-cn/)

vue-router在全家桶里面定位是什么呢? 前端路由、创建单页应用！
```
# 摘自vue-router官方解释
用 Vue.js + vue-router 创建单页应用，是非常简单的。
使用 Vue.js ，我们已经可以通过组合组件来组成应用程序，
当你要把 vue-router 添加进来，我们需要做的是，
将组件(components)映射到路由(routes)，然后告诉 vue-router 在哪里渲染它们
```
vue的核心概念是组件(components)，vue-router将组件(components)映射到路由(routes)，然后告诉 vue-router 在哪里渲染它们！

##### 关于路由的使用
1、在App.vue中新建视图 --- 渲染到哪
![App.vue](https://upload-images.jianshu.io/upload_images/5903867-e4fc8d058ac3819a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、 路由页面（router/module/index）编写路由 --- 渲染什么模块
![image.png](https://upload-images.jianshu.io/upload_images/5903867-1816f411311f596a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

module1、module2分文件夹，是解决大型项目路由复杂的问题。业务复杂度不高的可以直接把index.js文件拿出来

如果出现使用 ()=>import(@/component/index),加载组件报错的问题，可能是其他方式构建的项目，没有使用babel,或者缺少babel插件syntax-dynamic-import，解决办法参考： [vue-router按需加载](http://www.cnblogs.com/xiaochongchong/p/7772773.html)

3、 路由配置好之后，路由怎么跳转
路由按照上面的写法就配置好了各个路由，那么在页面之间需要路由跳转怎么做呢？
```
a. 标签导航 <router-link to="/foo">Go to Foo</router-link>
b. 编程式导航  $router.push()、$router.replace()
```
查看详细：[vue-router](https://router.vuejs.org/zh-cn/essentials/getting-started.html)

#### 三、[axios](https://github.com/axios/axios)(vue-resource已经不再维护，vue2.0官方推荐使用axios)

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。
*   从浏览器中创建 [XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
*   从 node.js 创建 [http](http://nodejs.org/api/http.html) 请求
*   支持 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) API
*   拦截请求和响应
*   转换请求数据和响应数据
*   取消请求
*   自动转换 JSON 数据
*   客户端支持防御 [XSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery)

```
//get
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

文档地址： [github](https://github.com/axios/axios) 、[中文翻译](https://www.kancloud.cn/yunye/axios/234845)

项目整合Axios的方法
1、全局注册axios，适用于请求较少的项目
在main.js中加入如下代码：
```
import axios from 'axios'
Vue.prototype.$http = axios
```

![全局注册](https://upload-images.jianshu.io/upload_images/5903867-95e30c165681a3ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、封装
```
//新建一个JS命名为api
import axios from 'axios' 
//在api中导入axios
let base = '/v1'

//把整个项目的网络请求都写在这个文件中用export导出

export const getproduct = params => { return axios.get(`${base}/product/info/${params}`) }

//这样写方便管理整个项目的网络请求
```

```
//在我们要是用这个请求时比如说getproduct

import {
    getproduct
  }from '../api/api';
export default {
  name: 'HelloWorld',
  data () {
    return {
      params:{}
    }
  },
  methods: {
    getProductList(){
      getproduct(this.params).then(result=>{
        console.log(result);
      })
    }
  }
}
//注意我们导出的时候用的是export 所以导入的时候必须带{}
```

扩展：[利用Aixos实现前端实现登录拦截](https://segmentfault.com/a/1190000008383094)

##### 跨域请求
跨域请求是web开发必须解决的一个问题。搞不定没有数据，就只能GAMEOVER了。 看这里不焦急~

不同域名之间的访问，需要跨域才能正确请求。跨域的方法很多，通常都需要后台配置
不过 Vue-cli 创建的项目，可以直接利用 Node.js 代理服务器，实现跨域请求【正向代理】

1、在 config>index.js 的 dev 中添加配置项 proxyTable：
* '/api' 为匹配项
* target 为被请求的地址

![配置代理](https://upload-images.jianshu.io/upload_images/5903867-dd49260f46ecf48f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

参考: 
[Vue-cli 创建的项目如何跨域请求](http://www.cnblogs.com/wisewrong/p/6609594.html)
[前端开发如何独立解决跨域问题](https://segmentfault.com/a/1190000010719058) --- 这篇文章是标题党

#### Vue 知识点总结
```
vue 的核心思想是，数据驱动和组件化。
```
1、MVVM数据驱动，有别于传统jq+bootstrap的技术栈，代码组织的时候，需要摒弃操作dom的想法，一切皆数据！一切交互的操作，都是对数据的修改~
![数据驱动](https://upload-images.jianshu.io/upload_images/5903867-b117fe93f9f5eac9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、组件化，其实就是把页面进行分块处理，分成多个小块，每个小块就是一个组件，这样可以形成组件的复用，而且提高开发效率。
把应用根据功能拆分为一个个颗粒度合理的组件，需要一些项目锻炼。
扩展： [编写良好的前端组件](https://imys.net/20170317/write-good-front-end-component.html)
![组件树](https://upload-images.jianshu.io/upload_images/5903867-7287367a0d8f9202.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、指令
文本 {% raw %}
{{ }} 、
{% endraw %} v-html、v-show、v-if、v-else、v-if-else、v-once、v-bind、v-for、v-on、

#### 区别点
a. v-if vs v-show
```
v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。
```

b. v-for 列表渲染
```
# 当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。
# 如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序，
# 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。
#  so. 列表渲染，被渲染组件要有唯一的key值，不应该使用index作为key值，否则会产生bug
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
```
[:key="index" 复现bug](http://jsrun.net/NgZKp/edit)

4、修饰符
.prevent 、.stop 、.laze

5、 组件通信 --- 数据的流动

* 组件内部通讯(data、computed)
![data/computed](https://upload-images.jianshu.io/upload_images/5903867-717bb0cce7199c1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 父组件 => 子组件 （props）
当子组件想访问父组件的值的时候，我们可以通过子组件的props属性，将父组件的值传递给子组件
```
单向数据流
Vue采用的是单向数据流方式，数据只能从父组件流向子组件，
子组件不能修改父组件传入的数据，否则Vue会给与警告。
```
![props](https://upload-images.jianshu.io/upload_images/5903867-aae29dcd2ec7f2c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 子组件 => 父组件
官网推荐的方法是在子组件中通过v-on绑定自定义事件来实现
每个 Vue 实例都实现了事件接口，即：
使用 $on(eventName) 监听事件
使用 $emit(eventName) 触发事件
这个的运行跟我们常用的dom原生的事件机制是一样的。父组件注册好监听事件，当子组件需要对父组件进行操作的时候，调用触发函数，触发事件。
代码案例： [Vue子组件传值给父组件](http://jsrun.net/IgZKp/edit)

* 非父子组件的相互通信
![event bus](https://upload-images.jianshu.io/upload_images/5903867-9dee74b23d09ae2f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 状态管理模式 [vuex](https://vuex.vuejs.org/zh-cn/) 

#### slot分发内容

为了让组件可以组合，我们需要一种方式来混合父组件的内容与子组件自己的模板。这个过程被称为**内容分发** (即 Angular 用户熟知的“transclusion”)。Vue.js 实现了一个内容分发 API，参照了当前 [Web Components 规范草案](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md)，使用特殊的 `<slot>` 元素作为原始内容的插槽。
```
    使用slot的好处就是可以定制个性化组件结构
```
* #### [单个插槽](https://cn.vuejs.org/v2/guide/components.html#%E5%8D%95%E4%B8%AA%E6%8F%92%E6%A7%BD "单个插槽")
![单个插槽](https://upload-images.jianshu.io/upload_images/5903867-8d9bead2ae781c76.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* #### [具名插槽](https://cn.vuejs.org/v2/guide/components.html#%E5%85%B7%E5%90%8D%E6%8F%92%E6%A7%BD "具名插槽")
![具名插槽](https://upload-images.jianshu.io/upload_images/5903867-370c3fe88b383fec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* #### [作用域插槽](https://cn.vuejs.org/v2/guide/components.html#%E4%BD%9C%E7%94%A8%E5%9F%9F%E6%8F%92%E6%A7%BD "作用域插槽")

子组件将数据像props一样，传递到父组件中，使得父组件根据数据映射视图，并插入到子组件slot的位置。。 

这个功能太变态。。。 实在不明白这样搞有什么用。。。


# vue知识点扩展：
* [异步组件](https://cn.vuejs.org/v2/guide/components.html#%E5%BC%82%E6%AD%A5%E7%BB%84%E4%BB%B6)
* [mixins](https://cn.vuejs.org/v2/guide/mixins.html)
* [过滤器](https://cn.vuejs.org/v2/guide/filters.html)
* [深入vue响应式原理](https://cn.vuejs.org/v2/guide/reactivity.html)
* [SSR](https://cn.vuejs.org/v2/guide/ssr.html)
* [单元测试](https://cn.vuejs.org/v2/guide/unit-testing.html)

# vue技术栈扩展：