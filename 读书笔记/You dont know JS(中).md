
# you don't know js (中卷)

#### 类型和值

js中没有指针,变量不可能成为指向另一个变量的引用.

简单值总是通过值复制的方式来赋值/传递,复合值(对象)则总是通过引用复制的方式来赋值/传递,并且引用指向的是值本身而非变量.
```
var a = 2;
var b = a;
b++;
a //2
b //3

var c = [1,2]
var d = c;
d.push(3)
c // [1,2,3]
d // [1,2,3]

//但是一个引用无法更改另一个引用的指向,如下

d = [4,5,6]
d //[4,5,6]
c //[1,2,3]
```
原生函数可以被当做构造函数来使用,此时创建的是字符串"abc"的封装对象,而非基本类型值"abc"
```
var a = new String("abc")
var b = String("abc")
a //String {"abc"} 0: "a"1: "b"2: "c"length: 3__proto__:String[[PrimitiveValue]]: "abc"
b // "abc"

```

### ES6 Set

```
Set的属性：
size：返回集合所包含元素的数量

Set的方法：

操作方法

add(value)：向集合添加一个新的项
delete(value)：从集合中移除一个值
has(value)：如果值在集合中存在，返回true,否则false
clear(): 移除集合里所有的项


遍历方法

keys()：返回一个包含集合中所有键的数组
values()：返回一个包含集合中所有值的数组
entries：返回一个包含集合中所有键值对的数组(感觉没什么用就不实现了)
forEach()：用于对集合成员执行某种操作，没有返回值
```

## 异步
setTimeout(1000)并不能保证程序在1000秒后马上执行,只是在1000秒后将其加入事件循环.真正执行时间总是>1000ms

#### 线程和进程
进程和线程独立运行,并可能同时运行,但多个线程能够共享单个进程的内存

---

```
setTimeout(..0) //相当于"把这个函数插入到当前时间循环队列的结尾处"
```
利用以上特性可以实现"并发协作",例如有100万个队列要处理,可以先分块,然后setTimeout(..0)将下一块放入下个事件队列中

#### 任务和事件循环
事件循环每一轮称为一个tick,一次只能从队列中处理一个事件(即使是并发的情况也是有顺序的.)
任务队列会插在下一个事件循环开始之前完成,Promise就是基于任务的,而不是事件循环.

### 回调问题
1.顺序性 假设短时间内发出几个请求,回调的顺序可能会不同,后面的回调会覆盖前面的回调,但后面的回调却有可能是先发出的请求的回调(考虑保护神产品页选产品情况)
2.可信任度 假设使用的是第三方非开源的API,那么回调可能不受自己控制

Promise改进了回调

## Promise 

#### 改进问题
1. 回调用过早和过晚(可能导致竞态条件)的问题:回调会立即调用回调函数.而promise不管是同步或是异步的,因为then的机制,总会被异步调用(即使是立即完成的promise也无法被同步观察到).一个Promise决议后,其所有通过then(..)注册的回调都会在**下一个**异步时机点上被立即调用
考虑下面代码
```
p.then(() => {
    p.then( () => {
        console.log('c')
    })
    console.log('a')
})
p.then(() => {
    console.log('b')
})
// a b c
```
2. 回调次数过多的问题:promise只能被resolve()或reject()决议1次,所以任何通过then()注册的回调就只会被调用1次,所以哪怕抽风了resolve()了N次,也只执行一次的结果.除非自己挖坑注册了多个(如p.then(f1);p.then(f2))

#### promise细节
1. resolve()只能传递一个参数,因此最好把参数包在数组或对象中;
2. Promise.resolve()总会返回一个promise结果.
```
以下两种情况等价;
let p1 = new Promise((resolve,reject) => {
   resolve(42);
});
let p2 = Promise.resolve(42);

如果直接传递promise进去,这返回本身
let p3 = Promise.resolve(p2);
p2 === p3 //true
```
因此在不确定then(..)的源头时,最好用Promise.resolve()封装一层,保证其是一个可信任的行为良好的Promise
3. 注意then(f1,f2)参数接收的是两个的函数(我们平时一般就用第一个,如果省略或传任何非函数值,则默认return上一步resolve的值,第二个不传则默认throw err,这个err可以继续沿着链传播下去),then如果不显式返回一个值,则会隐式返回undefined.
```
p.then(null) 
//等同于
p.then(v => v, err => {throw err})
```
```
p1.then(return 2).then(v => console.log(v)); //报错,不接受一段语句
```
4. promise穿透
```
p1.then(Promise.resolve(2)).then(v => console.log(v)); //42,不是2,因为链式流是基于p1的
```

#### try..catch
try..catch只能捕捉同步的错误(除非用例如yield的异步流程控制)

#### 错误捕捉风格
1.error-first回调风格
```
function(err,val) {
    if(err) {
        ...
    } else {
        console.log(val);
    }
};
```
2.promise的分离回调风格
```
p.then(v => v, err => {throw err})
```

#### promise.all([])

等待数组所有promise完成才响应.
promise.all([同步,异步]),可以将同步的结果延迟回调

#### promise.race([])

promise.race([p1,p2]),p1和p2只有一个能够取胜,即响应"第一个跨过终点线的promise"
应用:
1.超时竞赛:假设p2会在3秒后reject,则p1必须在3秒内响应才能成功.

#### promise局限性
1. 
```
let p = foo(42).then(f1).then(f2);
```
这里是指向链中最后一个,即then(f2);可以给p注册错误处理函数p.catch(f)

但是promise没有指向链中的引用.如果链中也有错误处理函数,那么错误就不会简单地传到尾端的catch()上


## 生成器generator

*foo(..)并不会执行,而是创建一个迭代器对象.

一般来说,next()调用要比yield多一个.

启动生成器时一定要用不带参数的next()

.next()一般返回对应yield的值
```
function *foo(x) {
    let y = x * (yield "Hello");
    return y;
}
let it = foo(6); //构建迭代器
let res = it.next(); //启动迭代器
res.value //"Hello"; //.value总是输出yield后表达式的值

res = it.next(7);  //注意,这里将(yield "Hello")部分替换成7,前者整体相当于参数了
res.value //42
```

如果生成器没有显示的return,则会有一个隐式的return(undefined)

可以通过for..of循环来自动迭代标准迭代器,不过要注意不要死循环.for..of循环会在每次迭代中自动调用next(),不会向next()传任何值,并且在接收到done:true后自动停止.
通过for..of来遍历数组值也是属于这种模式

### 异步迭代生成器

```
function *main() {
    try {
        var text = yield foo() //foo是ajax/fetch异步获取数据
        console.log(text)  //如果不用yield,这里就不会正确输出
    } catch (err) {
        console.log(err);
    }
}

var it = main(); //构建生成器
it.next(); //启动;
```
上述看似阻塞同步的代码,因为生成器的原因并没有阻塞,同时也使得try..catch能够捕捉异步错误




