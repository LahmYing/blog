---
title: js
date: 2021-04-22 16:00:49
tags: js
category: [js]
---

<!-- toc -->

# 一些概念

见 https://github.com/LahmYing/front-end-tools/tree/master/%E8%AF%AD%E8%A8%80/js

# for in, for of, forEach

for in 语句以任意顺序迭代对象的可枚举属性
for of 语句遍历可迭代对象定义要迭代的数据

for in 是为遍历对象属性而构建的，不建议与数组一起使用，数组可以用 Array.prototype.forEach() 和 for of

## 建议使用 for of 而不是 forEach

forEach 会跳过空值

```js
const array1 = ["a", , "c"];

for (const element of array1) {
  console.log(element); //  a, undefined, c
}

array1.forEach((element) => {
  console.log("forEach", element); // a, c
});
```

# String

```js
// 字符串可看作数组的一种进行操作

// 替换
replaceStr(".1.2.3", ".", "**"); // '.1.2.3' → "**1**2**3"
let replaceStr = function (str, before, after) {
  if (typeof str == "string") {
    let afterReplace = str.split(before).join(after);
  } else {
    console.log("ERROR: 要替换的第一个参数不是字符串");
  }

  return afterReplace;
};

// 解析 \
String.raw`Hi\n${2 + 3}!`;
// 返回 "Hi\\n5!"
String.raw`Hi\\n`;
// 返回 "Hi\\\\n"

// 包含
let s = "Hello world!";
s.startsWith("world", 6); // true
s.endsWith("Hello", 5); // true
s.includes("Hello", 6); // false

// 重复
"na".repeat(2.9); // "nana"
"x".repeat(3); // "xxx"

// 头尾补全
"x".padStart(5, "ab"); // 'ababx'
"x".padStart(4, "ab"); // 'abax'
"x".padEnd(5, "ab"); // 'xabab'
"x".padEnd(4, "ab"); // 'xaba'

// 消除空格
const s = "  abc  ";
s.trim(); // "abc"
s.trimStart(); // "abc  "
s.trimEnd(); // "  abc"
```

# NaN

```js
// NaN === NaN // false
// isNaN(NaN) // true
// 该函数接收一个参数，这个参数可以是任何类型，
// 如果接收的参数是数字类型，返回false;
// 如果是其他类型（除了数字的任何其他类型），则返回true
// isNaN('xyz') // true
```

# this

非箭头函数：会变动，运行时确定，可用 call() 改写后确定

箭头函数：定义时确定，本身没绑定 this，取父级的 context，最近的 {} 即为父级

# arguments

```js
let publish = function () {
  console.log(Object.prototype.toString.call(arguments)); // [object Arguments]
  // 将 arguments 对象转换为真正的数组
  let args = Array.prototype.slice.call(arguments, 0);
};

publish();
```

# 发布订阅

```js
let PubSub = {
  subscribe: function (ev, callback) {
    // 创建 _callbacks 对象，除非它已经存在了
    let calls = this._callbacks || (this._callbacks = {});
    // 针对给定的事件 key 创建一个数组，除非这个数组已经存在
    // 然后将回调函数追加到这个数组中
    (this._callbacks[ev] || (this._callbacks[ev] = [])).push(callback);
    return this;
  },
  publish: function () {
    // 将 arguments 对象转换为真正的数组
    let args = Array.prototype.slice.call(arguments, 0);
    // 拿出第 1 个参数，即事件名称
    let ev = args.shift();
    // 如果不存在 _callbacks 对象，则返回
    // 或者如果不包含给定事件对应的数组
    let list, calls, i, l;
    if (!(calls = this._callbacks)) return this;
    if (!(list = this._callbacks[ev])) return this;
    // 触发回调
    for (i = 0, l = list.length; i < l; i++) list[i].apply(this, args);
    return this;
  },
};

PubSub.subscribe("wem", function () {
  alert("Wem!");
});
PubSub.publish("wem");
```

# Array

## array.map 是否改变原数组

map 不修改调用它的原数组本身（当然可以在 callback 执行时改变原数组）

## Array.of

`Array.of()`基本上可以用来替代`Array()`或`new Array()`，并且不存在由于参数不同而导致的重载。它的行为非常统一

## find

返回第一个符合条件的数组成员

```javascript
[1, 4, -5, 10].find((n) => n < 0);
// -5
```

## findIndex

返回第一个符合条件的数组成员的位置

```javascript
[1, 5, 10, 15].findIndex(function (value, index, arr) {
  return value > 9;
}); // 2
```

