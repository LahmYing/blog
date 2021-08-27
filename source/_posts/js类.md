---
title: js类
date: 2021-04-08 10:44:17
tags: js
---

<!-- toc -->

# es6

{% asset_img class_es6.png 1000 1000 %}

```js
class Father {
  // 内部都挂在 prototype 上
  // === Father.prototype.constructor
  constructor(x, y) {
    console.log("new.target", new.target);
    // new.target 指向 当前的类
    if (new.target === Father) {
      this.x = x; // 实例属性
      this.y = y;
      this.#x = x; // 私有属性
      this.#y = y;
    } else {
      throw new Error("必须使用 new 命令生成实例");
    }
  }

  // 实例方法
  // === Father.prototype.toString
  toString() {
    return "(" + this.x + ", " + this.y + ")";
  }

  // reset 属性的存取行为
  get prop() {
    return "getter";
  }
  set prop(value) {
    console.log("setter: " + value);
  }

  // 属性名，可以采用表达式
  [methodName]() {
    // ...
  }

  // 静态属性
  static staticProp = 1;
  // 静态方法
  static classMethod() {
    // 这里的 this 指向 class
    // 实例属性、方法内的 this 指向实例
    return "hello";
  }

  // 私有方法
  #sum() {
    return this.#x + this.#y;
  }
}

class Son extends Father {
  constructor(x, y) {
    super(x, y); // 调用父类的 constructor(x, y)
    console.log("new.target", new.target);
  }
  superToString() {
    return super.toString();
  }
  static classMethod() {
    return super.classMethod() + ", 从super对象上调用父类静态方法";
  }
}

```

# es5

```js
function Person() {
  // 静态属性，类独享，只能这样调用 Person.name
  this.name = "张三";
  this.age = 20;
  // 实例方法
  this.run = function () {
    console.log(this.name + "在运动");
  };
}
// 实例属性
Person.prototype.sex = "男"; 
// 实例方法
Person.prototype.work = function () {
  console.log(this.name + "在工作");
};
// 静态方法，类独享
Person.getInfo = function () {
  console.log("我是静态方法");
};

let p = new Person();
console.log(p.name);
p.run();
p.work();

```
