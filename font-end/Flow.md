# Flow和typescript
TypeScript 实现了类型检查，同时实现了转译工具用来生成纯粹的 JavaScript 。Flow 只进行类型检查，并依赖Babel、flow-remove-types或者其它工具来移除类型说明,就是说，你不能在浏览器或像 Node 之类的其它 JavaScript 环境直接运行那段代码


## Numbers 类型
NaN和Infinity也属于number

## String 类型

```
// @flow
"foo" + String({});     // Works!
"foo" + [].toString();  // Works!
"" + JSON.stringify({}) // Works!
```

## Null和void
null对应null,void对应undefine

## Maybe type
"?type"的模式可以通过null或undefine

```
// @flow
function acceptsMaybeString(value: ?string) {
  // ...
}

acceptsMaybeString("bar");     // Works!
acceptsMaybeString(undefined); // Works!
acceptsMaybeString(null);      // Works!
acceptsMaybeString();          // Works!
```
## Optional object properties
这种情况不能为null

```
{ propertyName?: string }

// @flow
function acceptsObject(value: { foo?: string }) {
  // ...
}

acceptsObject({ foo: "bar" });     // Works!
acceptsObject({ foo: undefined }); // Works!
acceptsObject({ foo: null });      // Error!
acceptsObject({});                 // Works!
```

## Optional function parameters
可选函数参数,同上,不能为NULL

