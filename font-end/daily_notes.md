##### 取小数点后几位
```
num.toFixed(2) //取小数点后2位
```
打包tar 
```
tar czvf index.tar.gz index/
```
二进制浮点数精度问题,仅限于0.1和0.2的情况
```
0.1+0.2 === 0.3; //false 0.30000000000000004
```
判断一个值是否为整数
```
Number.isInteger(42.3) /false
Number.isInteger(42.000) /true
```
ES6新增方法Object.is()可以判断两个值是否绝对相等
```
Object.is(NaN,NaN) //true
NaN === NaN //false
```
大于等于号 >= 不要在HTML中使用,会和标签<>混淆,在JS中可以使用
#### Date()
Math.round(new Date().getTime()/1000)可以获取当前Unix时间戳,从1970年1月1日开始计算,每隔1秒+1000.中国处在东八区,有8*3600 = 28800的时差

要取得一星期前的日期可以
```
new Date(new Date().getTime() - 7 * 24 * 3600 * 1000).Format("yyyy-MM-dd")
```

DOM获取HTML和body
```
document.documentElement //html
document.body
```
#### 数组去重用Set

(new Set([1,1,2])的结果是一个对象,还要用扩展运算符展开)
```
var arr = [...new Set([1,1,2])]
```
```
flex:1只是在flex-direction的方向上flex:1,在副轴上不会
```
#### git删除远程库文件,例如
```
git rm src/screens/swipe.css
```

#### alignItems/alignSelf:

baseline: 项目的第一行文字的基线对齐。

stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

### 设置变量键名
```
this.setState({
    [item.id]: text
});
```
#### Object.assign()和对象扩展符
Object.assign()传的参数要以对象的形式传
```
Object.assign({}, state, {shareUrl:action.url});
```
对象扩展符,由于已经把对象({a:1,b:2})展开成a:1,b:2的形式,所以后面跟的参数直接可以传shareUrl:action.url
```
{ ...state, shareUrl:action.url };
```
#### 阻止双击文字选中
```
<div @click="toggleCheck" onselectstart="return false;">阻止默认点击选中效果</div>
```

### fetch 规范与 jQuery.ajax() 
主要有两种方式的不同，牢记：

当接收到一个代表错误的 HTTP 状态码时，从 fetch()返回的 Promise 不会被标记为 reject， 即使该 HTTP 响应的状态码是 404 或 500。相反，它会将 Promise 状态标记为 resolve （但是会将 resolve 的返回值的 ok 属性设置为 false ）， 仅当网络故障时或请求被阻止时，才会标记为 reject。

默认情况下, fetch 不会从服务端发送或接收任何 cookies, 如果站点依赖于用户 session，则会导致未经认证的请求（要发送 cookies，必须设置 credentials 选项）.

##### 2018-5-2

函数或变量名前加_,表示私有变量(函数)
###### package.json
波浪符号（~）：他会更新到当前minor version（也就是中间的那位数字）中最新的版本。放到我们的例子中就是：body-parser:~1.15.2，这个库会去匹配更新到1.15.x的最新版本，如果出了一个新的版本为1.16.0，则不会自动升级。波浪符号是曾经npm安装时候的默认符号，现在已经变为了插入符号。

插入符号（^）：这个符号就显得非常的灵活了，他将会把当前库的版本更新到当前major version（也就是第一位数字）中最新的版本。放到我们的例子中就是：bluebird:^3.3.4，这个库会去匹配3.x.x中最新的版本，但是他不会自动更新到4.0.0。

##### 2018-5-3
```
document.referrer //获取当前页面的上级页面(来源)URL
```

##### 2018-5-4
实现0.5px线(边框)
```
flex:1;
height: 1px;
background: #999999;
-webkit-transform: scaleY(.5);
transform:scaleY(.5);
```

##### 2018-5-22
paddingHorizontal,paddingVertical,StyleSheet.hairlineWidth

##### 2018-5-22
react 不同路由/组件之间通信可以用EventEmitter


