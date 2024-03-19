---
title: JS Question
tags: JavaScript
abbrlink: a511bb1e
date: 2021-03-20 19:49:45
---

# JavaScript Question

**Some interview questions and answers.**

## QA 1: getName

**Write out the value of each output.**

### Code

```js
function Foo() {
  getName = function () {
    console.log(1);
  };
  // console.log("this is" + this);
  return this;
}
Foo.getName = function () {
  console.log(2);
};
Foo.prototype.getName = function () {
  console.log("baidu" && "google");
};
var getName = function () {
  console.log(4);
};
function getName() {
  console.log(5);
}

Foo.getName();
getName();
Foo().getName();
getName();
new Foo.getName();
new Foo().getName();
new new Foo().getName();
```

```js
// 2
Foo.getName();
// 4	Both var and function declaration are promoted, andt the function is overwritten before the variable is promted.
getName();
// 1
Foo().getName();
// 1	Foo() return this is window, widown.getName has been rewritten.
getName();
// 2	According the rule, it's the same as new (Foo.getName)(), so the answer is 2.	[Foo.getName() --> new]
new Foo.getName();
// google	(new Foo()).getName(), the Foo object constructor does not have getName, find it in prototype object.	[Foo.getName --> Foo.prototype.getName]
new Foo().getName();
// google	new((new Foo()).getName)()
new new Foo().getName();
```

### Tips

1. Operator precedence

   ![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/JS-QA1-1.png)

2. For '&&', if the former is true, then execute the latter, otherwish only execute the former. For '||', if the former is true, only execute the former, the latter don't have to execute, otherwish have to execute the latter.

### References

[The JavaScript 'new' keyword](http://www.smallstars.top/myblog/posts/de945f16/)

## QA 2: Array Flattening

**Flat the structure of multiple nested arrays.**

### Code

```js
// ...
// This method only expend one layer.
const flatten1 = (arr) => {
  return [].concat(...arr);
};

// join and split
// This method will involve implicit conversion, some values will be affected like undefined, null,etc.
const flatten2 = (arr) => {
  return arr.join(",").split(",");
};

// toString and split
// This method will involve implicit conversion, some values will be affected like undefined, null,etc.
const flatten3 = (arr) => {
  return arr.toString().split(",");
};

// Recursion and reduce
const flatten4 = (arr) => {
  return arr.reduce((pre, cur) => {
    // we can filter some unwanted values.
    if (cur === undefined || cur === null) {
      return pre;
    }
    return Array.isArray(cur) ? pre.concat(flatten4(cur)) : pre.concat(cur);
  }, []);
};

// ES6
const flatten5 = (arr) => {
  // es6 flat method only expand one layer by default
  // return arr.flat()
  return arr.flat(Infinity);
};
```

## QA 3: Sleep

**Use Promise to encapuslate a Sleep function.**

### Code

```js
const sleep = (delay) => {
  return new Promise((res) => {
    setTimeout(res, delay);
  });
};

sleep(1).then(console.log(1));
```

## QA 4: Time Interval Output

**Output the values in array at regular interval.**

### Code

```js
const closureFunction = (arr, delay) => {
  for (var i = 0; i < arr.length; i++) {
    ((i) => {
      setTimeout(() => {
        console.log(arr[i]);
      }, i * delay);
    })(i);
  }
};

const setTimeoutFunction = (arr, delay) => {
  for (let i = 0; i < arr.length; i++) {
    setTimeout(() => {
      console.log(arr[i]);
    }, i * delay);
  }
};

const setIntervalFunction = (arr, delay) => {
  let index = 0;

  timer = setInterval(() => {
    console.log(arr[index]);
    index === arr.length ? clearInterval(timer) : index++;
  }, delay);
};
```

## QA 5: Currying

**Currying can convert a multi-argument function to a single-argument function and return a new function which can take the remaining arguments and return a result.**

```js

```

## QA 6: DeepCopy

```js
// Recursion
const deepCopy = (val) => {
  let newObj;
  if (typeof val === "object") {
    if (Array.isArray(val)) {
      newObj = [];
      for (let i of val) {
        newObj.push(deepCopy(i));
      }
    } else if (val === null) {
      return val;
    } else if (val.constructor === RegExp) {
      return val;
    } else {
      newObj = {};
      for (let i in val) {
        newObj[i] = deepCopy(val[i]);
      }
    }
  } else {
    // It isn't an object
    return val;
  }

  return newObj;
};

// Object.assign is shallow copy, but the target is a new Object.
const deepCopy1 = (val) => {
  return Object.assign({}, val);
};

// This can't copy the undefined, function and RegExp etc
// if the value is null, it will report the error.
const deepCopy2 = (val) => {
  return JSON.parse(JSON.stringify(val));
};
```
