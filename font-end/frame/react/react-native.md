## Direct Manipulation
### setNativeProps 
TouchableOpacity对复合组件不生效,如下会报错
```
class MyButton extends React.Component {
  render() {
    return (
      <View>
        <Text>{this.props.label}</Text>
      </View>
    )
  }
}

export default class App extends React.Component {
  render() {
    return (
      <TouchableOpacity>
        <MyButton label="Press me!" />
      </TouchableOpacity>
    )
  }
}
```
除非对复合组件使用setNativeProps
```
class MyButton extends React.Component {
  setNativeProps = (nativeProps) => {
    this._root.setNativeProps(nativeProps);
  }

  render() {
    return (
      <View ref={component => this._root = component} {...this.props}>
        <Text>{this.props.label}</Text>
      </View>
    )
  }
}
```
这样复合组件就可以包裹在TouchableOpacity中了;

### measure
可以直接调用measure操作DOM来获取位置
```
this.ScrollableTabViewDom.measure((x, y, width, height, left, top) => {
  console.log(x, y, width, height, left, top);
});
```

## Platform
除了用Platform.OS === 'ios'判断,还有另一种对象的形式
```
import {Platform, StyleSheet} from 'react-native';

const styles = StyleSheet.create({
  container: {
    flex: 1,
    ...Platform.select({
      ios: {
        backgroundColor: 'red',
      },
      android: {
        backgroundColor: 'blue',
      },
    }),
  },
});
```
不仅可以用于样式,还可以动态加载不同组件
```
const Component = Platform.select({
  ios: () => require('ComponentIOS'),
  android: () => require('ComponentAndroid'),
})();

<Component />;
```
甚至可以检测安卓和IOS版本
```
//ANDROID
import {Platform} from 'react-native';

if (Platform.Version === 25) {
  console.log('Running on Nougat!');
}
```
```
//IOS
import {Platform} from 'react-native';

const majorVersionIOS = parseInt(Platform.Version, 10);
if (majorVersionIOS <= 9) {
  console.log('Work around a change in behavior');
}
```

## Image
如果在请求网络图片时想附带其它参数,可以这样
```
<Image
  source={{
    uri: 'https://facebook.github.io/react/logo-og.png',
    method: 'POST',
    headers: {
      Pragma: 'no-cache',
    },
    body: 'Your Body goes here',
  }}
  style={{width: 400, height: 400}}
/>
```
另外二进制图片页要声明宽高,并且只推荐很小的图才用这种形式(如ICON)
```
// include at least width and height!
<Image
  style={{
    width: 51,
    height: 51,
    resizeMode: Image.resizeMode.contain,
  }}
  source={{
    uri:
      'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADMAAAAzCAYAAAA6oTAqAAAAEXRFWHRTb2Z0d2FyZQBwbmdjcnVzaEB1SfMAAABQSURBVGje7dSxCQBACARB+2/ab8BEeQNhFi6WSYzYLYudDQYGBgYGBgYGBgYGBgYGBgZmcvDqYGBgmhivGQYGBgYGBgYGBgYGBgYGBgbmQw+P/eMrC5UTVAAAAABJRU5ErkJggg==',
  }}
/>
```
另外,IOS下可以设置图片Cache Control

背景图片现在得用ImageBackground
```
  <ImageBackground source={...}>
    <Text>Inside</Text>
  </ImageBackground>
```

## Webview
1. 实现webview内部前进/后退功能

```
<WebView
  ref={webview => {
    this.webview = webview;
  }}
/>

this.webview.goBack()
this.webview.goForward()
```

2. 监听安卓返回键
```
  onBackAndroid = () => {
    if (this.state.webviewCanGoBack) {
      this.webview.goBack();
    } else {
      this.props.navigator && this.props.navigator.pop();
    }
    return true;
  };
  
    <WebView
      ref={webview => {
        this.webview = webview;
      }}
      onNavigationStateChange={navState => {
        this.setState({
          webviewCanGoBack: navState.canGoBack ? true : false,
          webviewCanGoForward: navState.canGoForward ? true : false,
        });
      }}      
    />
/>
  
```
3. webview与app通信
```
  handleMessage = event => {
    try {
      const json = event.nativeEvent.data;
      const options = JSON.parse(json);
      const { type, option } = options;

    } catch (e) {
      //
    }
  };
  
    <WebView
        onMessage={this.handleMessage} 
    />  
```

### Timer
除了setInterval和setTimeout外,还有setImmediate, clearImmediate和requestAnimationFrame, cancelAnimationFrame;

**setImmediate**和promise的机制一样,在当前事件循环结束前执行,相当于插入一个任务.

#### InteractionManager
InteractionManager确保长时间的任务(比如异步请求)能在交互/动画完成之后运行(比如 Navigator的转场动画,这样就确保路由切换时不会卡),可以在每个页面cdm请求数据的代码外面包一层
```
import { InteractionManager } from 'react-native';

componentDidMount() {
    InteractionManager.runAfterInteractions(() => {
      // ...long-running synchronous task...
    });
}
```

#### TimerMixin/react-mixin(ES6)
在TimerMixin/react-mixin(ES6)中
将setTimeout(fn, 500)改成this.setTimeout(fn, 500),这样当组件unmount的时候也会自动清除定时器
```
import TimerMixin from 'react-timer-mixin';

var Component = createReactClass({
  mixins: [TimerMixin],
  componentDidMount: function() {
    this.setTimeout(() => {
      console.log('I do not leak!');
    }, 500);
  },
});
```

### translate
```
const styles = StyleSheet.create({
  t1: {
    transform: [{translateX:100}] //x平移
  },
  t2: {
    transform: [{translateY:50}] //y平移
  },
  
  t1: {
    transform: [{scaleX:2}] //x方向放大
  },
  t2: {
    transform: [{scaleY:2}] //y方向放大
  },
  t3: {
    transform: [{scale:2}] //X/Y方向都放大
  },  
  
  t1: {
    transform: [{rotateX:'45deg'}] //X方向旋转
  },
  t2: {
    transform: [{rotateY:'45deg'}]
  },
  t3: {
    transform: [{rotateZ:'45deg'}]
  },
  t4: {
    transform: [{rotate:'45deg'}] //不指定轴旋转
  }
  
  t1: {
    transform: [{skewX:'45deg'}] //X方向倾斜
  },
  t2: {
    transform: [{skewY:'45deg'}]
  }  
});
```
