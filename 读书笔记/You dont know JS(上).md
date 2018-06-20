# you don't know js (上卷)

#### LHS和RHS查询
如果查找的目的是对变量进行赋值--LHS
如果目的是获取变量的值--RHS
RHS查询在作用域找不到所需的变量,引擎会抛出ReferenceError.LHS则会创建具有该名称的变量(非严格模式下,严格模式下也会报ReferenceError)


---


IIFE:立即执行的函数表达式
var 没有块级作用域的概念,let有.for循环头部的let,每次迭代都会声明,而var只声明一次

```
for (var i=1;i<=3;i++){
	console.log(1)
	setTimeout(function timer(){
		console.log(2);
	},0)
}
// 1 1 1 2 2 2

for (var i=1;i<=3;i++){
	setTimeout(function timer(){
		console.log(i);
	},0)
}
// 4 4 4 

for (let i=1;i<=3;i++){
    console.log(1)
	setTimeout(function timer(){
		console.log(i);
	},0)
}
// 1 1 1 1 2 3
```
setTimeout将代码放入下一个事件循环里,等待本轮代码执行完毕后才开始执行下一轮代码,因此for循环中的console.log(1)会先全部执行完毕.

(2)用var的情况下,全局的i等于4,最后3个setTimeout一起输出4.而(3)let的情况下,每次循环都创建了块级作用域,每个setTimeout输出的都是块级作用域里的i,即1,2,3

#### 模块模式(利用闭包)

```
function CoolModule() {
    var something = 'cool';
    var another = [1,2,3];
    function doSomething() {
        console.log(something)
    }
    function doAnother() {
        console.log( another.join("!"));
    }
    
    return{
        doSomething,
        doAnother
    }
}

var foo = CoolModule();

foo.doSomething(); //cool
foo.doAnother(); // 1!2!3
```
模块模式运用了闭包的特性.


模块模式两个必要条件:1.必须有外部封闭函数并被调用一次(用于创建新的模块实例)  2.封闭函数必须至少返回一个内部函数,这样内部函数才能在私有作用域中形成闭包,并且可以访问或修改私有的状态

模块模式可以创建任意多个实例

#### 单例模式
将上述CoolModule()变成IIFE就是一个单例模式
```
var foo = (function CoolModule(){
    ...
})()
foo.doSomething(); //cool
foo.doAnother(); // 1!2!3
```
#### ES6的模块模式
每个JS文件都是一个独立的模块,通过import/module引入,export导出。这种情况下不用外层封闭函数,return也由export代替
import和module的区别:import是将export导出的标识符绑定在变量上,module会将整个模块的API导出并绑定在一个变量上.

#### this和context
this是隐式传递一个对象的引用,context是显式
```
function identify() {
    return this.name
}

function identify(context){
    return context.name
}

```
在"use strict"模式下,全局作用域下函数的this不会默认绑定到全局对象上
```
function foo() {
    "use strict"
    console.log(this.a)
}
var a = 2;
foo //TypeError:this is undefined
```

#### 错误类型:TypeError和ReferenceError
ReferenceError:对未声明的标识符进行函数调用

TypeError:对声明过但未赋值的标识符(undefined)进行函数调用

(上述是针对变量标识符.当调用对象的属性时,即便对象中没有该属性,也是返回undefine,不会报错)
```
var oa={
    a:1
}
console.log(oa.b) //undefined
```

#### this绑定顺序优先级
new > call/apply(bind) > 隐式绑定(对象调用) > 默认绑定(全局)

*箭头函数的this绑定凌驾于这些之上,是根据当前的词法作用域来决定this(即继承外层函数的this)

#### 利用bind进行科里化
```
function foo(a,b){
    console.log(`a:${a},b:${b}`)
}
var bar = foo.bind(null,2)
bar(3) //a:2,b:3    
```

---

赋值表达式的返回值是目标函数的引用,
```
function foo(a){
    console.log(this.a)
}
var p = {a:3}
var o = {a:4,foo}
var a = 2
(p.foo = o.foo)() //2
```
#### 属性描述符的使用
```
var myObject = {};
Object.defineProperty(myObject,'a',{
    value:2,
    writable:true,
    configurable:true,
    enumerable:true
})
```

#### in操作符
in操作符检测的是属性名是否在对象**及其原型链**中,不是属性值
```
'a' in {a:1,b:2} //true
4 in [2,4,6] //false
1 in [2,4,6] //true
```
#### for ... in 循环和Object.keys()
for ... in 循环可以列出原型链上的属性,无法列出不可枚举的属性,但in操作符可以检测不可枚举的属性是否在对象中.

Object.keys()也是返回一个包含所有可枚举属性的**数组**,但无法返回原型链上的属性


功能|for ... in | Object.keys()
---|---|---
是否能列举原型上的属性|√ | ×
是否能列出不可枚举的属性|× | ×



另外,千万不要用for ... in 遍历数组.

#### 迭代器比较
forEach():遍历数组中所有的**值**,并**忽略回调函数的返回值**

every():会一直运行,直到回调函数返回false(或假值,如0)

some():会一直运行,直到回调函数返回true(或真值,如1)

#### 类

##### 多态:(狭义)用super引用继承自父类的原始方法的技术

##### 混入(mixin):JS中用来模拟类的复制行为的方法
```
mixin(sourceObj, targetObj) {
    for(var key in sourceObj) {
        if(!(key in targetObj)) {
            targetObj[key] = sourceObj[key];
        }
    }
    return targetObj;
}
```

## 原型
ES6之前没有类的定义,只有"**对象**",是用原型继承来模拟类

隐式屏蔽
```
var anotherObj = {a:2}
var myObj = Object.create(anotherObj)
myObj.hanOwnproperty('a') //false
myObj.a++ //隐式屏蔽
myObj.hanOwnproperty('a') //true
```
避免这种方式的办法就是直接对anotherObj.a++

#### constructor
```
function Foo() {
    // ...
}
Foo.prototype.constructor === Foo; //true
var a = new Foo();
a.constructor === Foo; //true

```
其实a没有constructor属性,而是调用了Foo.prototype的constructor属性,这个属性指向构造函数Foo;但若显示的改写了Foo.prototype = {} 等于一个新对象,则Foo.prototype会失去constructor这个属性.这个查找链会委托给顶端的Object.prototype.

因此constructor这个属性太过"随意",尽量避免使用.

#### [[prototype]]（隐式原型）与prototype（显式原型）
[[prototype]]是对象内置的属性,prototype是函数内置的属性.

隐式原型[[prototype]]指向创建这个对象的函数(constructor)的prototype








