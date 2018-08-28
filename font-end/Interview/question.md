
### 1.求a1的值? 
```
const a1 = [{a:1,b:2}, 2];
const a2 = a1.concat();
a2[1] = 1
a2[0].a = 2;

// a1 = [{a:2,b:2},2]
```
类似于Object.assign,concat(),[...arr2] = arr1等拷贝方法只是第一层深拷贝(基本类型如字符串,数字),里面引用类型的属性属于更深层次,因此对象的指针还是指向同一位置.要实现这种深拷贝可以考虑用FOR循环或者如下的封装函数

```
const deepClone = obj => {
  let clone = Object.assign({}, obj);
  Object.keys(clone).forEach(
    key => (clone[key] = typeof obj[key] === 'object' ? deepClone(obj[key]) : obj[key])
  );
  return clone;
};
```
或者可以用下列方法实现深拷贝,因为将JSON字符串化时已经解除了引用,再解析时就是一个新的对象
```
var newObj = JSON.parse(JSON.stringify(someobj))
```
但这种情况
1. 会忽略 undefined
2. 不能序列化函数
3. 不能解决循环引用的对象
```
let a = {
    age: undefined,
    jobs: function() {},
    name: 'yck'
}
let b = JSON.parse(JSON.stringify(a))
console.log(b) // {name: "yck"}
```

### 2.promise
```
const first = () => (new Promise((resovle,reject)=>{
    console.log(3);
    let p = new Promise((resovle, reject)=>{
         console.log(7);
        setTimeout(()=>{
           console.log(5);
           resovle(6); 
        },0)
        resovle(1);
    }); 
    resovle(2);
    p.then((arg)=>{
        console.log(arg);
    });

}));

first().then((arg)=>{
    console.log(arg);
});
console.log(4);

//(一轮)3、7、4、(一轮尾任务)1、2、(二轮)5。

```
### 3.还是promise
```
Promise.resolve(1).then(Promise.resolve(2)).then((v) => {
  console.log(v)
}) 

// 1

Promise.resolve(1).then(return Promise.resolve(2)).then((v) => {
  console.log(v)
})

// error

Promise.resolve(1).then(null).then((v) => {
  console.log(v)
})

// 1

Promise.resolve(1).then(return 2).then((v) => {
  console.log(v)
})

// error

Promise.resolve(1).then(() => {
  return 2
}).then((v) => {
  console.log(v)
})

//2
```

### 4.async await
```
setTimeout(() => {console.log('111111111111111111111')},0)
//注意,这里没用async声明函数
function testSometing() {
    console.log("testSomething");
    return "return testSomething";
}

async function testAsync() {
    console.log("testAsync");
    return Promise.resolve("hello async");
}

async function test() {
    console.log("test start...");

    const testFn1 = await testSometing();
    console.log(testFn1);

    const testFn2 = await testAsync();
    console.log(testFn2);

    console.log('test end...');
}

test();

var promiseFn = new Promise((resolve)=> { 
                    console.log("promise START...");
                    resolve("promise RESOLVE");
                });
promiseFn.then((val)=> console.log(val));

console.log("===END===")

```
输出
```
<!-- 第一轮事件循环-->
    test start...
    testSomething
    promise START...
    ===END===
    <!--promise的任务开始执行-->
        <!--第一轮任务-->
            return testSomething
            testAsync
            
        <!--第二轮任务-->
            promise RESOLVE
            
        <!--第三轮任务-->
            hello async
            test end...

<!-- 第二轮事件循环-->
111111111111111111111
```
