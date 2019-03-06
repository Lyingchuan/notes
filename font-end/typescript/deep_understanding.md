## Readonly
这有一个 Readonly 的映射类型，它接收一个泛型 T，用来把它的所有属性标记为只读类型：
```
type Foo = {
  bar: number;
  bas: number;
};

type FooReadonly = Readonly<Foo>;

const foo: Foo = { bar: 123, bas: 456 };
const fooReadonly: FooReadonly = { bar: 123, bas: 456 };

foo.bar = 456; // ok
fooReadonly.bar = 456; // Error: bar 属性只读
```
你甚至可以把索引签名标记为只读：
```
interface Foo {
  readonly [x: number]: number;
}

// 使用

const foo: Foo = { 0: 123, 2: 345 };
console.log(foo[0]); // ok（读取）
foo[0] = 456; // Error: 属性只读
```
如果你想以不变的方式使用原生 JavaScript 数组，可以使用 TypeScript 提供的 ReadonlyArray<T> 接口：
```
const foo: ReadonlyArray<number> = [1, 2, 3];
console.log(foo[0]); // ok

foo.push(4); // Error: ReadonlyArray 上不存在 `push`，因为他会改变数组

foo = foo.concat(4); // ok, 创建了一个复制
```

#### 注意
```
const foo: {
  readonly bar: number;
} = {
  bar: 123
};

function iMutateFoo(foo: { bar: number }) {  //这里没有定义readonly
  foo.bar = 456;
}

iMutateFoo(foo);
console.log(foo.bar); // 456
```
readonly 能确保“我”不能修改属性，但是当你把这个属性交给其他并没有这种保证的使用者（允许出于类型兼容性的原因），他们能改变它。当然，如果 iMutateFoo 明确的表示，他们的参数不可修改，那么编译器会发出错误警告：
```
interface Foo {
  readonly bar: number;
}

let foo: Foo = {
  bar: 123
};

function iTakeFoo(foo: Foo) { // 这里定义了readonly
  foo.bar = 456; // Error: bar 属性只读
}

iTakeFoo(foo);
```