utils/EventEmitter.js
```
import { EventEmitter } from 'events';

export default new EventEmitter();
```
发送
```
import emitter from '../utils/EventEmitter';
emitter.emit('pointService')
```
监听
```
emitter.addListener('pointService', this.showServiceModal);
```

##### 5-31
v-for 不仅可以遍历数组,还可以遍历对象

##### 6-1
& 与运算,当相与的两个位都为1时才为1;

应用--多选框:把二进制值和选项一一对应(1248),选中多项即把值相加,如选中第一第三项,则值为1+4 = 5,此时可用该值得知选中了哪几项;
```
5 & 1 //true
5 & 2 //false
5 & 4 //true
5 & 8 //false
```

##### 6-10
react-native中 绝对定位的元素,在代码行后面的要比前面的z-index值高

##### 6-11
计算属性名,指定一个表达式,并用这个表达式的结果作为属性的名称
```
this.setState({
   [..] : 1 
})
```
```
window.open(url)
```
如果url包含https://,则跳转地址就为该url.如果不含https://,则跳转地址为:当前域名+url

##### 6-13
让react-native textinput输入的文字居顶而不是居中可以对CSS设置
```
textAlignVertical: 'top'
```

##### 6-19
Truthy(真值)指的是在Boolean上下文中转换后的值为真的值,true就是单纯的布尔值;

---

JSX中,可以用IIFE来写一大堆逻辑
```
function render() {
  return (
    <div>
      {(() => {
        if (renderComponent1) {
          return <Component1 />;
        } else {
          return <div />;
        }
      })()}
    </div>
  );
}
```
高阶函数接收一个组件,返回处理过后的这个组件(如添加/删除属性)

##### 6-20
H5弹出modal时禁止底层滚动
```
弹出层父元素设置属性为overflow-y:scroll;
弹窗弹出时，用js控制让底层元素的position属性设置为fixed;
弹窗关闭时候，用js控制底层元素的position属性为正常；
在iOS端，为了弹窗里面的滚动效果看起来顺滑，需要设置弹窗层的包裹元素属性：-webkit-overflow-scrolling : touch;

最后在弹出层弹出的时候记录一下当前位置，然后在弹出层消失的时候js控制滚动到之前的位置
```

##### 6-21
SSR : Server Slide Rendering , 即服务器端渲染

##### 6-22
1. react-native 内外两层ScrollView的情况下,IOS可以分别触发滑动,ANDROID则只能触发一层滑动(默认外层),若要滑动内层,则需要将外层scrollEnabled设置为false.
2. hybrid app (some UIs in React Native, some UIs in platform code)

##### 6-26
动态插入meta标签
```
var dc = document.documentElement;

var mt = document.createElement("meta");
mt.name = "viewport";
mt.content = "width=device-width,initial-scale=1.0,maximum-scale=1.0, minimum-scale=1.0,user-scalable=no";
dc.firstElementChild.appendChild(mt);
```
#### 各途径获取宽高区别
对**window**

1.$(window).width()与$(window).height()等同于

document.documentElement.clientWidth与document.documentElement.clientHeight

获得的是屏幕可视区域的宽高，不包括滚动条与工具条

2.window.innerWidth与window.innerHeight,获得的是可视区域的宽高，但是window.innerWidth宽度包含了纵向滚动条的宽度，window.innerHeight高度包含了横向滚动条的高度(IE8以及低版本浏览器不支持)。

3.window.outerWidth与window.outerHeight：获得的是加上工具条与滚动条窗口的宽度与高度。

对**元素**:

1.offsetWidth & offsetHeight 返回本身的宽高 + padding + border + 滚动条

2.scrollWidth & scrollHeight这两个属性是元素的内容区域加上内边距，在加上任何溢出内容的尺寸.因此，如果没有溢出时，这些属性与clientWidth和clientHeight是相等的

3.scrollLeft & scrollTop指定的是元素的滚动条的位置scrollLeft和scrollTop都是可写的属性，通过设置它们来让元素中的内容滚动。

##### H5拨打电话
```
<a href="tel:4008751651">4008751651</a>。
```

##### 6-29
react封装的方法传入回调函数时,要对回调函数绑定this(通过bind或箭头函数),否则this不会指向当前组件.

