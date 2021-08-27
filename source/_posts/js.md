---
title: js
date: 2021-04-22 16:00:49
tags: js
---

<!-- toc -->

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

# Object

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
