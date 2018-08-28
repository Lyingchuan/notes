1. 用this.props.navigation.navigate('Details')不能重复进入相同的组件,相反,用this.props.navigation.push('Details')可以
2. 编程式返回this.props.navigation.goBack(),返回到顶端可以navigation.popToTop()或者navigate('Home') (not push!)
3. 
```
  Home: {
    screen: HomeScreen
  },
```
可以简写为
```
Home: HomeScreen
```

4. 传递属性
```
this.props.navigation.navigate('Details', {
  itemId: 86,
  otherParam: 'anything you want here',
});
```

5. 读取属性
```
this.props.navigation.getParam(paramName, defaultValue) //推荐
//或者
this.props.navigation.state.params
```
6. 通过静态属性navigationOptions读取props设置title(因为是静态属性,所以无法通过this.props的方式读取)
```
class DetailsScreen extends React.Component {
  static navigationOptions = ({ navigation }) => {
    return {
      title: navigation.getParam('otherParam', 'A Nested Details Screen'),
    };
  };
```
7. 设置属性
```
  <Button
    title="Update the title"
    onPress={() => this.props.navigation.setParams({otherParam: 'Updated!'})}
  />
```
8. 设置标题样式
```
class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Home',
    headerStyle: {
      backgroundColor: '#f4511e',
    },
    headerTintColor: '#fff',
    headerTitleStyle: {
      fontWeight: 'bold',
    },
  };
```
9. 统一设置所有导航栏样式
```
const RootStack = createStackNavigator(
  {
    Home: HomeScreen,
    Details: DetailsScreen,
  },
  {
    initialRouteName: 'Home',
    /* The header config from HomeScreen is now here */
    navigationOptions: {
      headerStyle: {
        backgroundColor: '#f4511e',
      },
      headerTintColor: '#fff',
      headerTitleStyle: {
        fontWeight: 'bold',
      },
    },
  }
);
```
10. 用组件的形式覆盖默认title
```
class LogoTitle extends React.Component {
  render() {
    return (
      <Image
        source={require('./spiro.png')}
        style={{ width: 30, height: 30 }}
      />
    );
  }
}

class HomeScreen extends React.Component {
  static navigationOptions = {
    // headerTitle instead of title
    headerTitle: <LogoTitle />,
  };

  /* render function, etc */
}
```
11. 导航栏和功能页互动
之前我们用的是Redux,现在RN也可以实现
```
class HomeScreen extends React.Component {
  static navigationOptions = ({ navigation }) => {
    return {
      headerTitle: <LogoTitle />,
      headerRight: (
        <Button
          onPress={navigation.getParam('increaseCount')}
          title="+1"
          color="#fff"
        />
      ),
    };
  };

  componentDidMount() {
    this.props.navigation.setParams({ increaseCount: this._increaseCount });
  }

  state = {
    count: 0,
  };

  _increaseCount = () => {
    this.setState({ count: this.state.count + 1 });
  };

  /* later in the render function we display the count */
}
```

12. 兼容IPX界面可以包裹一层
```
import { SafeAreaView } from 'react-navigation';

class App extends Component {
  render() {
    return (
      <SafeAreaView style={styles.container}>
        <Text style={styles.paragraph}>
          This is top text.
        </Text>
        <Text style={styles.paragraph}>
          This is bottom text.
        </Text>
      </SafeAreaView>
    );
  }
}
```

13. TabNavigator的statusBar取决于最后一个tab的statusBar,为了修复这个问题,
一般只在第一个页面设置statusBar,同时给每个tab添加事件监听函数
```
  componentDidMount() {
    this._navListener = this.props.navigation.addListener('didFocus', () => {
      StatusBar.setBarStyle('dark-content');
      isAndroid && StatusBar.setBackgroundColor('#ecf0f1');
    });
  }

  componentWillUnmount() {
    this._navListener.remove();
  }
```

14. 针对tabNavigator里面特定的screen隐藏tab,必须得对stack设置navigationOptions,而非里面的screen
```
const FeedStack = createStackNavigator({
  FeedHome: FeedScreen,
  Details: DetailsScreen,
});

const TabNavigator = createBottomTabNavigator({
  Feed: FeedStack,
  Profile: ProfileScreen,
});

const AppNavigator = createSwitchNavigator({
  Auth: AuthScreen,
  Home: TabNavigator,
});


FeedStack.navigationOptions = ({ navigation }) => {
  let tabBarVisible = true;
  if (navigation.state.index > 0) {
    tabBarVisible = false;
  }

  return {
    tabBarVisible,
  };
};
```

15. 将props.navigation传入任何组件
如果该组件不是screen,没有在stack里面定义,则读取不到props.navigation,如下
```
import React from 'react';
import { Button } from 'react-native';
import { withNavigation } from 'react-navigation';

export default class MyBackButton extends React.Component {
  render() {
    // This will throw an 'undefined is not a function' exception because the navigation
    // prop is undefined.
    return <Button title="Back" onPress={() => { this.props.navigation.goBack() }} />;
  }
}
```
此时必须要
```
<MyBackButton navigation={this.props.navigation} />
```
或者使用withNavigation高阶组件
```


class MyBackButton extends React.Component {
  render() {
    return <Button title="Back" onPress={() => { this.props.navigation.goBack() }} />;
  }
}

// withNavigation returns a component that wraps MyBackButton and passes in the
// navigation prop
export default withNavigation(MyBackButton);
```

16. deeplink
```
const SimpleApp = createStackNavigator({
  Home: { screen: HomeScreen },
  Chat: {
    screen: ChatScreen,
    path: 'chat/:user',
  },
});
```