当传递JSON字符串作为URL参数时,需要使用JSON.parse(decodeURI(jsonData)),先解码,再解析成对象;

##### 7-2
IOS网页防止自动识别电话号码变蓝,index.html添加
```
<meta name = "format-detection" content = "telephone=no">
```
不会影响到
```
<a href="tel:4008751651">4008751651</a>。
```

##### 7-4
APP用https协议,在android系统下webview会加载不出来http的图片

```
'android:screenOrientation="portrait"' //限制竖屏显示
'android:screenOrientation="landscape"' //限制横屏显示
```

##### 7-5 
取消rn黄色警告
```
console.disableYellowBox = true
// 或
import {YellowBox} from 'react-native';
YellowBox.ignoreWarnings(['Warning: ...']);
```
##### JS引擎
JavaScript引擎是专门处理JavaScript脚本的虚拟机，一般会附带在网页浏览器之中.
- 在iOS、Android的模拟器或是真机上，React Native使用的是JavaScriptCore
- 在使用Chrome调试时，所有的JavaScript代码都运行在Chrome中，并且通过WebSocket与原生代码通信。此时的运行环境是V8引擎。

##### 7-6
HTML input不可编辑/只读属性readOnly

##### 7-9
react同时定义props对象内外层时可以用
```
static propTypes = {
  user: PropTypes.shape({
    _id: PropTypes.string,
  )}.isRequired,
}
//它内部需要有一个 类型为字符串的 id 属性，而整个 user 对象又是必需的
```
确保使用了 props 的每个组件都声明了 propTypes 和 defaultProps，这对写出更好的 React 代码很有帮助。

React 从 v15.5 版本开始，不再内置 proptypes，需要作为独立的依赖包添加到你的项目中。或者用FLOW等.

##### 7-11
H5 navigator.geolocation.getCurrentPosition()可以用于获取地理位置,可以获取经纬度和海拔等信息;

##### 7-12
##### location.hash
location.hash输出的是#及后面的字符串内容,如'#print',可以用来定位到网页具体内容
```
<!--一是使用锚点-->
<a name="print"></a>
<!--二是使用id属性，比如-->
<div id="print" >
```
HTTP请求不包括#,#后面的字符不会被发送到服务端,改变#不会触发网页重载,但会记录到History中,这样前进后退可以回退到相应位置.

监听哈希改变事件
```
window.addEventListener("hashchange", func, false);
```

##### git merge 和 git merge --no-ff的区别
git merge –no-ff 可以保存你之前的分支历史记录。能够更好的查看 merge历史，以及branch 状态。git merge 则不会显示 feature，只保留单条分支记录。

**react-native调试时**,android必须打开"**读取运动数据**"的权限,才能摇一摇弹出弹窗;

##### 静态类型语言在编译时执行类型检查，而动态类型的语言在运行时执行类型检查。

##### 7-16
修改git全局配置，禁止git自动将lf转换成crlf,  命令：
```
git config --global core.autocrlf false
```

移动端触摸事件:onTouchStart,onTouchEnd

##### 7-17
浏览器获取和失去焦点事件
```
window.onfocus = function() { };
window.onblur = function() { };

// for IE
document.onfocusin = function() { };
document.onfocusout = function() { };
```

浏览器新消息通知可以改变document.title,使其闪烁来实现,也可以使用

##### Notification
```
// 先检查浏览器是否支持
if (!('Notification' in window)) {
  alert('该浏览器不支持消息通知,请换用Chrome浏览器或者关闭消息推送');
} else if (Notification.permission === 'granted') {
  // 检查用户是否同意接受通知
  // If it's okay let's create a notification
  var notification = new Notification('Hi there!');
} else if (Notification.permission !== 'denied') {
  // 否则我们需要向用户获取权限
  Notification.requestPermission(function(permission) {
    // 如果用户同意，就可以向他们发送通知
    //共有三种可能:granted, denied, 或default
    if (permission === 'granted') {
      var notification = new Notification('Hi there!');
    }
  });
}
```

