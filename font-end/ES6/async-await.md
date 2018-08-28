async 函数是 Generator 函数的语法糖。使用 关键字 async 来表示，在函数内部使用 await 来表示异步。相较于 Generator，async 函数的改进在于下面四点：
1. 内置执行器。Generator 函数的执行必须依靠执行器，而 async 函数自带执行器，调用方式跟普通函数的调用一样
2. 更好的语义。async 和 await 相较于 * 和 yield 更加语义化
3. 更广的适用性。co 模块约定，yield 命令后面只能是 Thunk 函数或 Promise对象。而 async 函数的 await 命令后面则可以是 Promise 或者 原始类型的值（Number，string，boolean，但这时等同于同步操作）
4. 返回值是 Promise。async 函数返回值是 Promise 对象，比 Generator 函数返回的 Iterator 对象方便，可以直接使用 then() 方法进行调用

---

正常情况下，await 命令后面跟着的是 Promise ，如果不是的话，也会被转换成一个 立即 resolve 的 Promise。

async里如果有多个await函数的时候，如果其中任一一个抛出异常或者报错了，都会导致函数停止执行，直接reject;

举例
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
setTimeout里的console会在下一轮事件循环中输出,即最后才输出.
promise是基于任务的,任务会在当前事件循环末尾(下一个事件循环之前)执行.await返回的是promise,因此会在**本轮**事件循环末尾执行,async函数中await后面的代码会暂停,跳出async函数先执行后面代码.

如果testSometing用async来声明,则"promise RESOLVE"会提前到"return testSomething"前面,因为此时又创建了一个等待被resolve的promise →"return testSomething",其在任务队列中的位置在promiseFn之后.