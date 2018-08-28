ES5构造函数

```
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);

p.__proto__ === Point.prototype //true
```

用ES6 class来表示


```
class Point {
  constructor(x, y) {
    this.x = x; //类的方法内部如果含有this，它默认指向类的实例
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}

var p = new Point(1,2)

p.toString()
```
ES5 的构造函数Point，对应 ES6 的Point类的构造方法(constructor)。


```
class Point {
  constructor() {
    // ...
  }

  toString() {
    // ...
  }

  toValue() {
    // ...
  }
}

// 等同于

Point.prototype = {
  constructor() {},
  toString() {},
  toValue() {},
};

typeof Point // "function"
Point === Point.prototype.constructor // true
//在类的实例上面调用方法，其实就是调用原型上的方法。

let p = new Point()

p.constructor === p.prototype.constructor // true
//类的数据类型就是函数，类本身就指向构造函数。

```
类的内部所有定义的方法，都是不可枚举的

```
class Point {
  constructor(x, y) {
    // ...
  }

  toString() {
    // ...
  }
}

Object.keys(Point.prototype)
// []
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```
实例的属性除非显式定义在其本身（即定义在this对象上），否则都是定义在原型上（即定义在class上）

```
class Point {

  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }

}

var point = new Point(2, 3);

point.hasOwnProperty('x') // true
point.hasOwnProperty('y') // true
point.hasOwnProperty('toString') // false
point.__proto__.hasOwnProperty('toString') // true
```
上面代码中，x和y都是实例对象point自身的属性（因为定义在this变量上），所以hasOwnProperty方法返回true，而toString是原型对象的属性（因为定义在Point类上），所以hasOwnProperty方法返回false


## class的静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```
上面代码中，Foo类的classMethod方法前有static关键字，表明该方法是一个静态方法，可以直接在Foo类上调用（Foo.classMethod()），而不是在Foo类的实例上调用。如果在实例上调用静态方法，会抛出一个错误，表示不存在该方法。
**注意，如果静态方法包含this关键字，这个this指的是类，而不是实例。**

父类的静态方法，可以被子类继承。父类Foo有一个静态方法，子类Bar可以调用这个方法。(实例不可以调用)

```
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
}

Bar.classMethod() // 'hello'
```
以前，我们定义实例属性，只能写在类的constructor方法里面。
```
class ReactCounter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
}
```
有了新的写法以后，可以不在constructor方法里面定义。
```
class ReactCounter extends React.Component {
  //实例属性
  state = {
    count: 0
  };
  static prop = 1; //静态属性
}
```

# class的继承 extend

在子类的构造函数中，只有调用super之后，才可以使用this关键字，否则会报错
```
class ColorPoint extends Point{
  constructor() {
    super() //super作为函数调用时，代表父类的构造函数constructor
  }    
}
```


父类的静态方法，也会被子类继承.子类可直接调用,但子类的实例不可以调用静态方法
```
class A {
  static hello() {
    console.log('hello world');
  }
}

class B extends A {
}

B.hello()  // hello world
```





