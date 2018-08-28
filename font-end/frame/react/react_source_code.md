每个组件其实是一个对象,而不是真实的DOM.
初始时props是空对象
```
<A><div>这是A组件</div></A>
```
此时A对象的props中增加了children属性,如果我们进行多层的组件嵌套，其实就是在父对象的props中增加children字段及对应的描述值，也就是js对象的多层嵌套。

![image](https://user-gold-cdn.xitu.io/2017/9/26/f6359a8ab57b8e1bf65df837c91bb9a5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

关于ReactComponent
![image](https://user-gold-cdn.xitu.io/2018/2/28/161db4dcee7ff574?imageslim)
通过执行React.createElement创建出的ReactElement类型的js对象，就是"React组件"
