[React Native第三方组件库汇总](https://juejin.im/post/5b78d0fa6fb9a019be279af4)


## react-native-platform-touchable
[react-native-platform-touchable](https://github.com/react-community/react-native-platform-touchable)

该依赖提供一个component:<Touchable>

android >= 21版本使用的是<TouchableNativeFeedback>

android <= 20以及IOS使用的是<TouchableOpacity>

## react-loadable

[react-loadable](https://www.jianshu.com/p/697669781276)

```
import Loadable from 'react-loadable';

function MyLoadingComponent() {
  return <div>Loading...</div>;
}

const LoadableAnotherComponent = Loadable({
  loader: () => import('./another-component'),
  LoadingComponent: MyLoadingComponent
});

class MyComponent extends React.Component {
  render() {
    return <LoadableAnotherComponent/>;
  }
}
```

## autobind-decorator
[autobind-decorator github](https://github.com/andreypopp/autobind-decorator)


便捷绑定函数至当前实例
```
import autobind from 'autobind-decorator'

class Component {
  constructor(value) {
    this.value = value
  }

  @autobind
  method() {
    return this.value
  }
}

let component = new Component(42)
let method = component.method // .bind(component) isn't needed!
method() // returns 42


// Also usable on the class to bind all methods
// Please see performance if you decide to autobind your class
@autobind
class Component { }
```

## react-loading
[react-loading](react-loading)

一种集合多种加载动画的组件
```
import React from 'react';
import ReactLoading from 'react-loading';

const Example = ({ type, color }) => (
	<ReactLoading type={type} color={color} height={667} width={375} />
);

export default Example;
```


## whatwg-fetch
window.fetch polyfill,兼容那些不支持fetch的浏览器环境

## styled-components
提供一种在JS里写源生CSS的方式,同时可以将div p等标签重命名,另外CSS和JS在同一份文件里也减少了文件数量.

但个人更习惯于用css module和less
```
import React from 'react';

import styled from 'styled-components';

// Create a <Title> react component that renders an <h1> which is
// centered, palevioletred and sized at 1.5em
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

// Create a <Wrapper> react component that renders a <section> with
// some padding and a papayawhip background
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

// Use them like any other React component – except they're styled!
<Wrapper>
  <Title>Hello World, this is my first styled component!</Title>
</Wrapper>
```