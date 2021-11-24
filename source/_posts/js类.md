---
title: class
date: 2021-04-08 10:44:17
tags: js
category: [js]
---

<!-- toc -->

# es6

class 内部都挂在 prototype 上，方便继承

```js
class Father {
  // 实例属性方法新写法，无需放在 constructor 内
  // [methodName]() {} // methodName 可采用表达式
  // toString() { // === Father.prototype.toString
  //   return "(" + this.x + ", " + this.y + ")";
  // }

  // 内部都挂在 prototype 上
  // === Father.prototype.constructor
  constructor(x, y) {
    console.log("new.target", new.target);
    // new.target 指向 当前的类
    if (new.target === Father) {
      this.x = x; // 实例属性
      this.y = y;
      // this.#x = x; // 私有属性
      // this.#y = y;
    } else if (typeof new.target !== "function") {
      throw new Error("必须使用 new 命令生成实例");
    }
  }

  // reset 属性的存取行为
  get prop() {
    return "getter";
  }
  set prop(value) {
    console.log("setter: " + value);
  }

  // 静态属性
  static staticProp = 1;
  // 静态方法
  static classMethod() {
    // 这里的 this 指向 class
    // 实例属性、方法内的 this 指向实例
    this.staticProp = this.staticProp + "staticProp";
    return "hello";
  }

  // 私有方法
  // #sum() {
  //   return this.#x + this.#y;
  // }
}

let f = new Father("X", "Y");
console.log(f);

console.log(Father === Father.prototype.constructor); // true
console.log("f.prop ===", f.prop);

class Son extends Father {
  constructor(x, y) {
    // 调用父类的 constructor(x, y)
    // super 作为函数 super() 只能在子类构造函数中使用
    super(x, y);
    console.log("new.target", new.target);
  }
  superToString() {
    // 在实例方法中，super 指向 Father.prototype
    return super.toString();
  }
  static classMethod() {
    // 静态方法中，super 指向 Father
    return super.classMethod() + ", 从super对象上调用父类静态方法";
  }
}
```

# es5

```js
function Person() {
  // 内部变量，内部可访问
  insideProperty = "insideProperty";
  // 实例属性
  this.name = "张三";
  this.age = 20;
  // 实例方法
  this.run = function () {
    console.log(this.name + "在运动");
  };
}
// 原型属性，实例可使用
Person.prototype.sex = "男";
// 原型方法，实例可使用
Person.prototype.work = function () {
  console.log(this.name + "在工作");
};
// 静态方法，类独享
Person.getInfo = function () {
  console.log("静态方法，类独享");
};

let p = new Person();
let p2 = new Person();
console.log(Person.getInfo());
console.log("p.name", p.name);
p2.name = "p2-name";
console.log("p.name 是否被 p2 影响", p.name === p2.name, p.name);
p.run();
p.work();
Person.insideProperty; // undefined
p.insideProperty; // undefined
```
