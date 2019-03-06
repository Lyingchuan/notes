
#### 数组类型
```
let list: number[] = [1, 2, 3];

//第二种方式是使用数组泛型

let list: Array<number> = [1, 2, 3];
```

#### 元组类型
元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 string和number类型的元组。
```
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

---

如果要传额外的未定义的参数可以用

1.类型断言
```
interface SquareConfig {
    color?: string;
    width?: number;
}
//opacity未定义
let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);
```
2.添加一个字符串索引签名
```
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}
```

---
#### 枚举
数字枚举
```
enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}
```
不断递增(Down = 2,Left = 3, Right = 4)
```
enum Direction {
    Up,
    Down,
    Left,
    Right,
}
```
不定义初始值,则从0开始(Up = 0, Down = 1)

字符串枚举
```
enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT",
}
```
枚举可以反向映射

#### 模块
外部模块

在Node.js里大部分工作是通过加载一个或多个模块实现的。 我们可以使用顶级的 export声明来为每个模块都定义一个.d.ts文件，但最好还是写在一个大的.d.ts文件里。 我们使用与构造一个外部命名空间相似的方法，但是这里使用 module关键字并且把名字用引号括起来，方便之后import

```
declare module "url" {
    export interface Url {
        protocol?: string;
        hostname?: string;
        pathname?: string;
    }

    export function parse(urlStr: string, parseQueryString?, slashesDenoteHost?): Url;
}
```
引入
```
import * as URL from "url";
let myUrl = URL.parse("http://www.typescriptlang.org");
```
假如你不想在使用一个新模块之前花时间去编写声明，你可以采用声明的简写形式以便能够快速使用它。简写模块里所有导出的类型将是any。
```
declare module "hot-new-module";
```

#### 断言
我们确实需要在还不确定类型的时候就访问其中一个类型的属性或方法，比如：
```
function getLength(something: string | number): number {
    if (something.length) {
        return something.length;
    } else {
        return something.toString().length;
    }
}
//ERROR
```
此时可以使用类型断言，将 something 断言成 string
```
function getLength(something: string | number): number {
    if ((<string>something).length) {
        return (<string>something).length;
    } else {
        return something.toString().length;
    }
}
```
类型断言不是类型转换，断言成一个联合类型中不存在的类型是不允许的：
```
function toBoolean(something: string | number): boolean {
    return <boolean>something;
}
//ERROR
```

#### 泛型
##### 泛型变量
```
function identity<T>(arg: T): T {
    return arg;
}

let output = identity<string>("myString")
//或不指定,自动识别
let output = identity("myString") 
```
##### 泛型接口
```
interface GenericIdentityFn {
    <T>(arg: T): T;
}
let myIdentity: GenericIdentityFn = identity;
```
```
//或将泛型作为参数
interface GenericIdentityFn<T> {
    (arg: T): T;
}
let myIdentity: GenericIdentityFn<number> = identity;
```
##### 泛型类
```
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```
类有两部分：静态部分和实例部分。泛型类指的是实例部分的类型，所以类的静态属性不能使用这个泛型类型
##### 泛型约束
```
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);  // Now we know it has a .length property, so no more error
    return arg;
}
```


##### Exclude<T,U>
从 T 中排除那些可以赋值给 U 的类型。
```
type T = Exclude<1|2|3|4|5, 3|4>  // T = 1|2|5 
```
此时 T 类型的值只可以为 1 、2 、 5 ，当使用其他值是 TS 会进行错误提示。

### 高级类型
#### 索引类型
```
function pluck<T, K extends keyof T>(o: T, names: K[]): T[K][] {
  return names.map(n => o[n]);
}

interface Person {
    name: string;
    age: number;
}
let person: Person = {
    name: 'Jarid',
    age: 35
};
let strings: string[] = pluck(person, ['name']); // ok, string[]
```
keyof T， 索引类型查询操作符。 对于任何类型 T， keyof T的结果为 T上已知的公共属性名的联合。

T[K]， 索引访问操作符,只要确保类型变量 K extends keyof T就可以了

#### 映射类型
一个常见的任务是将一个已知的类型每个属性都变为可选的,或者只读版本
```
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
}
type Partial<T> = {
    [P in keyof T]?: T[P];
}
```
像下面这样使用可以将Person类型转变为可选的或只读的新类型
```
type PersonPartial = Partial<Person>;
type ReadonlyPerson = Readonly<Person>;
```