## fill

```javascript
["a", "b", "c"].fill(7, 1, 2); // ['a', 7, 'c']
```

## 转成一维数组

```javascript
// 返回一个新数组
// 如果不管有多少层嵌套，都要转成一维数组，可以用Infinity关键字作为参数
[1, [2, [3, [4, [5]]]]].flat(Infinity);
// [1, 2, 3, 4, 5]
```

## 排序

```javascript
const arr = ["peach", "straw", "apple", "spork"];

const stableSorting = (s1, s2) => {
  if (s1[0] < s2[0]) return -1;
  return 1;
};

arr.sort(stableSorting);
// ["apple", "peach", "straw", "spork"]
```

## 其他

```js
// 首操作 shift unshift

// 尾操作 pop push

// 排序 sort
// sort 可以接收一个比较函数来实现自定义的排序
let arr = [11, 20, 1, 3, 5, 30];
// 大到小
arr.sort((x, y) => y - x); // >> Array(6) [ 30, 20, 11, 5, 3, 1 ]
// 小到大
arr.sort((x, y) => x - y); // >> Array(6) [ 1, 3, 5, 11, 20, 30 ]

// 翻转 reserve

// 连接 concat
let arr = [1, 2, 3];
let newArr = arr.concat([4, 5, 6], [7, 8, 9]);

// 插入
splice(2, 0, "red", "green"); // 会从当前数组的位置2开始插入字符串"red"和"green"

// 切片
// s.slice(开始下标, 结束下标)
s.slice(0, 3);
s.slice(1, 3);
// 省略下标参数意思是取到底
s.slice(2);

// 生成
// 只要一个对象有length，Array.from就能把它变成一个数组，返回新的数组
// 对 String，Set，Map 也可以
let likeArr = {
  0: "a",
  1: "b",
  2: "c",
  length: 3,
};

// 生成
// Array.of 总是返回参数值组成的数组
Array.of(); // []
Array.of(undefined); // [undefined]
Array.of(3); // [3]
Array.of(3, 4, 5); // [3, 4, 5]

// 复制
// Array.copyWithin(target, start = 0, end = this.length)
// 将指定位置的数组项复制到其他位置，会覆盖原数组项，然后返回当前数组。使用该方法会修改当前数组
// （1）target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
// （2）start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示倒数。
// （3）end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。
// ['a', 'b', 'c', 'd', 'e', 'f', 'g'].copyWithin(0, 3) //  ["d", "e", "f", "g", "e", "f", "g"]

// 填充
// [1,2,3,4,5].fill('a'); // ["a", "a", "a", "a", "a"]
// [1, 2, 3, 4, 5].fill('a', 2, 4); // [1, 2, "a", "a", 5]

// 序号
// [1, 4, -5, 10].find((n) => n < 0) // -5
// [1, 4, -5, 10].find((n) => n < -10) // undefined

// 序号
// findIndex
// 返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回 - 1
// findIndex 可借助 Object.is 识别 NaN, indexOf 不可以
// ['a', 'b', NaN, 'c'].findIndex(y => Object.is(NaN, y)) // 2

// 包含
// includes
// [NaN].includes(NaN) // true
// [NaN].indexOf(NaN)  // -1

// map
// 创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果
const new_array = [1, 2, 3].map((x) => x * x);
// Array(3)[1, 4, 9]

// filter
// 创建一个新数组, 其包含通过所提供函数实现的测试的所有元素
const new_array2 = [1, 2, 3].filter((x) => x > 1);
// Array [ 2, 3 ]

// reduce
// reduce() 方法对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15
```

# Object

## Object.is 与 ===

不同之处只有两个：

- +0 不等于-0
- NaN 等于自身

```javascript
Object.is("xixi", "xixi"); //true
Object.is({}, {}) + //false
  0 ===
  -0; //true
NaN === NaN; // false

Object.is(+0, -0); // false
Object.is(NaN, NaN); // true
```

## 浅拷贝

```javascript
// 浅拷贝
Object.assign(target, ...sources);
```

## **_proto_**

```javascript
// __proto__ 建议换成
Object.setPrototypeOf(); // （写操作）
Object.getPrototypeOf(); // （读操作）
Object.create(); // （生成操作）
```

## 返回对象自身属性（非继承）

```javascript
// 返回指定对象所有自身属性（非继承属性）的描述对象
// 自身属性、静态属性应该是一个意思
Object.getOwnPropertyDescriptor();
```

## 遍历