##### 7-18
##### localStorage / sessionStorage
写入的几种方式
```
var storage=window.localStorage;
//写入a字段
storage["a"]=1;
//或者
storage.b=1;
//或者
storage.setItem("c",3);

//第一种方法读取
var a=storage.a;
//第二种方法读取
var b=storage["b"];
//第三种方法读取
var c=storage.getItem("c");
```
可以利用此原理实现react路由回退传值

##### 7-23
关于polyfill:移动端上，中国接近 94% 的用户浏览器，都是原生支持 Promise 的，并不需要 polyfill

为了优化,可以使用动态polyfill:polyfill.io
```
<script src="https://cdn.polyfill.io/v2/polyfill.min.js"></script>
```
它会根据你的浏览器 UA 头，判断你是否支持某些特性，从而返回给你一个合适的 polyfill。

##### 7-24

CSS控制文本一行显示,并带省略号
```
overflow: hidden;
white-space: nowrap;
text-overflow: ellipsis;
```

##### 7-25
移动端触摸效果不用:hover,用:active,若要考虑兼容性,则可通过绑定ontouchstart和ontouchend来控制按钮的类名即可


```
typeof null // 'object'
```
因为在 JS 的最初版本中，使用的是32位系统，为了性能考虑使用低位存储了变量的类型信息，000 开头代表是对象，然而 null 表示为全零，所以将它错误的判断为 object

==或===判断两个对象时,只有是同一引用才会返回true,若是不同引用,即使里面内容相同也是false

##### 7-26

箭头函数其实是没有 this的，箭头函数中的this只取决于他外面的第一个不是箭头函数的函数的 this;

函数的callee属性代表函数本身,caller 属性代表函数的调用者

临时性死区:在let中也存在变量提升这个概念的，但是在let和const中还存在一个叫做 “临时性死区” 的概念。在这个作用域的开始直到变量的声明之前，这个变量都是处在 “临时性死区” 当中的，这个时候引用他的话会报referenceError的错误

##### 7-27
css文字装饰
```
h1 {text-decoration:overline} //上划线
h2 {text-decoration:line-through}
h3 {text-decoration:underline} //下划线
h4 {text-decoration:blink} //闪烁
```

##### 7-31

url获取含中文的参数时,必须对其进行解码,否则会乱码
```
decodeURI(getKeyValue('amount'))
```
IOS网页返回默认不刷新页面,安卓会刷新

##### 8-1

git pull 和 git merge

git pull = git fetch + git merge

fetch和push命令可以分别对远程分支进行fetch和push操作，而pull不是直接跟远程分支对话的。
fetch同pull的区别在于：git fetch:是从远程获取最新版本到本地，不会自动merge
而git pull是从远程获取最新版本并merge到本地仓库
从安全角度出发，git fetch比git pull更安全，因为我们可以先比较本地与远程的区别后，选择性的合并。
git push 默认推送到master。

less自动编译成css,需要在VSCODE装上Easy LESS插件

##### 8-8

鼠标移到文字上显示内容可以对标签添加title属性,比如在VUE中
```
<td class="note-content" :title="item.note">{{item.note}}</td>
```

##### 8-13
string.codePointAt()是charAt()的升级版,能够查询>0xFFFF的字符。

string.repeat(n)会将字符串重复n次

在函数形参中使用...扩展运算符，可以将不定形参传入rest数组中。
```
function say(...res) {
   for (var item of res) {
     console.log(item);
   }
 }
 say(1, 2, 3)  
 //1
 //2
 //3
```

find:用于找出**第一**个符合条件的数组**元素**。找不到返回 undefined。

findIndex:返回**第一个**符合条件的数组元素的**索引**。找不到返回-1;

find(findIndex)只返回符合条件的第一个原始元素值(或索引),不会改变值本身.forEach对数组每一项都执行函数,没有返回值,map会返回执行结果.

对象的结构赋值也可以设置默认值,如
```
var {a = 10, b = 5} = {a: 3};

console.log(a); // 3
console.log(b); // 5
```

