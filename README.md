# vue-nuxt2-app

> vue nuxt2 demo


For detailed explanation on how things work, checkout [Nuxt.js docs](https://nuxtjs.org).

## 搭建指南
``` bash
# 使用脚手架
$ npx create-nuxt-app <项目名>
# 集成选择(Element UI + axios  + ESLint)
1. 在集成的服务器端框架之间进行选择:
- [x] None (Nuxt默认服务器)
- [ ] Express
- [ ] Koa
- [ ] Hapi
- [ ] Feathers
- [ ] Micro
- [ ] Adonis (WIP)
2. 选择您喜欢的UI框架:
- [ ] None (无)
- [ ] Bootstrap
- [ ] Vuetify
- [ ] Bulma
- [ ] Tailwind
- [x] Element UI
- [ ] Buefy
3. 选择你想要的Nuxt模式 (Universal or SPA)
- [x] Universal
- [ ] Single Page App
4. 添加 axios module 以轻松地将HTTP请求发送到您的应用程序中。
- [ ] no
- [x] yes
5. 添加 EsLint 以在保存时代码规范和错误检查您的代码。
- [ ] no
- [x] yes
6. 添加 Prettier 以在保存时格式化/美化您的代码。
- [x] no
- [ ] yes
```
## 配置ESLint规则
修改```.eslintrc.js```文件```rules```属性
```
    // 取消强制执行每行的最大属性数
    "vue/max-attributes-per-line": 0,
    // 允许空标签
    'vue/html-self-closing': 0,
    // 禁止弹窗
    'no-alert': 2,
    // 箭头函数必须有空格
    'arrow-spacing': 'error',
    // 函数定义时括号前面必须有空格
    'space-before-function-paren': [
      'error',
      'always'
    ],
    // 禁止使用var
    'no-var': 2,
    // 使用单引号
    'quotes': [
      'error',
      'single'
    ],
    // 代码末尾不加分号
    'semi': [
      'error',
      'never'
    ]
```
## 配置Sass
```
npm i sass node-sass sass-loader -D --registry=https://registry.npm.taobao.org
```
示例参考demo/sass

## 使用axios
API地址:https://axios.nuxtjs.org/
> 配置Proxy

修改nuxt.config.js，追加proxy属性
```
proxy: {
    '/api/': {
      target: 'http://dev.nuxtdemo.com:3001', // api主机
      pathRewrite: { '^/api': '' }
    }
}
```
修改axios属性
```
/*
** Axios module configuration
*/
axios: {
    // See https://github.com/nuxt-community/axios-module#options
prefix: process.env.NODE_ENV === 'production' ? '/' : '/api/',
proxy: true
},
```
> axios示例
```
// 你可能想要在服务器端获取并渲染数据。Nuxt.js添加了asyncData方法使得你能够在渲染组件之前异步获取数据。
async asyncData (app) {
    let { data } = await app.$axios.get('/api/v1/users').then(res => {
      return res
    })
    console.table(data)
    return {
      users: data
    }
}
```
示例参考/demo/data
## 使用plugins
插件目录 plugins 用于组织那些需要在 根vue.js应用 实例化之前需要运行的 Javascript 插件。

示例参考/demo/ele-ui
## HTML 头部
Nuxt.js 使用了 vue-meta 更新应用的 头部标签(Head) and html 属性。
个性化特定页面的 Meta 标签
```
head () {
    return {
      title: '优购时尚商城-时尚服饰鞋包网购首选-优生活，购时尚！',
      meta: [
        { hid: 'keywords', name: 'keywords', content: '优购网,优购时尚商城,优购网上鞋城' },
        { hid: 'description', name: 'description', content: '优购网-时尚服饰鞋包网购首选,经营耐克、阿迪达斯、Kappa、匡威、百丽、他她、天美意、森达等众多知名品牌,100%专柜正品,免费送货,10天退换货,提供愉悦购物体验!' }
      ]
    }
  }
```
示例参考/demo/head
## 使用Vuex
Nuxt.js 内置引用了 vuex 模块，所以不需要额外安装。

Nuxt.js 支持两种使用 store 的方式，你可以择一使用：

1. 普通方式： store/index.js 返回一个 Vuex.Store 实例
2. 模块方式： store 目录下的每个 .js 文件会被转换成为状态树指定命名的子模块 （当然，index 是根模块）

在store目录下新建根模块 index.js
```
export const state = () => {
  list: []
}

export const getter = () => {
  getList: state => state.list
}

export const mutations = {
  SET_LIST (state, data) {
    state.list = data
  }
}

export const actions = {
  async getList ({ commit }) {
    return this.$axios.get('/api/v1/users').then(res => {
      return commit('SET_LIST', res.data)
    })
  }
}

```
引用根模块
```
import { mapState } from 'vuex'
export default {
  fetch ({store, params}) {
    return store.dispatch('getList')
  },
  computed: {
    ...mapState({
      list: state => state.list
    })
  }
}
```
示例参考/demo/store
## Build Setup

``` bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm start

# generate static project
$ npm run generate
```
