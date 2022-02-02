---
title: typescript
date: 2021-09-10 14:16:19
tags: [typescript]
category: [typescript]
---

<!-- toc -->

# 参考

https://ts.xcatliu.com/#%E5%85%B3%E4%BA%8E%E6%9C%AC%E4%B9%A6

# 终端运行 TS 文件

`yarn global add typescript`
`yarn global add ts-node`

## 文件结构

```
run-ts
- test.ts
- tsconfig.json
```

### test.ts

```ts
let anyThing: any = "hello";
console.log("anyThing.myName: ", anyThing.myName); // console 需 dom lib, 见 tsconfig.json
```

### tsconfig.json

```json
{
  "compilerOptions": {
    "allowJs": true,
    "esModuleInterop": true,
    "lib": ["esnext", "dom"],
    "module": "esnext",
    "moduleResolution": "node",
    "noEmit": true,
    "pretty": true,
    "resolveJsonModule": true,
    "target": "esnext"
  },
  "include": ["./*.ts"]
}
```

# 类型

## 回顾 js 数据类型

- 对象类型
- 原始数据类型：布尔值，数字，字符串，null，undefined，symbol

## void

- 没有返回值的函数
- 声明一个 `void` 类型的变量没有什么用，因为你只能将它赋值为 `undefined` 和 `null`
  - `let unusable: void = undefined;`

## undefined 和 null 是所有类型的子类型

可以赋值给任意类型

```typescript
let num: number = undefined;
let a: void = null;

num = a; // Type 'void' is not assignable to type 'number'
```

## any

在任意值上可以访问任何属性

也允许调用任何方法（存疑，见代码）

```typescript
let anyThing: any = "hello";
console.log(anyThing.myName); // undefined

let anyOne: any = "Tom";
console.log(anyOne.sayHello()); // TypeError: anyOne.sayHello is not a function
```

### 应用于 Vue+ts

```ts
this.$refs["adSpace"].validate(); // Property 'validate' does not exist on type 'Vue | Element | (Vue | Element)[]'.

(this.$refs["adSpace"] as any).validate();
```

## 联合类型

eg: `let myFavoriteNumber: string | number`

### 只能访问共有的属性或方法

```typescript
function getLength(something: string | number): number {
  return something.length;
}
// test.ts:2:20 - error TS2339: Property 'length' does not exist on type 'string | number'.
//  Property 'length' does not exist on type 'number'.
```

# 接口

## 可选、只读、任意属性

```typescript
interface Person {
  // 只读
  readonly id: number;
  name: string;
  // 可选
  age?: number;
  // 任意属性
  // 注：一旦定义了任意属性，那么确定属性和可选属性的类型都必须是任意属性类型(比如下面的 any)的子类；
  [propName: string]: any;
  // [propName: string]: number | string;
}
```

## ts 如何判断实例是否实现了接口

不能用 instanceof，因为运行时是不存在 Interface，只能基于接口形状进行判断

# 数组

```typescript
let a: [number, string, any] = [3, "7", undefined];
let b: number[] = [4, 5];
let c: any[] = [4, "5", null, undefined];
```

# 类型断言

手动指定一个值的类型, `<type>value` 或 `value as type`
tsx 中只能用 `value as type`，建议统一用 `value as type`

- 将一个联合类型断言为其中一个类型
- 将一个父类断言为更加具体的子类
- 将一个子类断言为父类
- 将任何一个类型断言为 any
- 将 any 断言为一个具体的类型

## 断言的限制条件

我们注意到，父子类可以互相断言，any 和其他类型可以互相断言

在 typescript 中，父类`兼容`子类，比如 `Cat extends Animal` 即 `Animal 兼容 Cat`

**_A 和 B 互相断言的前提：A 兼容 B 或者 B 兼容 A_**

```typescript
// 将一个联合类型断言为其中一个类型
function getLength(something: string | number): number {
  if ((<string>something).length) {
    return (<string>something).length;
  } else {
    return something.toString().length;
  }
}
```

## 将子类实例可赋给父类变量

