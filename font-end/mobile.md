### 基本概念
1. ##### 物理像素(physical pixel):
    物理像素又被称为设备像素，他是显示设备中一个最微小的物理部件。每个像素可以根据操作系统设置自己的颜色和亮度。正是这些设备像素的微小距离欺骗了我们肉眼看到的图像效果。
2. ##### 设备独立像素(density-independent pixel)
    设备独立像素也称为密度无关像素，可以认为是计算机坐标系统中的一个点，这个点代表一个可以由程序使用的虚拟像素(比如说CSS像素)，然后由相关系统转换为物理像素

3. ##### CSS像素
    CSS像素是一个抽像的单位，主要使用在浏览器上，用来精确度量Web页面上的内容。一般情况之下，CSS像素称为与设备无关的像素(device-independent pixel)，简称DIPs。

4. ##### 屏幕密度
    屏幕密度是指一个设备表面上存在的像素数量，它通常以每英寸有多少像素来计算(PPI)。

5. ##### 设备像素比(device pixel ratio)
    设备像素比简称为dpr，其定义了物理像素和设备独立像素的对应关系。它的值可以按下面的公式计算得到：设备像素比 ＝ 物理像素 / 设备独立像素

    在JavaScript中，可以通过window.devicePixelRatio获取到当前设备的dpr。

iPhone6的设备宽度和高度为375pt*667pt,可以理解为设备的独立像素；而其dpr为2，根据上面公式，我们可以很轻松得知其物理像素为750pt * 1334pt



### 如何做横屏适配
对屏幕 resize 事件进行监听，当判断为竖屏时将整个根容器进行逆时针 CSS3 旋转 90 度
```
var detectOrient = function() {
  var width = document.documentElement.clientWidth,
      height =  document.documentElement.clientHeight,
      $wrapper =  document.getElementById("J_wrapper"),
      style = "";
  if( width >= height ){ // 横屏
      style += "width:" + width + "px;";  // 注意旋转后的宽高切换
      style += "height:" + height + "px;";
      style += "-webkit-transform: rotate(0); transform: rotate(0);";
      style += "-webkit-transform-origin: 0 0;";
      style += "transform-origin: 0 0;";
  }
  else{ // 竖屏
      style += "width:" + height + "px;";
      style += "height:" + width + "px;";
      style += "-webkit-transform: rotate(90deg); transform: rotate(90deg);";
      // 注意旋转中点的处理
      style += "-webkit-transform-origin: " + width / 2 + "px " + width / 2 + "px;";
      style += "transform-origin: " + width / 2 + "px " + width / 2 + "px;";
  }
  $wrapper.style.cssText = style;
}
window.onresize = detectOrient;
detectOrient();
```
### VW
vw 是什么？
vw 是基于视图(Viewport)的长度单位，而与Viewport相关四个单位有：

vw：Viewport's Width 简写，1vw 等于 window.innerWidth 的 1%
vh：Viewport's Height 简写，1vh 等于 window.innerHeight 的 1%
vmin：当前 vw 和 vh 中较小值
vmax：当前 vw 和 vh 中较大值
![image](https://user-gold-cdn.xitu.io/2018/8/4/16504545961c08db?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

为了保证在低版本的机型也能正常显示页面，可以引入 viewport-units-polyfill 来处理 vw 的兼容问题。