##### 8-14
使用requestAnimationFrame时,防止onscroll,mousemove时二次绘制动画的方法
```
const onScroll = e => {
    if (scheduledAnimationFrame) { return }

    scheduledAnimationFrame = true
    window.requestAnimationFrame(timestamp => {
        scheduledAnimationFrame = false
        animation(timestamp)
    })
}
window.addEventListener('scroll', onScroll)
```

##### 8-28
绑定函数作用域至bind括号内的参数值,属于ES5方法
```
this.toggleTabNavigator = this.toggleTabNavigator.bind(this);
```
ES6采用箭头函数

reverse()属于数组方法,不能对字符串使用,需将字符串用split('')转化成数组,reverse()后再join('')

源生JS事件中,this和currentTarget为注册的目标,target为实际触发的目标

__proto__是对象的属性,指向构造该对象的构造函数的原型
prototype则是函数的属性
```
'abc'.repeat(3) //'abcabcabc'
```

#### 8-29
JS获取某一类元素
```
document.querySelector("ul")
```

##### 计算图片大小
对于一张 100 * 100 像素的图片来说，图像上有 10000 个像素点，如果每个像素的值是 RGBA 存储的话，那么也就是说每个像素有 4 个通道，每个通道 1 个字节（8 位 = 1个字节），所以该图片大小大概为 39KB（10000 * 1 * 4 / 1024）。

将 script 标签放在 body 底部，因为 JS 文件执行会阻塞渲染。当然也可以把 script 标签放在任意位置然后加上 defer ，表示该文件会并行下载，但是会放到 HTML 解析完成后顺序执行。对于没有任何依赖的 JS 文件可以加上 async ，表示加载和渲染后续文档元素的过程将和 JS 文件的加载与执行并行无序进行。

静态资源尽量使用 CDN加载，由于浏览器对于单个域名有并发请求上限，可以考虑使用多个 CDN 域名。

##### CORS
CORS需要浏览器和后端同时支持。IE 8 和 9 需要通过 XDomainRequest 来实现。

浏览器会自动进行 CORS 通信，实现CORS通信的关键是后端。只要后端实现了 CORS，就实现了跨域。

服务端设置 Access-Control-Allow-Origin 就可以开启 CORS。 该属性表示哪些域名可以访问资源，如果设置通配符则表示所有网站都可以访问资源。

##### 重绘（Repaint）和回流（Reflow）

重绘是当节点需要更改外观而不会影响布局的，比如改变 color 就叫称为重绘
回流是布局或者几何属性需要改变就称为回流。
回流必定会发生重绘，重绘不一定会引发回流。回流所需的成本比重绘高的多，改变深层次的节点很可能导致父节点的一系列回流。

#### 8-30
react-native的Image source属性除了uri还可以设置catch/method/scale等属性
```
<Image
  source={{uri: 'https://facebook.github.io/react-native/docs/assets/favicon.png',catch:"force-cache"}}
/>
```

#### 9-6
string.search(searchvalue)

search() 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。而indexOf的参数只能是字符串

#### 9-17
```
const obj = {a:1,b:2}
obj.a = 2
obj.b = 1
//ok,改变的是里面的键值对
obj = {c: 3}
//error,改变的是整个obj的映射,应该用let
```

