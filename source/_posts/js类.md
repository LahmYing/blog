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

# super 语法糖

```js
class A {
  constructor() {
    this.f2 = () => console.log("a.f2"); // 原型链上的属性
    this.f3.aa = "A"; // 原型链上的属性
  }
  f() {
    console.log("a.f");
  } //  A 的静态属性
  f1() {
    console.log("a.f1");
  } //  A 的静态属性
  f3() {} //  A 的静态属性
}

class B extends A {
  constructor() {
    super();

    super.f = () => console.log("b.super.f");
    this.f(); // 'b.super.f'  // B 不允许修改 A 的静态属性, 会被强制改成 this.f()
    super.f(); // 'a.f'       // B 访问 A 的静态属性

    this.f2(); // 'a.f2'      // 直接继承 原型链上的属性
  }
}

// super 其实就是 B._proto_ 加上 B.prototype._proto_ = A.prototype
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
