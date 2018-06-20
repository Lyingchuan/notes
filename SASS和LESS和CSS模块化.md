sass是基于Ruby的，是在服务器端处理的。很多开发者不选择LESS是因为LESS输出修改过的CSS到浏览器需要依赖于Javascript引擎，而Javascript引擎需要额外的时间来处理代码

#### 变量
```
@blue: #00c;/*定义蓝色变量*/
@light_blue: @blue + #333;/*定义浅蓝色变量*/
@dark_blue: @blue - #333;/*定义深蓝变量*/
```

#### Mixin
LESS
```
.border {
  border-top: 1px dotted #333;
}

article.post {
  background: #eee;
  .border;
}

ul.menu {
  background: #ccc;
  .border;
}
```
SASS要加@mixin,引入时要加@include
```
@mixin border {
  border-top: 1px dotted #333;
}

article.post {
  background: #eee;
  @include border;
}

ul.menu {
  background: #ccc;
  @include border;
}
```
#### 选择器继承(仅SASS)
```
.menu {
  border: 1px solid #ddd;
}
.footer {
  @extend .menu;
}
/* 上面的规则和下面的规则是一样的效果 */
.menu, .footer {
  border: 1px solid #ddd;
}

```

#### 命名空间
```
#defaults {
  .nav_list () {
    list-style: none;
    margin: 0; padding: 0;
   }
   .button () { … }
   .quote () { … }
}


---


nav ul {
  #defaults > .nav_list;
}
```

#### 作用域
```
@color: #00c; /* 蓝色 */

#header {
  @color: #c00; /* 红色 */
   border: 1px solid @color; /* 红色边框 */
}

#footer {
  border: 1px solid @color; /*蓝色边框 */
}
```

#### 字符串插入
```
@base_url = 'http://coding.smashingmagazine.com';
background-image: url("@{base_url}/images/background.png");
```

# CSS模块化
```
.normal {
  color: green;
}

/* 以上与下面等价 */
:local(.normal) {
  color: green; 
}

/* 定义全局样式 */
:global(.btn) {
  color: red;
}

/* 定义多个全局样式 */
:global {
  .link {
    color: green;
  }
  .box {
    color: yellow;
  }
}
/* 样式复用 */
.aaa{
    composes:normal;
}
```
CSS模块化能解决全局样式冲突的问题.
但1.className只能唯一,不能像style={[styles.a,styles.b]}用两个类名
2.因为生成的className是含哈希值的,所以无法getElementByClassName()获取到,办法是用上面提到过的:global方法,然后用className=''正常引入,这样css类名不含哈希值,但却变成全局变量,尝试用ID?