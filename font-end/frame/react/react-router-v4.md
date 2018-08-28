[官方中文文档](http://reacttraining.cn/web/example/basic)

### 组件实现原理
```
import React, { PropTypes, Component } from 'react'
let instances = []
const register = (comp) => instances.push(comp)
const unregister = (comp) =>      instances.splice(instances.indexOf(comp), 1)
const historyPush = (path) => {
  history.pushState({}, null, path)
  instances.forEach(instance => instance.forceUpdate())
}
const historyReplace = (path) => {
  history.replaceState({}, null, path)
  instances.forEach(instance => instance.forceUpdate())
}
const matchPath = (pathname, options) => {
  const { exact = false, path } = options
  if (!path) {
    return {
      path: null,
      url: pathname,
      isExact: true
    }
  }
  const match = new RegExp(`^${path}`).exec(pathname)
  if (!match)
    return null
  const url = match[0]
  const isExact = pathname === url
  if (exact && !isExact)
    return null
  return {
    path,
    url,
    isExact,
  }
}

class Route extends Component {
  static propTypes: {
    path: PropTypes.string,
    exact: PropTypes.bool,
    component: PropTypes.func,
    render: PropTypes.func,
  }
  componentWillMount() {
    addEventListener("popstate", this.handlePop)
    register(this)
  }
  componentWillUnmount() {
    unregister(this)
    removeEventListener("popstate", this.handlePop)
  }
  handlePop = () => {
    this.forceUpdate()
  }
  render() {
    const {
      path,
      exact,
      component,
      render,
    } = this.props
    const match = matchPath(location.pathname, { path, exact })
    if (!match)
      return null
    if (component)
      return React.createElement(component, { match })
    if (render)
      return render({ match })
    return null
  }
}

class Link extends Component {
  static propTypes = {
    to: PropTypes.string.isRequired,
    replace: PropTypes.bool,
  }
  handleClick = (event) => {
    const { replace, to } = this.props
    event.preventDefault()
    replace ? historyReplace(to) : historyPush(to)
  }
  render() {
    const { to, children} = this.props
    return (
      <a href={to} onClick={this.handleClick}>
        {children}
      </a>
    )
  }
}
```
- 监听popstate事件,前进后退时触发强制更新视图;
- <Link>标签阻止了a标签的默认点击事件;
- 所有的路由栈都放在一个数组里(instances)方便操作
- pushState 和 replaceState 都接收三个参数。第一个参数是一个与历史实体相关联的对象，我们不需要，所以设置成一个空对象。第二个参数是标题，我们也不需要，所以也设置成空。第三个是我们需要使用的，指的是：相关 URL。

### V4版本
在react-router v4中可以将各组件标签放进<Router></Router>中

---

```
<Route exact path="/" component={RootRouter} />
```
exact作用是完全匹配,必须是"/"后面不跟任何东西才匹配

经常用来配置根路由(如不设置exact,则渲染"/xx"路由时也会渲染"/"根路由)

### <Switch>
```
<Route path="/about" component={About}/>
<Route path="/:user" component={User}/>
<Route component={NoMatch}/>
```
如果URL是 /about,那么上面3个路由都会匹配
```
<Switch>
  <Route exact path="/" component={Home}/>
  <Route path="/about" component={About}/>
  <Route path="/:user" component={User}/>
  <Route component={NoMatch}/>
</Switch>
```
若加上<Switch>,则只会匹配一个