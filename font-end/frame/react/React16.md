React v16.2新特性
### 1. Fragment
```
render() {
  return (
    <React.Fragment>
      注：        
      <p>产品说明一</p>
      <p>产品说明二</p>
    </React.Fragment>
  );
}
```
可以看到是一个正常单节点写法，直接包裹里面的内容。但是 Fragment 本身并不会产生真实的 DOM 节点，因此也不会导致层级嵌套增加。还可简写为 <></>
```
render() {
  return (
    <>
      注：
      <p>产品说明一</p>
      <p>产品说明二</p>
    </>
  );}
```
### 2. Ref API

以前
```
// callback 获取
render() {
  return (
    <input 
    	ref={el => {this.input = el;}}
    />
  );
}
```
Ref API
```
constructor(props) {
  super(props);
  this.input = React.createRef();
}
componentDidMount() {
  console.log(this.input);
}
render() {
  return <input ref={this.input} />;
}
```
获取子组件Ref
```
class MyButton extends Component {
  constructor(props){
    super(props);
    this.buttonRef = React.createRef();
  }
  render(){
    return (
      <button ref={this.buttonRef}>
        {props.children}
      </button>
    );
  }
}
class App extends Component {
  constructor(props){
    super(props);
    this.myRef = React.createRef();
  }
  componentDidComponent{
    // 通过ref一层层访问
    console.log(this.myRef.buttonRef);
  }
  render(){
    return (
      <MyButton ref={this.myRef}>
        Press here
      </MyButton>
    );
  }
}
```
### 3. 防止 XSS(跨站脚本) 攻击
```
const title = response.potentiallyMaliciousInput;
// 直接使用是安全的：
const element = <h1>{title}</h1>;
```
React DOM 在渲染之前默认会过滤所有传入的值。它可以确保你的应用不会被注入攻击。所有的内容在渲染之前都被转换成了字符串。这样可以有效地。

### 4. Context
```
// 创建一个 theme Context,  默认 theme 的值为 light
const ThemeContext = React.createContext('light');

function ThemedButton(props) {
  // ThemedButton 组件从 context 接收 theme
  return (
    <ThemeContext.Consumer>
      {theme => <Button {...props} theme={theme} />}
    </ThemeContext.Consumer>
  );
}

// 中间组件
function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class App extends React.Component {
  render() {
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}
```
上述只是在render的JSX语法中访问到了context,若想在生命周期中访问,则可以通过props传递
```
class Button extends React.Component {
  componentDidMount() {
    // ThemeContext value is this.props.theme
  }

  componentDidUpdate(prevProps, prevState) {
    // Previous ThemeContext value is prevProps.theme
    // New ThemeContext value is this.props.theme
  }

  render() {
    const {theme, children} = this.props;
    return (
      <button className={theme ? 'dark' : 'light'}>
        {children}
      </button>
    );
  }
}

export default props => (
  <ThemeContext.Consumer>
    {theme => <Button {...props} theme={theme} />}
  </ThemeContext.Consumer>
);
```

### 5. Portals

Portals 提供了一种很好的将子节点渲染到父组件以外的 DOM 节点的方式。
```
ReactDOM.createPortal(child, container)
```
第一个参数（child）是任何可渲染的React子元素，例如一个元素，字符串或碎片。第二个参数（container）则是一个 DOM 元素。
```
render() {
  // React does *not* create a new div. It renders the children into `domNode`.
  // `domNode` is any valid DOM node, regardless of its location in the DOM.
  return ReactDOM.createPortal(
    this.props.children,
    domNode,
  );
}
```
对于 portal 的一个典型用例是当父组件有 overflow: hidden 或 z-index 样式，但你需要子组件能够在视觉上“跳出（breakout）”其容器。例如，对话框、hovercards以及提示框.

对于Portals,即使子元素DOM节点不在React树父元素内,它也能冒泡到react树中的父元素上.

### 代码分割
之前
```
import { add } from './math';

console.log(add(16, 26));
```
之后
```
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```