```ts
interface Animal {
  name: string;
}
interface Cat {
  name: string;
  run(): void;
}

let tom: Cat = {
  name: "Tom",
  run: () => {
    console.log("run");
  },
};
let animal: Animal = tom;

console.log("将子类实例赋给父类变量", animal);
```

## 赋值时尽量避免用类型断言

我们通过断言 `animal as Cat` 成功赋值给变量 cat，使 cat 在后续中被当为 Cat 类型，但很明显此时的 cat 是不完整的

```ts
import { Animal, Cat } from "";

let animal: Animal = { name: "father-class-instance" };
// let cat: Cat = animal; // Property 'run' is missing in type 'Animal' but required in type 'Cat'.
let cat = animal as Cat; // 不会报错
console.log(cat); // { name: 'father-class-instance' }
```

# 声明文件

引用第三方库时，比如 jQuery，由于 ts 无法识别 `$('#foo') 或 jQuery('#foo')`，此时需要对 $ 或 jQuery 这两个全局变量进行声明，书写声明文件（`.d.ts` 为后缀）

## 管理、下载第三方库声明文件

- 使用 `@types` 统一管理第三方库的声明文件
- 下载 [@types](#) 管理下的 jQuery 声明文件 ：`npm install @types/jquery --save-dev`
  - 其他第三方库声明文件 [https://www.npmjs.com/search?q=@types](https://www.npmjs.com/search?q=@types)
  - 当一个第三方库没有提供声明文件时，我们就需要自己书写声明文件了

## 书写、发布声明文件

[https://ts.xcatliu.com/basics/declaration-files](https://ts.xcatliu.com/basics/declaration-files)

# 内置对象

### ts 包含 js、DOM、BOM 的内置对象

很多 js、DOM、BOM 的内置对象，ts 已经在其核心库中定义了，我们可以直接在 ts 中使用这些类型如 `Document`、`HTMLElement`、`Event`、`NodeList`、`Boolean`、`Error`、`Date`、`RegExp` 等

### ts 不包含 Node.js 的内置对象

如果想用 TypeScript 写 Node.js，则需要引入第三方声明文件：
`npm install @types/node --save-dev`

### TypeScript 核心库的定义文件

[https://github.com/Microsoft/TypeScript/tree/master/src/lib](https://github.com/Microsoft/TypeScript/tree/master/src/lib)

# 类型别名

```typescript
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
  if (typeof n === "string") {
    return n;
  } else {
    return n();
  }
}
```

# 字符串字面量类型

用来约束变量的取值只能是几个字符串之一，也用 `type` 来定义

`type EventNames = 'click' | 'scroll' | 'mousemove'`

# 元组

```typescript
let tom: [string, number];
tom = ["Tom", 25];
tom.push("male");
tom.push(true); // push tom 中未定义的元素，会报错

// Argument of type 'true' is not assignable to parameter of type 'string | number'.
```

# 枚举

枚举（Enum）类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等

```typescript
enum Days {
  Sun,
  Mon,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat,
}
console.log(Days["Sun"] === 0); // true
console.log(Days[0] === "Sun"); // true
```

## 手动赋值（不推荐）

```typescript
// 未手动赋值的，序号递增，步长为1
enum Days {
  Sun = 7,
  Mon = 1.5,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat,
}
console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1.5); // true
console.log(Days["Tue"] === 2.5); // true
console.log(Days["Sat"] === 6.5); // true

// 如果紧接在计算所得项(Red)后面的是未手动赋值的项(Green, Blue)
// 那么它就会因为无法获得初始值而报错
enum Color {
  Red = "red".length,
  Green,
  Blue,
}
// index.ts(1,33): error TS1061: Enum member must have initializer.
// index.ts(1,40): error TS1061: Enum member must have initializer.
```

## 常数枚举 const enum

常数枚举是使用 `const enum` 定义的枚举类型

```typescript
// 常数枚举不允许有计算成员
const enum Color {
  Red,
  Green,
  Blue = "blue".length,
}
// index.ts(1,38): error TS2474: In 'const' enum declarations member initializer must be constant expression.

// 常数枚举的类型如 Directions 会在编译阶段被删除
const enum Directions {
  Up,
  Down,
  Left,
  Right,
}
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
// 编译结果
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

## 外部枚举 declare enum

常用于声明文件，在编译结果中也会被删除（节省空间吧）

# 类

## 相关概念

- 抽象类（Abstract Class）：抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现

## 用法

### ES6

#### 继承

super 关键字调用父类的构造函数和方法

```typescript
class Cat extends Animal {
  constructor(name) {
    super(name); // 调用父类的 constructor(name)
    console.log(this.name);
  }
  sayHi() {
    return "Meow, " + super.sayHi(); // 调用父类的 sayHi()
  }
}

let c = new Cat("Tom"); // Tom
console.log(c.sayHi()); // Meow, My name is Tom
```

#### getter setter

```typescript
class Animal {
  constructor(name) {
    this.name = name;
  }
  get name() {
    return "Jack";
  }
  set name(value) {
    console.log("setter: " + value);
  }
}

let a = new Animal("Kitty"); // setter: Kitty
a.name = "Tom"; // setter: Tom
console.log(a.name); // Jack
```

#### 静态方法 static

直接通过类名来调用

```typescript
class Animal {
  static isAnimal(a) {
    return a instanceof Animal;
  }
}

let a = new Animal("Jack");
Animal.isAnimal(a); // true
a.isAnimal(a); // TypeError: a.isAnimal is not a function
```

## 访问修饰符

### public private protected

- 访问修饰符，决定了属性、方法能否被访问，分三种 _**公有、私有、子类中可访问 **_

  - public 到处都可访问。属性、方法默认为 public
  - private 私有，不能在 _**声明它的类**_ 的外部访问
  - protected 可以在子类中访问

- 修饰构造函数
  - private constructor 不允许继承和实例化
  - protected constructor 只允许继承
- 修饰构造函数参数

相当于 _**定义且赋值参数**_

```typescript
class Animal {
  // public name: string;
  // 不限制类型，这里的 string 只是举例，下面实例化给了数字 3
  public constructor(public name) {
    // this.name = name;
  }
}
const a = new Animal(3);
console.log(a.name);
```

### readonly

readonly 也是访问修饰符，跟 public 三者在一起时要放后面，如 ` public ``readonly `

## 抽象类

- abstract class 用于继承，且子类必须实现 abstract class 中的 abstract function

# 类与接口

## 类实现接口

类的一些共同特性可抽出来作为接口，让所有类来 implement (实现)，一个类可实现多个接口

```typescript
interface Alarm {
  alert(): void;
}

interface Light {
  lightOn(): void;
  lightOff(): void;
}

class Door {}

class SecurityDoor extends Door implements Alarm {
  alert() {
    console.log("SecurityDoor alert");
  }
}

class Car implements Alarm {
  alert() {
    console.log("Car alert");
  }
  lightOn() {
    console.log("Car light on");
  }
  lightOff() {
    console.log("Car light off");
  }
}
```

## 接口继承接口

```typescript
interface Alarm {
  alert(): void;
}

interface LightableAlarm extends Alarm {
  lightOn(): void;
  lightOff(): void;
}
```

## 接口继承类

不常用，此处不赘述

# 泛型

定义函数、接口、类的时候，不预先指定类型，使用时再指定类型的一种特性

```typescript
function createArray<T>(length: number, value: T): Array<T> {
  let result: T[] = [];
  for (let i = 0; i < length; i++) {
    result[i] = value;
  }
  return result;
}

// 指定 T 为 string
createArray<string>(3, "x"); // ['x', 'x', 'x']
// 不指定 T，由 ts 自己推出来
createArray(3, "x"); // ['x', 'x', 'x']
```

## 多个类型参数

```typescript
function swap<T, U>(tuple: [T, U]): [U, T] {
  return [tuple[1], tuple[0]];
}

swap([7, "seven"]); // ['seven', 7]
```

# 声明合并

# 扩展阅读
