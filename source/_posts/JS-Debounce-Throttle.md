---
title: Debounce And Throttle
tags: JavaScript
abbrlink: "48951515"
date: 2021-03-20 16:46:18
---

# Debounce And Throttle

**They are all done the closing package**

- Debounce

  The same action is triggered consecutively over a period of time, executing only the last triggered action.

- Throttle

  The same action is triggered consecutively over a period of time, executing the action at each interval.

![Debounce and Throttle](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/Debounce&Throttle.png)

## Principle of implementation

### Debounce

```js
const debounce = (fn, delay) => {
  let timer = null;

  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn(args);
    }, delay);
  };
};
```

### Throttle

```js
// timer
const throttleTimer = (fn, delay) => {
  let timer = null;

  return (...args) => {
    if (!timer) {
      timer = setTimeout(() => {
        fn(args);
        timer = null;
      }, delay);
    }
  };
};

// timestamp
const throttleTimestamp = (fn, delay) => {
  let startTime = Date.now();

  return (...args) => {
    let curTime = Date.now();
    let remainTime = delay - (curTime - startTime);
    if (remainTime < 0) {
      fn(args);
      startTime = Date.now();
    }
  };
};

// time + timestamp
const throttle = (fn, delay) => {
  let timer = null;
  let startTime = Date.now();

  return () => {
    let curTime = Date.now();
    let remainTime = delay - (curTime - startTime);
    clearTimeout(timer);
    if (remainTime > 0) timer = setTimeout(fn, delay);
    else {
      fn(...args);
      startTime = Date.now();
    }
  };
};
```

## References

[详解防抖函数（debounce）和节流函数（throttle）](https://www.cnblogs.com/aurora-ql/p/13757733.html)