```javascript
// 遍历 keys（不含继承的）
// Object.keys()
var obj = { foo: "bar", baz: 42 };
Object.keys(obj); // ["foo", "baz"]
// 遍历 values（不含继承的）
Object.values(obj);
// 遍历 entries（不含继承的）
Object.entries(obj); // [["foo", "baz"], [ "baz", 42 ]]
```

## 冻结

```javascript
// 冻结
Object.freeze(obj);
// 作为参数传递的对象与返回的对象都被冻结
// 所以不必保存返回的对象（因为两个对象全等）
var o = Object.freeze(obj);
o === obj; // true
Object.isFrozen(obj); // === true
```

## Object.values/keys/entries

```js
Object.values({ x: 1, y: 2, a: 3, b: 4 });
// [1, 2, 3, 4]
JSON.stringify(Object.entries({ x: 1, y: 2, a: 3, b: 4 }));
// "[[\"x\",1],[\"y\",2],[\"a\",3],[\"b\",4]]"
Object.keys({ x: 1, y: 2, a: 3, b: 4 });
// ["x", "y", "a", "b"]
```

# rest 运算符

与扩展符都是 `...` ，作用相反，用于代替 arguments 变量

```js
bar = function (a, ...args) {
  console.log(a);
  console.log(args);
};
bar(1, 2, 3, 4);
//1
//[ 2, 3, 4 ]
```

```js
let arr = [
  { name: "Li", age: 10 },
  { name: "Hu", age: 12 },
];
let arr1 = arr.map((item) => {
  item.age = item.age + 2;
  return item;
});
console.log(JSON.stringify(arr));
// "[{\"name\":\"Li\",\"age\":12},{\"name\":\"Hu\",\"age\":14}]"
```

# 立即执行函数

立即执行函数其实就是一个语句

```js
// 变量可以这样传递
// 引入全局变量
let a = 3;
(function ($) {
  console.log($);
})(a); // 3

// 导出为全局变量，直接挂在 window 下
(function ($, exports) {
  exports.Foo = "wem";
})(jQuery, window);
```

# 闭包

- 函数 A 有（或返回）内部函数 B，函数 B 中使用了函数 A 的变量 a，这种情况成为闭包
- 注意不同的闭包实例 a 变量的值
- 执行的函数或实例被回收后，a 变量才会被回收

```js
function init() {
  var name = "Mozilla";
  function displayName() {
    alert(name);
  }
  // 执行的函数
  displayName();
}
init();

// 注意不同的闭包实例 a 变量的值
function makeFunc() {
  var count = 1;
  function display() {
    alert(count);
    count = count + 1;
  }
  return display;
}
var myFunc = makeFunc(); // 实例
myFunc(); // 1
myFunc(); // 2
myFunc(); // 3

function makeAdder(x) {
  return function (y) {
    return x + y;
  };
}
var add5 = makeAdder(5);
var add10 = makeAdder(10);
console.log(add5(2)); // 7
console.log(add10(2)); // 12
```

<!-- # 定义+初始化+赋值

- 与通过 var 声明的有初始化值 undefined 的变量不同，通过 let 声明的变量直到它们的定义被执行时才初始化

```js
console.log(z); // undefined,即 z 被初始化为 undefined
var z = "z";
console.log(z); // z

var a = 1;
console.log(a); // 1
var a = 2; // 不会报 “重复定义” 的错
console.log(a); // 2

// 默认为通过 var 定义，挂在 global 下
console.log(typeof undeclaredVariable); // undefined
``` -->

# new（创建实例）

比如 `let newObj = new Shape()`
以下是 new 的目的所在

## 实例获取原型属性/方法

`emptyObj.__proto__ = Shape.prototype`

## 实例获取实例属性/方法

临时空对象 emptyObj， `替换 this` ，返回 emptyObj

```js
function Shape(inst_name) {
  console.log(this);
  this.x = "x,实例属性";
  this.inst_name = String(inst_name);
  // 执行时，work.call(this) 换成 work.call(emptyObj)
  this.work = function () {
    console.log("work,实例方法");
  };
}
```

# 立即执行函数

优点：独立的作用域，不会污染全局变量（第一个括号内）

```js
(function () {
  var age = 12;
  var name = {
    first: "Barry",
    last: "King",
  };
  return {
    name,
    age,
  };
})();

// 赋值（执行后的返回）
let r = (function () {
  var age = 12;
  var name = {
    first: "Barry",
    last: "King",
  };
  return {
    name,
    age,
  };
})();
```

