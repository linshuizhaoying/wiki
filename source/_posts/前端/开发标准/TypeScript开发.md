---
title: typescript 基础开发
toc: true
date: 2017-09-05  17:49:39
tags: [前端,代码标准]
---

## 资料

[官方英文文档](http://www.typescriptlang.org/docs/handbook/jsx.html)


[官方中文文档](https://zhongsp.gitbooks.io/typescript-handbook/content/doc/handbook/tutorials/TypeScript%20in%205%20minutes.html)

[非官方文档](https://ts.xcatliu.com/basics/primitive-data-types.html)

[写d.ts参考](http://definitelytyped.org/)

> [typescript-handbook](https://zhongsp.gitbooks.io/typescript-handbook/)
> 
#### 原始类型

```

// 原始值
const isDone: boolean = false;
const amount: number = 6;
const address: string = 'beijing';
const greeting: string = `Hello World`;
// 数组
const list: number[] = [1, 2, 3];
const list: Array<number> = [1, 2, 3];
// 元组
const name: [string, string] = ['Sean', 'Sun'];
// 枚举
enum Color {
    Red,
    Green,
    Blue
};
const c: Color = Color.Green;
// 任意值：可以调用任意方法
let anyTypes: any = 4;
anyTypes = 'any';
anyTypes = false
// 空值
function doSomething (): void {
    return undefined;
}
// 类型断言
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;

```

#### 联合类型

```

let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;

```


#### 接口

> 接口（Interfaces）是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类（classes）去实现（implements）。

```

interface Person {
  name: string;
  age: number;
}

let xcatliu: Person = {
  name: 'Xcat Liu',
  age: 25,
};

// 可选属性,可选属性的含义是该属性可以不存在。

interface Person {
  name: string;
  age?: number;
}

let xcatliu: Person = {
  name: 'Xcat Liu',
};


// 任意属性 确定属性和可选属性都必须是它的子属性：
interface Person {
  name: string;
  age?: number;
  [propName: string]: any;
}

let xcatliu: Person = {
  name: 'Xcat Liu',
  website: 'http://xcatliu.com',
};


// 只读属性  readonly 定义只读属性

interface Person {
  readonly id: number;
  name: string;
  age?: number;
  [propName: string]: any;
}

let xcatliu: Person = {
  id: 89757,
  name: 'Xcat Liu',
  website: 'http://xcatliu.com',
};

xcatliu.id = 9527; // error


```

#### 数组的类型

```

let fibonacci: number[] = [1, 1, 2, 3, 5];


interface NumberArray {
  [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];

let list: any[] = ['Xcat Liu', 25, { website: 'http://xcatliu.com' }];


```

#### 函数

```

function sum(x: number, y: number): number {
  return x + y;
}

sum(1, 2, 3); //输入多余的（或者少于要求的）参数，是不被允许的：

let mySum = function (x: number, y: number): number {
  return x + y;
};

//接口中函数的定义

interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  return source.search(subString) !== -1;
}

// 可选参数

function buildName(firstName: string, lastName?: string) { // 可选参数后面不允许再出现必须参数
  if (lastName) {
    return firstName + ' ' + lastName;
  } else {
    return firstName;
  }
}
let xcatliu = buildName('Xcat', 'Liu');
let xcat = buildName('Xcat');

// 参数默认值

function buildName(firstName: string, lastName: string = 'Liu') {
  return firstName + ' ' + lastName;
}

```

#### 类型断言

<类型>值

// 或

值 as 类型


```

function getLength(something: string | number): number {
  if ((<string>something).length) {
    return (<string>something).length;
  } else {
    return something.toString().length;
  }
}

```

#### 声明文件


通常我们会把类型声明放到一个单独的文件中，这就是声明文件：

// jQuery.d.ts

declare var jQuery: (string) => any;
我们约定声明文件以 .d.ts 为后缀。

然后在使用到的文件的开头，用「三斜线指令」表示引用了声明文件：

/// <reference path="./jQuery.d.ts" />

jQuery('#foo');


#### 类型别名

> 类型别名用来给一个类型起个新名字。

```

type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
  if (typeof n === 'string') {
    return n;
  }
  else {
    return n();
  }
}

```

#### 字符串字面量类型

> 字符串字面量类型用来约束取值只能是某几个字符串中的一个。


```

type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
  // do something
}

handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
handleEvent(document.getElementById('world'), 'dbclick'); // 报错，event 不能为 'dbclick'

```

#### 元组


> 数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象。


```

let xcatliu: [string, number] = ['Xcat Liu', 25];

let xcatliu: [string, number];
xcatliu[0] = 'Xcat Liu';
xcatliu[1] = 25;

xcatliu[0].slice(1);
xcatliu[1].toFixed(2);

// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error

```


#### 类

> 类(Class)：定义了一件事物的抽象特点，包含它的属性和方法
> 对象（Object）：类的实例，通过 new 生成
> 面向对象（OOP）的三大特性：封装、继承、多态

> 封装（Encapsulation）：将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节，就能通过对外提供的接口来访问该对象，同时也保证了外界无法任意更改对象内部的数据

> 继承（Inheritance）：子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体的特性

> 多态（Polymorphism）：由继承而产生了相关的不同的类，对同一个方法可以有不同的响应。比如 Cat 和 Dog 都继承自 Animal，但是分别实现了自己的 eat 方法。此时针对某一个实例，我们无需了解它是 Cat 还是 Dog，就可以直接调用 eat 方法，程序会自动判断出来应该如何执行 eat
存取器（getter & setter）：用以改变属性的读取和赋值行为

> 修饰符（Modifiers）：修饰符是一些关键字，用于限定成员或类型的性质。比如 public 表示公有属性或方法

> 抽象类（Abstract Class）：抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现

> 接口（Interfaces）：不同类之间公有的属性或方法，可以抽象成一个接口。接口可以被类实现（implements）。一个类只能继承自另一个类，但是可以实现多个接口

```

使用 class 定义类，使用 constructor 定义构造函数。

通过 new 生成新实例的时候，会自动调用构造函数。

class Animal {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    return `My name is ${this.name}`;
  }
}

let a = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack

```


```

类的继承

使用 extends 关键字实现继承，子类中使用 super 关键字来调用父类的构造函数和方法。

class Cat extends Animal {
  constructor(name) {
    super(name); // 调用父类的 constructor(name)
    console.log(this.name);
  }
  sayHi() {
    return 'Meow, ' + super.sayHi(); // 调用父类的 sayHi()
  }
}

let c = new Cat('Tom'); // Tom
console.log(c.sayHi()); // Meow, My name is Tom

```

```

存取器

使用 getter 和 setter 可以改变属性的赋值和读取行为：

class Animal {
  constructor(name) {
    this.name = name;
  }
  get name() {
    return 'Jack';
  }
  set name(value) {
    console.log('setter: ' + value);
  }
}

let a = new Animal('Kitty'); // setter: Kitty
a.name = 'Tom'; // setter: Tom
console.log(a.name); // Jack


静态方法

使用 static 修饰符修饰的方法称为静态方法，它们不需要实例化，而是直接通过类来调用：

class Animal {
  static isAnimal(a) {
    return a instanceof Animal;
  }
}

let a = new Animal('Jack');
Animal.isAnimal(a); // true
a.isAnimal(a); // TypeError: a.isAnimal is not a function

```

#### TypeScript 中类的用法

```

TypeScript 可以使用三种访问修饰符（Access Modifiers），分别是 public、private 和 protected。

public 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 public 的
private 修饰的属性或方法是私有的，不能在声明它的类的外部访问
protected 修饰的属性或方法是受保护的，它和 private 类似，区别是它在子类中也是允许被访问的
下面举一些例子：

class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom';
console.log(a.name); // Tom
上面的例子中，name 被设置为了 public，所以直接访问实例的 name 属性是允许的。

很多时候，我们希望有的属性是无法直接存取的，这时候就可以用 private 了：

class Animal {
  private name;
  public constructor(name) {
    this.name = name;
  }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom';

// index.ts(9,13): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
// index.ts(10,1): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
需要注意的是，TypeScript 编译之后的代码中，并没有限制 private 属性在外部的可访问性。

上面的例子编译后的代码是：

var Animal = (function () {
    function Animal(name) {
        this.name = name;
    }
    return Animal;
}());
var a = new Animal('Jack');
console.log(a.name);
a.name = 'Tom';
使用 private 修饰的属性或方法，在子类中也是不允许访问的：

class Animal {
  private name;
  public constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
  constructor(name) {
    super(name);
    console.log(this.name);
  }
}

// index.ts(11,17): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
而如果是用 protected 修饰，则允许在子类中访问：

class Animal {
  protected name;
  public constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
  constructor(name) {
    super(name);
    console.log(this.name);
  }
}

```

抽象类

```

abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}

class Cat extends Animal {
  public sayHi() {
    console.log(`Meow, My name is ${this.name}`);
  }
}

let cat = new Cat('Tom');

```

#### 一个类可以实现多个接口：

> 一般来讲，一个类只能继承自另一个类，有时候不同类之间可以有一些共有的特性，这时候就可以把特性提取成接口（interfaces），用 implements 关键字来实现。这个特性大大提高了面向对象的灵活性。
 
 
```

interface Alarm {
  alert();
}

interface Light {
  lightOn();
  lightOff();
}

class Car implements Alarm, Light {
  alert() {
    console.log('Car alert');
  }
  lightOn() {
    console.log('Car light on');
  }
  lightOff() {
    console.log('Car light off');
  }
}

```

#### 泛型

> 泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

```

实现一个函数 createArray，它可以创建一个指定长度的数组，同时将每一项都填充一个默认值：

function createArray<T>(length: number, value: T): Array<T> {
  let result = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}

createArray<string>(3, 'x'); // ['x', 'x', 'x']


在函数名后添加了 <T>，其中 T 用来指代任意输入的类型，在后面的输入 value: T 和输出 Array<T> 中即可使用了

```

`多个类型参数`

```

定义泛型的时候，可以一次定义多个类型参数：

function swap<T, U>(tuple: [T, U]): [U, T] {
  return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]
上例中，我们定义了一个 swap 函数，用来交换输入的元组。

```

`泛型约束`

```

我们可以对泛型进行约束，只允许这个函数传入那些包含 length 属性的变量。这就是泛型约束：

interface Lengthwise {
  length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}

多个类型参数之间也可以互相约束：

function copyFields<T extends U, U>(target: T, source: U): T {
  for (let id in source) {
    target[id] = (<T>source)[id];
  }
  return target;
}

let x = { a: 1, b: 2, c: 3, d: 4 };

copyFields(x, { b: 10, d: 20 });
上例中，我们使用了两个类型参数，其中要求 T 继承 U，这样就保证了 U 上不会出现 T 中不存在的字段。

```


`泛型接口`

```

interface CreateArrayFunc {
  <T>(length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc;
createArray = function<T>(length: number, value: T): Array<T> {
  let result = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']

```

`泛型类`

```

class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };

```

## 提升学习

#### 接口学习

常用的接口定义一般是这么写的:

```

interface Project{
  _id : String,
  projectName?: String,
  projectUrl?: String,
  projectDesc?: String,
  version?: String,
  transferUrl?: String,
  status?: String,
  type?: String,
  teamMember?: Array<any>,
  interfaceList?: Array<any>,
}


```

这在约定好接口字段的时候很管用，但是有时候字段没约定好，比如有时候可能会传入多余的数据，这个时候编译前会自动报错提醒。

此时可以考虑这么写

```

interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}

SquareConfig可以有任意数量的属性，并且只要它们不是color和width，那么就无所谓它们的类型是什么。

```




#### 泛型学习

经过学习，除去经常用的接口定义 Interface, 以及常用的基本类型定义。想要更优写法肯定需要应用上泛型。

> 泛型来创建可重用的组件，一个组件可以支持多种类型的数据。 这样用户就可以以自己的数据类型来使用组件。

> 泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。 

> 泛型就是指定一个表示类型的变量，用它来代替某个实际的类型用于编程，而后通过实际调用时传入或推导的类型来对其进行替换，以达到一段使用泛型程序可以实际适应不同类型的目的。

`HandBook` 上有几个例子，比如当需要一种方法使返回值的类型与传入参数的类型是相同的情况下。

```

function identity<T>(arg: T): T {
    return arg;
}

给identity添加了类型变量T。 T帮助我们捕获用户传入的类型（比如：number），之后我们就可以使用这个类型。 之后我们再次使用了T当做返回值类型。现在我们可以知道参数类型与返回值类型是相同的了。 这允许我们跟踪函数里使用的类型的信息。

```

还有就是泛型类的应用。

泛型类其实多数时候是应用于容器类。假设我们需要实现一个 FilteredList，我们可以向其中 add()(添加) 任意数据，但是它在添加的时候会自动过滤掉不符合条件的一些，最终通过 get all() 输出所有符合条件的数据(数组)。而过滤条件在构造对象的时候，以函数或 Lambda 表达式提供。

```

// 声明泛型类，类型变量为 T
class FilteredList<T> {
    // 声明过滤器是以 T 为参数类型，返回 boolean 的函数表达式
    filter: (v: T) => boolean;
    // 声明数据是 T 数组类型
    data: T[];
    constructor(filter: (v: T) => boolean) {
        this.filter = filter;
    }

    add(value: T) {
        if (this.filter(value)) {
            this.data.push(value);
        }
    }

    get all(): T[] {
        return this.data;
    }
}

// 处理 string 类型的 FilteredList
const validStrings = new FilteredList<string>(s => !s);

// 处理 number 类型的 FilteredList
const positiveNumber  = new FilteredList<number>(n => n > 0);

```

这个可以应用于维护本地数据的地方，以及ajax提交数据的一些地方。主要是用于筛选数据。我们完全可以基于这个思想做一些多条件的过滤。在很多对数据需要处理的地方这个还是很有用的。

### 命名空间与模块

 “内部模块”现在称做“命名空间”。 “外部模块”现在则简称为“模块”。
 
 常用的 `helper函数`，封装的 `until` 系列都是习惯用 export 导出模块然后引用的。但是一些验证函数如果也这么使用就不稳妥了。
 
 随着更多验证器的加入，我们需要一种手段来组织代码，以便于在记录它们类型的同时还不用担心与其它对象产生命名冲突。 因此，我们把验证器包裹到一个命名空间内，而不是把它们放在模块下。
  我们来看一个例子。在之前我们是这么写的:
  
  ```
  
  interface StringValidator {
    isAcceptable(s: string): boolean;
}

let lettersRegexp = /^[A-Za-z]+$/;
let numberRegexp = /^[0-9]+$/;

class LettersOnlyValidator implements StringValidator {
    isAcceptable(s: string) {
        return lettersRegexp.test(s);
    }
}

class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegexp.test(s);
    }
}

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: StringValidator; } = {};
validators["ZIP code"] = new ZipCodeValidator();
validators["Letters only"] = new LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        let isMatch = validators[name].isAcceptable(s);
        console.log(`'${ s }' ${ isMatch ? "matches" : "does not match" } '${ name }'.`);
    }
}
  
  ```

然后改成命名空间的写法：

```

namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }

    const lettersRegexp = /^[A-Za-z]+$/;
    const numberRegexp = /^[0-9]+$/;

    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }

    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        console.log(`"${ s }" - ${ validators[name].isAcceptable(s) ? "matches" : "does not match" } ${ name }`);
    }
}

```

很明显这个组织格式差别不大，但是在一个命名空间内可以避免一些不必要的冲突。

然后我们可以将一些不同逻辑的代码进行物理分割，但是在逻辑上，在同一个命名空间内，即使它们分离两地但是还是心连心。

```

Validation.ts
namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }
}
LettersOnlyValidator.ts
/// <reference path="Validation.ts" />
namespace Validation {
    const lettersRegexp = /^[A-Za-z]+$/;
    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }
}
ZipCodeValidator.ts
/// <reference path="Validation.ts" />
namespace Validation {
    const numberRegexp = /^[0-9]+$/;
    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}
Test.ts
/// <reference path="Validation.ts" />
/// <reference path="LettersOnlyValidator.ts" />
/// <reference path="ZipCodeValidator.ts" />

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        console.log(`"${ s }" - ${ validators[name].isAcceptable(s) ? "matches" : "does not match" } ${ name }`);
    }
}

```

模块的基本都了解，在 Typescirpt 中，我们可以用更好的方式组织模块。

比如我们常常引用自己的模块是通过相对导入。

比如 `import { DefaultHeaders } from "../constants/http";`

我们引用的一些外部模块是非相对的.

比如 `import * as $ from "jQuery";`

对于一些比较成熟的依赖库我们可以通过将其作为外部模块载入。

>可以使用顶级的 export 声明来为每个模块都定义一个 .d.ts 文件，但最好还是写在一个大的 .d.ts 文件里。 我们使用与构造一个外部命名空间相似的方法，但是这里使用 module 关键字并且把名字用引号括起来，方便之后 import.

```

node.d.ts (simplified excerpt)
declare module "url" {
    export interface Url {
        protocol?: string;
        hostname?: string;
        pathname?: string;
    }

    export function parse(urlStr: string, parseQueryString?, slashesDenoteHost?): Url;
}

declare module "path" {
    export function normalize(p: string): string;
    export function join(...paths: any[]): string;
    export let sep: string;
}

```

我们可以 `/// <reference> node.d.ts` 并且使用 `import url = require("url");` 或 `import * as URL from "url"` 加载模块

```

/// <reference path="node.d.ts"/>
import * as URL from "url";
let myUrl = URL.parse("http://www.typescriptlang.org");

```
 
### 书写声明文件

针对模块有三种可用的模块， `module.d.ts`, `module-class.d.ts` 和 `module-function.d.ts`. 

如果模块不能被调用或构造，使用module.d.ts文件。

`module.d.ts`里一般这么写:

```

// Type definitions for [~THE LIBRARY NAME~] [~OPTIONAL VERSION NUMBER~]
// Project: [~THE PROJECT NAME~]
// Definitions by: [~YOUR NAME~] <[~A URL FOR YOU~]>

/*~ This is the module template file. You should rename it to index.d.ts
 *~ and place it in a folder with the same name as the module.
 *~ For example, if you were writing a file for "super-greeter", this
 *~ file should be 'super-greeter/index.d.ts'
 */

/*~ If this module is a UMD module that exposes a global variable 'myLib' when
 *~ loaded outside a module loader environment, declare that global here.
 *~ Otherwise, delete this declaration.
 */
export as namespace myLib;

/*~ If this module has methods, declare them as functions like so.
 */
export function myMethod(a: string): string;
export function myOtherMethod(a: number): number;

/*~ You can declare types that are available via importing the module */
export interface someType {
    name: string;
    length: number;
    extras?: string[];
}

/*~ You can declare properties of the module using const, let, or var */
export const myField: number;

/*~ If there are types, properties, or methods inside dotted names
 *~ of the module, declare them inside a 'namespace'.
 */
export namespace subProp {
    /*~ For example, given this definition, someone could write:
     *~   import { subProp } from 'yourModule';
     *~   subProp.foo();
     *~ or
     *~   import * as yourMod from 'yourModule';
     *~   yourMod.subProp.foo();
     */
    export function foo(): void;
}

```


```

使用 module-class.d.ts 如果模块能够使用new来构造：
var x = require("bar");
// Note: using 'new' operator on the imported variable
var y = new x("hello");

```

`module-class.d.ts` 的模板写法如下:

```

// Type definitions for [~THE LIBRARY NAME~] [~OPTIONAL VERSION NUMBER~]
// Project: [~THE PROJECT NAME~]
// Definitions by: [~YOUR NAME~] <[~A URL FOR YOU~]>

/*~ This is the module template file for function modules.
 *~ You should rename it to index.d.ts and place it in a folder with the same name as the module.
 *~ For example, if you were writing a file for "super-greeter", this
 *~ file should be 'super-greeter/index.d.ts'
 */

/*~ Note that ES6 modules cannot directly export callable functions.
 *~ This file should be imported using the CommonJS-style:
 *~   import x = require('someLibrary');
 *~
 *~ Refer to the documentation to understand common
 *~ workarounds for this limitation of ES6 modules.
 */

/*~ If this module is a UMD module that exposes a global variable 'myFuncLib' when
 *~ loaded outside a module loader environment, declare that global here.
 *~ Otherwise, delete this declaration.
 */
export as namespace myFuncLib;

/*~ This declaration specifies that the function
 *~ is the exported object from the file
 */
export = MyFunction;

/*~ This example shows how to have multiple overloads for your function */
declare function MyFunction(name: string): MyFunction.NamedReturnType;
declare function MyFunction(length: number): MyFunction.LengthReturnType;

/*~ If you want to expose types from your module as well, you can
 *~ place them in this block. Often you will want to describe the
 *~ shape of the return type of the function; that type should
 *~ be declared in here, as this example shows.
 */
declare namespace MyFunction {
    export interface LengthReturnType {
        width: number;
        height: number;
    }
    export interface NamedReturnType {
        firstName: string;
        lastName: string;
    }

    /*~ If the module also has properties, declare them here. For example,
     *~ this declaration says that this code is legal:
     *~   import f = require('myFuncLibrary');
     *~   console.log(f.defaultName);
     */
    export const defaultName: string;
    export let defaultLength: number;
}

```



```

使用 module-function.d.ts，如果模块能够作为函数调用。
var x = require("foo");
// Note: calling 'x' as a function
var y = x(42);

```

`module-function.d.ts` 的模板写法如下:

```

// Type definitions for [~THE LIBRARY NAME~] [~OPTIONAL VERSION NUMBER~]
// Project: [~THE PROJECT NAME~]
// Definitions by: [~YOUR NAME~] <[~A URL FOR YOU~]>

/*~ This is the module template file for class modules.
 *~ You should rename it to index.d.ts and place it in a folder with the same name as the module.
 *~ For example, if you were writing a file for "super-greeter", this
 *~ file should be 'super-greeter/index.d.ts'
 */

/*~ Note that ES6 modules cannot directly export class objects.
 *~ This file should be imported using the CommonJS-style:
 *~   import x = require('someLibrary');
 *~
 *~ Refer to the documentation to understand common
 *~ workarounds for this limitation of ES6 modules.
 */

/*~ If this module is a UMD module that exposes a global variable 'myClassLib' when
 *~ loaded outside a module loader environment, declare that global here.
 *~ Otherwise, delete this declaration.
 */
export as namespace myClassLib;

/*~ This declaration specifies that the class constructor function
 *~ is the exported object from the file
 */
export = MyClass;

/*~ Write your module's methods and properties in this class */
declare class MyClass {
    constructor(someParam?: string);

    someProperty: string[];

    myMethod(opts: MyClass.MyClassMethodOptions): number;
}

/*~ If you want to expose types from your module as well, you can
 *~ place them in this block.
 */
declare namespace MyClass {
    export interface MyClassMethodOptions {
        width?: number;
        height?: number;
    }
}

```

一些规范之前没有注意，在项目需要重新审视。

>不要使用如下类型Number，String，Boolean或Object。 这些类型指的是非原始的装盒对象，它们几乎没在JavaScript代码里正确地使用过。

```

/* 错误 */
function reverse(s: String): String;

/* OK */
function reverse(s: string): string;

```

一些例子:

**全局变量** :  `declare var foo: number;`

**全局变量** : `declare function greet(greeting: string): void;`

**带属性的对象** :

```

全局变量myLib包含一个makeGreeting函数， 还有一个属性numberOfGreetings指示目前为止欢迎数量。

代码

let result = myLib.makeGreeting("hello, world");
console.log("The computed greeting is:" + result);

let count = myLib.numberOfGreetings;
声明

使用declare namespace描述用点表示法访问的类型或值。
declare namespace myLib {
    function makeGreeting(s: string): string;
    let numberOfGreetings: number;
}

```

**函数重载** :

```

getWidget函数接收一个数字，返回一个组件，或接收一个字符串并返回一个组件数组。

代码

let x: Widget = getWidget(43);

let arr: Widget[] = getWidget("all of them");
声明

declare function getWidget(n: number): Widget;
declare function getWidget(s: string): Widget[];

```

**组织类型** :

```

greeter对象能够记录到文件或显示一个警告。 你可以为.log(...)提供LogOptions和为.alert(...)提供选项。
代码

const g = new Greeter("Hello");
g.log({ verbose: true });
g.alert({ modal: false, title: "Current Greeting" });
声明

使用命名空间组织类型。
declare namespace GreetingLib {
    interface LogOptions {
        verbose?: boolean;
    }
    interface AlertOptions {
        modal: boolean;
        title?: string;
        color?: string;
    }
}
你也可以在一个声明中创建嵌套的命名空间：
declare namespace GreetingLib.Options {
    // Refer to via GreetingLib.Options.Log
    interface Log {
        verbose?: boolean;
    }
    interface Alert {
        modal: boolean;
        title?: string;
        color?: string;
    }
}

```




