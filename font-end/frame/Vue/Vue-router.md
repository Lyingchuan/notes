### 全局守卫:
1. router.beforeEach 全局前置守卫 进入路由之前
2. router.beforeResolve 全局解析守卫(2.5.0+) 在beforeRouteEnter调用之后调用
3. router.afterEach 全局后置钩子 进入路由之后

```
// main.js 入口文件
import router from './router'; // 引入路由
router.beforeEach((to, from, next) => { 
  next();
});
router.beforeResolve((to, from, next) => {
  next();
});
router.afterEach((to, from) => {
  console.log('afterEach 全局后置钩子');
});
```
to和from是将要进入和将要离开的路由对象,路由对象指的是平时通过this.$route获取到的路由对象。

next:Function 这个参数是个函数，且必须调用，否则不能进入路由(页面空白)。

### 路由组件内的守卫：
1. beforeRouteEnter 进入路由前
2. beforeRouteUpdate (2.2) 路由复用同一个组件时
3. beforeRouteLeave 离开当前路由时

```
beforeRouteEnter (to, from, next) {
// 在路由独享守卫后调用 不！能！获取组件实例 `this`，组件实例还没被创建.不过可在next回调中访问组件实例
},
beforeRouteUpdate (to, from, next) {
// 在当前路由改变，但是该组件被复用时调用 可以访问组件实例 `this`
// 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
// 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
},
beforeRouteLeave (to, from, next) {
// 导航离开该组件的对应路由时调用，可以访问组件实例 `this`
}
```