# 深浅拷贝

```js
// 浅拷贝
// 1.Object.clone
// 2.Object.assign
// 3.扩展符号，比如 let b = [...a],就拷贝了 a 给 b

// 深拷贝
// 1.JSON.parse(JSON.stringify(object))
//    stringify 后对象转为字符串，为基本类型，此时再 parse 成新对象时，就不会与老对象共用指针了
// 缺点：
//    会忽略 undefined
//    不能序列化函数
//    不能解决循环引用的对象

// 2.如果确定没有 循环引用，那就用这个吧
//deep copy深拷贝
var deepCopy = function (obj) {
  const keys = Object.keys(obj); //得到obj里所有的keys

  const newObject = {};

  for (let i = 0; i < keys.length; i++) {
    const key = keys[i];
    if (typeof obj[key] === "object") {
      newObject[key] = deepCopy(obj[key]); // 此处递归
    } else {
      newObject[key] = obj[key];
    }
  }
  return newObject;
};

// 3.最通用的，但对性能有影响，用
// lodash 的 _.cloneDeep(value) 函数

// 4.如果不想引入 lodash 库，那就用下面这个吧，
// 保持引用关系
function cloneForce(x) {
  // =============
  const uniqueList = []; // 用来去重
  // =============

  let root = {};

  // 循环数组
  const loopList = [
    {
      parent: root,
      key: undefined,
      data: x,
    },
  ];

  while (loopList.length) {
    // 深度优先
    const node = loopList.pop();
    const parent = node.parent;
    const key = node.key;
    const data = node.data;

    // 初始化赋值目标，key为undefined则拷贝到父元素，否则拷贝到子元素
    let res = parent;
    if (typeof key !== "undefined") {
      res = parent[key] = {};
    }

    // =============
    // 数据已经存在
    let uniqueData = find(uniqueList, data);
    if (uniqueData) {
      parent[key] = uniqueData.target;
      continue; // 中断本次循环
    }

    // 数据不存在
    // 保存源数据，在拷贝数据中对应的引用
    uniqueList.push({
      source: data,
      target: res,
    });
    // =============

    for (let k in data) {
      if (data.hasOwnProperty(k)) {
        if (typeof data[k] === "object") {
          // 下一次循环
          loopList.push({
            parent: res,
            key: k,
            data: data[k],
          });
        } else {
          res[k] = data[k];
        }
      }
    }
  }

  return root;
}

function find(arr, item) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i].source === item) {
      return arr[i];
    }
  }

  return null;
}
```

# 原型

```js
/*
每个函数
都有 prototype 属性，除了 Function.prototype.bind() ，该属性指向原型。

每个对象
都有 __proto__ 属性，指向了创建该对象的构造函数的原型。
其实这个属性指向了[[prototype]]，但是[[prototype]] 是内部属性，我们并不能访问到，所以使用 _proto_ 来访问。

原型链
对象可以通过 __proto__ 来寻找不属于该对象的属性，__proto__ 将对象连接起来组成了原型链


Object 是所有对象的爸爸，所有对象都可以通过 __proto__ 找到它
Function 是所有函数的爸爸，所有函数都可以通过 __proto__ 找到它
Function.prototype 和 Object.prototype 是两个特殊的对象，他们由引擎来创建
除了以上两个特殊对象，其他对象都是通过构造器 new 出来的
函数的 prototype 是一个对象，也就是原型
对象的 __proto__ 指向原型， __proto__ 将对象和原型连接起来组成了原型链

Object.prototype 的 __proto__  属性是一个访问器属性（一个getter函数和一个setter函数）
 暴露了通过它访问的对象的内部[[Prototype]] (一个对象或 null)
*/

// 例子
function Shape() {
  console.log(this);
  this.x = 999;
  this.y = 123;
}

let newObj = new Shape();

// 函数的构造函数往上找
newObj.constructor; // function Shape()
Shape.constructor; // function Function()

// 函数的原型的构造函数是函数自己
Shape.prototype.constructor; // function Shape()
newObj.__proto__.constructor; // function Shape()
```

# 堆栈

{% asset_img 栈和堆.png %}

# apply-call-bind

```js
// 后面参数形式不一样而已
someFunc.apply(context, [1, 2, 3, 4]);
someFunc.call(context, 1, 2, 3, 4);

// bind 返回一个原函数的拷贝，并拥有指定的 this 值和初始参数
// function.bind(thisArg[, arg1[, arg2[, ...]]])
someFunc.bind(context, 1, 2, 3, 4);
```
