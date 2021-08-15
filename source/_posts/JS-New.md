---
title: The JavaScript 'new' keyword
tags: JavaScript
abbrlink: de945f16
date: 2021-03-28 17:08:27
---

# The JavaScript 'new' keyword

**In JavaScript, the 'new' keyword is used to create an instance object of a class (mock class). When an object is instantiated, it inherits the properties and methods of the class.**

## Execution Process

1. Create a new and empty object.
2. Set the new object prototype to point to class constructor.
3. Execute the constructor, binding the 'this' keyword.
4. If the constructor does not return an object, it returns this. 

## Simulate the 'new' keyword

### use Object.create to simulate 'new' keyword

```js
const newNew = function (Parent, ...args) {
  let newObj = Object.create(Parent.prototype);
  let res = Parent.apply(newObj, args);
  return typeof res === "object" ? res : newObj;
};

let object1 = new Base();
let object2 = newNew(Base);

// true
console.log(object1.__proto__ === object2.__proto__);
```

### No Object.create to simulate 'new' keyword

```js
// newNew
const newNew1 = function (Parent, ...args) {
  // let newObject = {};
  let newObject = Object();
  newObject.__proto__ = Parent.prototype;
  let res = Parent.apply(newObject, args);
  return typeof res === "object" ? res : newObject;
};

let object1 = new Base();
let object2 = newNew(Base);

// true
console.log(object1.__proto__ === object2.__proto__);
```





## 'new'  keyword and Object.create()

**Their \_\_proto\_\_ points to different things. The 'new' point to the constructor's prototype object and create point to the Function.**

```js
// Object.create & new
function Base() {
  let num = 0;
  getNum = function () {
    console.log(num);
  };
}

let newObj = new Base();
let createObj = Object.create(Base);

console.log(newObj, createObj);
```

![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/JS-new.png)



## Use 'new' to simulate Object.create()

```js
const create1 = function (Parent) {
  let res = function () {};
  res.prototype = Parent;
  return new res();
};

let object3 = Object.create(Base);
let object4 = create1(Base);

// true
console.log(object3.__proto__ === object4.__proto__);	
```



## References

[JavaScript new 关键字](https://www.jianshu.com/p/4bbf0c582e97)

[MDN Object.create](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

[New 和 Object.create()的区别](https://blog.csdn.net/DepressedPrince/article/details/80909636)