---
title: js
date: 2021-04-22 16:00:49
tags: js
---

<!-- toc -->

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

# array.map 是否改变原数组

map 不修改调用它的原数组本身（当然可以在 callback 执行时改变原数组）

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