#### 9-18
[github上解决react-native安卓无法hot reloading的问题](https://github.com/facebook/react-native/issues/18899)

#### 9-20
[解决react-native run-android时,64K引用限制问题](https://developer.android.com/studio/build/multidex)

#### 9-25
console.group
```
  console.group('%cNetwork Request Failed:', 'color: red');
  console.log('%cURL  : %o', 'font-weight: bold', url);
  console.log(
    '%cBody : %o',
    'font-weight: bold',
    data,
  );
  console.log('%cRes  : %o', 'font-weight: bold', res);
  console.groupEnd();
```

#### 10-12

##### MVVM和MVC的区别
MVVM把View和Model的同步逻辑自动化了(通过双向数据绑定),就是View的变化能实时让Model发生变化，而Model的变化也能实时更新到View

#### 进程和线程的区别

根本区别：进程是操作系统资源分配的基本单位，而线程是任务调度和执行的基本单位。

在开销方面：每个进程都有独立的代码和数据空间（程序上下文），程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一类线程共享代码和数据空间，每个线程都有自己独立的运行栈和程序计数器（PC），线程之间切换的开销小。

所处环境：在操作系统中能同时运行多个进程（程序）；而在同一个进程（程序）中有多个线程同时执行（通过CPU调度，在每个时间片中只有一个线程执行）。

内存分配方面：系统在运行的时候会为每个进程分配不同的内存空间；而对线程而言，除了CPU外，系统不会为线程分配内存（线程所使用的资源来自其所属进程的资源），线程组之间只能共享资源。

包含关系：没有线程的进程可以看做是单线程的，如果一个进程内有多个线程，则执行过程不是一条线的，而是多条线（线程）共同完成的；线程是进程的一部分，所以线程也被称为轻权进程或者轻量级进程。

##### 面向对象三大特性：封装,继承,多态。

#### 10-15
chrome修改user-agent,扩展:User-Agent Switcher

git push -u 是绑定默认提交的远程版本库

git merge 和git rebase区别：

merge不会修改提交历史，rebase会修改提交历史 。
rebase只应用于本地没有提交的代码，如果应用到已经提交到远程的分支不要应用，不然会非常的麻烦，git merge 可以应用于远程分支。

#### 10-17
```
1.如果当前页是http://localhost:3000/newdevicemanage/

window.location.href = `newdevicemanage/home?id=${id}`

// http://localhost:3000/newdevicemanage/newdevicemanage/home?id=${id};

2.如果当前页是http://localhost:3000/newdevicemanage

window.location.href = `newdevicemanage/home?id=${id}`

// http://localhost:3000/newdevicemanage/home?id=${id};
```

#### 10-24
react.js复制文字可以引用依赖react-copy-to-clipboard,用法见github

#### 10-31
封装组件
```
import React from 'react';

const ImageWrapper = ({ desc }) => <div>{desc && <div>{desc}</div>}</div>;

export default ImageWrapper;
```
使用有两种方式
```
1.
  render() {
    return <ImageWrapper desc="示意图" />;
  }
2.
  render() {
    return (
    <div>
        {ImageWrapper({desc: '示意图'})}
    </div>
    );
  }
```

#### 11-2
reduce的使用
```
[{x:1},{y:2},{z:3}].reduce(function(prev, next){
    return Object.assign(prev, next);
})

<!--return {x:1, y:2, z:3}-->
```

Web中移动可视区
```
window.scrollTo(0, 0)
```

#### 11-7
delete的用法
```
let a = {c:1,d:2}
delete a.c
a  //{d:2}
```

#### 11-16
远程分支有更新,本地查看不到可以git fetch

这将创建并更新本地远程分支。即创建并更新origin/xxx 分支，拉取代码到origin/xxx分支上。

H5中window.innerHeight高度会随着软键盘弹起而改变,所以有input元素的页面最好不要用行内样式style={{height:window.innerHeight},最好是在componentDidmount中设置好,后面就不会改变
```
    var hrt = document.documentElement.clientHeight; //获取当前可视区域的高度存到hrt变量
    window.onload = function() {
      //在页面整体加载完毕时
      document.getElementById('container').style.height = hrt + 'px'; //把获取到的高度赋值给根div
    };
```

#### 11-21

 let obj = Object.create(proxy):创建一个以proxy为原型的对象(obj)
 
#### 11-28
比较两个this
```
a = 2
let obj = {
    aaa: function() {
    	let timeout = setTimeout(function() {console.log(this.a)},1000)
    },
    a:1
}

obj.aaa() //2
```
定时器是挂载的window下面的 ,它的作用域是window
```
a = 2
let obj = {
    aaa: function() {
    	let timeout = setTimeout(() => {console.log(this.a)},1000)
    },
    a:1
}

obj.aaa() //1
```
所以要不用箭头函数,要不就保存外部的this -> self


#### 11-29
利用symbol实现私有变量
```
// 例子 4-3

const Example = (function() {
    var _private = Symbol('private');

    class Example {
        constructor() {
          this[_private] = 'private';
        }
        getName() {
          return this[_private];
        }
    }

    return Example;
})();

var ex = new Example();

console.log(ex.getName()); // private
console.log(ex.name); // undefined
```

for...of还可以这样使用
```
for (const [i, v] of ['a', 'b', 'c'].entries()) {
  console.log(i, v);
}
```

#### 11-30
“回文串”是一个正读和反读都一样的字符串，比如“level”或者“noon”等等就是回文串。

#### 12-25
Array的reduce()把一个函数作用在这个Array的[x1,x2,x3...]上，reduce()把结果继续和序列的下一个元素做累积计算，其效果就是：
```
[x1, x2, x3, x4].reduce(f) = f(f(f(x1, x2), x3), x4)

例如对Array求和
var arr = [1, 3, 5, 7, 9];
arr.reduce(function (x, y) {
    return x + y;
}); // 25
```

window的onload事件和domcontentloaded谁先谁后？

DOMContentLoaded然后才是onload的输出。所以说一般情况下，DOMContentLoaded事件要在window.onload之前执行，当DOM树构建完成的时候就会执行DOMContentLoaded事件。当window.onload事件触发时，页面上所有的DOM，样式表，脚本，图片，flash都已经加载完成了。

构造函数返回值

```
function Person(name) {
    this.name = name
    return name;
}
let p = new Person('Tom');
p //{name:'Tom'}

function Person(name) {
    this.name = name
    return {};
}
let p = new Person('Tom');
p //{}
```
构造函数不需要显示的返回值。使用new来创建对象(调用构造函数)时，如果return的是非对象(数字、字符串、布尔类型等)会忽而略返回值;如果return的是对象，则返回该对象(注：若return null也会忽略返回值）。
```
function F() {}
function O() {}

O.prototype = new F();
var obj = new O();

console.log(obj instanceof O); // true
console.log(obj instanceof F); // true
console.log(obj.__proto__ === O.prototype); // true
console.log(obj.__proto__.__proto__ === F.prototype); // true
```
实例的__proto__属性（原型）等于其构造函数的prototype属性

实现数组扁平化
```
function flatten(arr){
    while(arr.some(item=>Array.isArray(item)){
        arr = [].concat(...arr);
    }
    return arr;
}
```

#### 1-4
函数声明提升优先于变量声明提升
```
console.log(a) // ƒ a() {}
function a() {}
var a = 1
```

#### 1-17
正则限制输入为正整数
```
/^[0-9]*[1-9][0-9]*$/.test(value)
```

#### 1-18
#### event loop
js引擎遇到一个异步事件后并不会一直等待其返回结果，而是会将这个事件挂起，继续执行执行栈中的其他任务。当一个异步事件返回结果后，js会将这个事件加入与当前执行栈不同的另一个队列，我们称之为事件队列。被放入事件队列不会立刻执行其回调，而是等待当前执行栈中的所有任务都执行完毕， 主线程处于闲置状态时，主线程会去查找事件队列是否有任务。如果有，那么主线程会从中取出排在第一位的事件，并把这个事件对应的回调放入执行栈中，然后执行其中的同步代码...，如此反复，这样就形成了一个无限的循环。这就是这个过程被称为“事件循环（Event Loop）”的原因。

前面我们介绍过，在一个事件循环中，异步事件返回结果后会被放到一个任务队列中。然而，根据这个异步事件的类型，这个事件实际上会被对应的宏任务队列或者微任务队列中去。并且在当前执行栈为空的时候，主线程会 查看微任务队列是否有事件存在。如果不存在，那么再去宏任务队列中取出一个事件并把对应的回到加入当前执行栈；如果存在，则会依次执行队列中事件对应的回调，直到微任务队列为空，然后去宏任务队列中取出最前面的一个事件，把对应的回调加入当前执行栈...如此反复，进入循环。

```
console.log('script start')

async function async1() {
  await async2()
  console.log('async1 end')
}
async function async2() {
  console.log('async2 end')
}
async1()

setTimeout(function() {
  console.log('setTimeout')
}, 0)

new Promise(resolve => {
  console.log('Promise')
  resolve()
})
  .then(function() {
    console.log('promise1')
  })
  .then(function() {
    console.log('promise2')
  })

console.log('script end')
// script start => async2 end => Promise => script end => promise1 => promise2 => async1 end => setTimeout
```

#### 1-22
扩展运算符只会复制可枚举属性(浅复制),继承属性即使是可枚举的，也会被忽略.

#### 1-24
[git merge --no--ff](https://blog.csdn.net/chaiyu2002/article/details/81020370)

#### 2-13
js对超过16位数字处理的精度会存在偏差,比如
```
parseInt(12345678901234567) //12345678901234568
```
所以最好用字符串来传递,如果涉及到计算,若前面位数一致,可以slice之后再对后面不同的部分(小于17位)计算

#### 2-14
所有的引用类型(数组、函数、对象)都具有对象特性，即可自由扩展属性.
因此也可以给函数添加属性

#### 2-28
使用CSS module覆写ANTD样式
```
import styles from './index.less';
...
<List.Item className={styles.switchList}>
...
```
```
<!--index.less-->
.switchList {
  :global {
    .ant-list-item-meta {
      flex-grow: 0;
      flex-basis: 150px;
    }
    .ant-list-item-content {
      flex-grow: 0;
    }
  }
}
```
外层包装了一层CLASSNAME这样可以在本组件覆写ANTD样式同时也不会影响其它地方的默认样式

#### 3-4

setState的同步和异步

在 React 中，如果是由 React 引发的事件处理（比如通过 onClick 引发的事件处理），调用 setState 不会同步更新 this.state，除此之外的 setState 调用会同步执行 this.state。所谓“除此之外”，指的是绕过 React 通过 addEventListener 直接添加的事件处理函数，还有通过 setTimeout/setInterval 产生的异步调用。


原因：在 React 的 setState 函数实现中，会根据一个变量 isBatchingUpdates 判断是直接更新 this.state 还是放到队列中回头再说，而 isBatchingUpdates 默认是 false，也就表示 setState 会同步更新 this.state，但是，有一个函数 batchedUpdates，这个函数会把 isBatchingUpdates 修改为t rue，而当 React 在调用事件处理函数之前就会调用这个 batchedUpdates，造成的后果就是由 React 控制的事件处理过程 setState 不会同步更新 this.state。

```
class Example extends React.Component {
  constructor() {
    super();
    this.state = {
      val: 0
    };
  }
  
  componentDidMount() {
    this.setState({val: this.state.val + 1});
    console.log(this.state.val);    // 第 1 次 log

    this.setState({val: this.state.val + 1});
    console.log(this.state.val);    // 第 2 次 log

    setTimeout(() => {
      this.setState({val: this.state.val + 1});
      console.log(this.state.val);  // 第 3 次 log

      this.setState({val: this.state.val + 1});
      console.log(this.state.val);  // 第 4 次 log
    }, 0);
  }

  render() {
    return null;
  }
};
<!--输出： 0 0 2 3-->
```

解析：
1、第一次和第二次都是在 react 自身生命周期内，触发时 isBatchingUpdates 为 true，所以并不会直接执行更新 state，而是加入了 dirtyComponents，所以打印时获取的都是更新前的状态 0。
2、两次 setState 时，获取到 this.state.val 都是 0，所以执行时都是将 0 设置成 1，在 react 内部会被合并掉，只执行一次。设置完成后 state.val 值为 1。
3、setTimeout 中的代码，触发时 isBatchingUpdates 为 false，所以能够直接进行更新，所以连着输出 2，3。

---

判断类型除了instanceof和Array.isArray(),还可以用Object.prototype.toString.call()

```
const an = ['Hello','An'];
an.toString(); // "Hello,An"
Object.prototype.toString.call(an); // "[object Array]"


Object.prototype.toString.call('An') // "[object String]"
Object.prototype.toString.call(1) // "[object Number]"
Object.prototype.toString.call(Symbol(1)) // "[object Symbol]"
Object.prototype.toString.call(null) // "[object Null]"
Object.prototype.toString.call(undefined) // "[object Undefined]"
Object.prototype.toString.call(function(){}) // "[object Function]"
Object.prototype.toString.call({name: 'An'}) // "[object Object]"

```