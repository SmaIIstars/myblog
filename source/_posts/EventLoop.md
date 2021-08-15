---
title: EventLoop
abbrlink: df38ec5a
date: 2021-04-04 21:09:45
tags:
---

# Event Loop

**The JS monitor process determines the order in which different event are executed. This is the Event Loop**



## Basic knowledge of JS

- JS is a sigle line scripting language. So it avoids the paradoxical problem of working with the same DOM at the same time.

- The monitor process will constantly check if the main thread execution stack is empty. and if it is, it will go to the Event Queue to check if there are any functions waiting to be called.

- All synchronized tasked are executed on the main thread, forming an execution stack.

- Synchronous

  Tasks queued for execution on the main thread which must wait the previous task to complete.

- Asynchronous

  Tasks enter the task queue but don't enter the main thread cannot enter the main thread until the task queue notifies the main thread that an asynchronous task is ready.

  - setTimeout
  - setInterval
  - ajax
  - promise
  - I/O

- Micro Task
  - promise.then
  - promise.nextTick (node)
- Macro Task
  - The overall script
  - setTimeout
  - setInterval



## Event Loop

![EventLoop](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/EventLoop1.png)

1. With the overall scripty as the first macro task starting, splite the all task into two parts. Synchronous tasks and asynchronous tasks.
2. Synchronous tasks will be executed enter the main thread right now.
3. Asynchronous tasks will splite into micro and macro tasks.
4. Macro and micro tasks will enter the different Event Table, and moves the registered callback function for the specified event to different event queue.
5. When the main thread is idle, it will executed the callback functions in the Event Queue of microtask and the macro task in order.



## Code

### Case 1

```js
const event1 = () => {
  console.log(1);

  setTimeout(() => {
    console.log(2);
    Promise.resolve().then(() => {
      console.log(3);
    });
  });

  new Promise((resolve, reject) => {
    console.log(4);
    resolve(5);
  }).then((data) => {
    console.log(data);

    Promise.resolve()
      .then(() => {
        console.log(6);
      })
      .then(() => {
        console.log(7);

        setTimeout(() => {
          console.log(8);
        }, 0);
      });
  });
  
  setTimeout(() => {
    console.log(9);
  });

  console.log(10);
};

// 1 4 10 5 6 7 2 3 9 8
event1()
```

| Micro | Macro |        Output        |
| :---: | :---: | :------------------: |
|   5   |  2,9  |        1,4,10        |
|       | 2,9,8 |     1,4,10,5,6,7     |
|   3   |  9,8  |    1,4,10,5,6,7,2    |
|       |   8   |  1,4,10,5,6,7,2,3,9  |
|       |       | 1,4,10,5,6,7,2,3,9,8 |

### Case 2 

```js
const event2 = () => {
  console.log(1);

  setTimeout(() => {
    console.log(2);
    Promise.resolve().then(() => {
      console.log(3);
    });
  });

  new Promise((resolve, reject) => {
    console.log(4);
    resolve(5);
  }).then((data) => {
    console.log(data);
  });

  setTimeout(() => {
    console.log(6);
  });

  console.log(7);
};

// 1 4 7 5 2 3 6
event2()
```

### Case 3

```js
const event3 = () => {
  async function async1() {
    console.log("1");
    await async2();
    console.log("2");
  }
  async function async2() {
    console.log("3");
  }

  console.log("4");

  setTimeout(() => {
    console.log("5");
  }, 0);

  async1();

  new Promise(function (reslove) {
    console.log("6");
    reslove();
  }).then(function () {
    console.log("7");
  });
  console.log("8");
};

// Return of await as a then()
/*
await async2();
console.log("2");
is same as
new Promise((resolve) => {
      console.log("3");
      resolve("2");
    }).then((res) => {
      console.log(res);
    });
*/
// 4 1 3 6 8 2 7 5
event3()
```



## References

[js运行机制详解（Event Loop）](https://www.jianshu.com/p/e06e86ef2595)