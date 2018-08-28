## React-native项目优化
1. 图片加载优化
推广二维码以前是所有图片同时加载,现在是根据后台返回图片总数先显示图片加载占位符,然后分步加载.

2. 路由优化
由navigator(单线程)升级成react-navigation(多线程)
后者路由的线程和JS线程独立运行,不会引起路由跳转时卡顿


## React项目优化
#### 1. 给组件绑定点击函数时,我们习惯用箭头函数
```
<MyInput onChange={e => this.props.update(e.target.value)} />
```
但由于每次 render 操作 MyInput 组件的 onChange 属性都会返回一个新的函数，由于引用不一样，所以父组件的 render 也会导致 MyInput 组件的 render ，即使没有任何改动，所以需要尽量避免这样的写法，最好这样写：
```
update = (e) => {
  this.props.update(e.target.value)
}
render() {
  return <MyInput onChange={this.update} />
}
```
#### 2. PureComponent
如果你编写的是纯粹的组件（返回值完全由props和state所决定），你可以利用PureComponent来为你做这个工作 . 当组件更新时，如果组件的 props 和 state 都没发生改变， render 方法就不会触发，省去 Virtual DOM 的生成和比对过程，达到提升性能的目的**注意,PureComponent的比较只是浅比较,也就是只比较了第一层的值**,所以下例是不会更新的
```
class App extends PureComponent {
  state = {
    items: [1, 2, 3]
  }
  handleClick = () => {
    const { items } = this.state;
    items.pop();
    this.setState({ items });
  }
  render() {
    return (<div>
      <ul>
        {this.state.items.map(i => <li key={i}>{i}</li>)}
      </ul>
      <button onClick={this.handleClick}>delete</button>
    </div>)
  }
}
```
会发现，无论怎么点 delete 按钮， li 都不会变少，因为 items 用的是一个引用， shallowEqual 的结果为 true 。改正：
```
handleClick = () => {
  const { items } = this.state;
  items.pop();
  this.setState({ items: [].concat(items) });
}
```
如果不使用任何 state、refs 或 生命周期方法,则可以使用无状态函数式组件,即返回 JSX 的函数.
3. InteractionManager确保长时间的任务(比如异步请求)能在交互/动画完成之后运行(比如 Navigator的转场动画,这样就确保路由切换时不会卡),可以在每个页面cdm请求数据的代码外面包一层
```
InteractionManager.runAfterInteractions(() => {
  // ...long-running synchronous task...
});