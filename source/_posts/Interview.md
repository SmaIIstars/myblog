---
title: Interview
tags: JavaScript
abbrlink: 4decbe97
date: 2020-12-18 15:27:44
top:
---

# JavaScript

## 数据类型

- 字符串（String）
- 数值（Number）
- 布尔型（Boolean）
- 空（Null）
- 未定义（Undefined）
- 对象（Object）

## 数组方法

1. push：在最后压入
2. pop：弹出最后一位
3. shift：删除最后一位
4. unshift：在开头压入
5. join：拼接
6. sort：排序
7. concat：和参数在原本的数组上创建新的副本并返回
8. reverse：翻转
9. splice：删除、插入、替换
10. slice：截取
11. indexOf：查询下标
12. forEach：遍历
13. map：映射
14. every：判断每个值是否满足条件
15. some：判断是否存在满足条件的值
16. reduce：值聚合

## JS 深拷贝的原因和方法

> 原因
>
> > 基本数据类型：在栈中直接赋予值引用数据类型
> >
> > 对象和函数：值在堆当中，赋予地址

> 方法

## 操作 DOM 的方法

## JS 原型和原型链

## 函数执行过程

## async 和 await /promise

## 闭包

## 变量提升

## Bom 和 Dom 区别

> Bom： browser Object Model
>
> - 与浏览器相关：描述与浏览器相关操作的方法和接口
> - 无标准
> - 根本对象是 window
>
> Dom：Document Object Model
>
> - 与 Html 文档相关
> - W3C 标准
> - 根本对象是 window.doucment

## Event Loop

[参考文献](https://segmentfault.com/a/1190000016278115)

## 代码题

- 斐波那契数列

```javascript
//基础
function fib(number) {
  number -= 1;
  if (number <= 2) {
    return 1;
  }
  let f = 1;
  let g = 1;
  while (number--) {
    g = g + f;
    f = g - f;
  }
  return f;
}

function fib2(num) {
  if (n == 1 || n == 2) {
    return 1;
  }
  return fib2(num - 1) + fib2(num - 2);
}

//大数(100超位)
function bigNum() {
  this.min = [];
  this.max = [];
  this.res = [];

  bigNum.prototype.add = function (a, b) {
    let str1, str2;
    let temp;
    let addFlag = 0;

    str1 = a.split("").reverse();
    str2 = b.split("").reverse();
    // console.log(str1);

    if (parseInt(a) > parseInt(b)) {
      this.max = str1;
      this.min = str2;
    } else {
      this.max = str2;
      this.min = str1;
    }

    let minLength = this.min.length;
    let maxLength = this.max.length;

    for (let i = 0; i < maxLength; i++) {
      if (i < minLength) {
        temp = parseInt(this.min[i]) + parseInt(this.max[i]) + addFlag;
        // console.log(temp);
      } else {
        temp = parseInt(this.max[i]) + addFlag;
        // console.log(temp);
      }
      if (parseInt(temp) > 9) {
        this.res[i] = temp % 10;
        addFlag = parseInt(temp / 10);
        if (i == maxLength - 1) {
          this.res[maxLength] = 1;
        }
      } else {
        this.res[i] = temp;
        addFlag = 0;
      }
    }

    // console.log(this.max);
    // console.log(this.min);
    // console.log(this.res);
    return this.res.reverse();
  };

  bigNum.prototype.toResString = function () {
    let rstr = "";
    let len = this.res.length;
    for (let i = 0; i < len; i++) {
      rstr += this.res[i].toString();
    }
    return rstr;
  };
}

function fib1(index) {
  if (index <= 2) {
    return 1;
  }

  let a = "1";
  let b = "1";
  let sum = new bigNum();
  for (let i = 2; i < index; i++) {
    sum.add(a, b);
    a = b;
    b = sum.toResString();
    // console.log(sum.res);
    // console.log(a);
    // console.log(i);
  }

  return sum;
}

console.log(fib1(100).toResString());
```

- 排序
- 去重
- 统计字符串中最多字符

# ES6

## var、let、const

- 相当于全局变量和局部变量
- var 没有块级作用域且可以重复定义
- let 有块级作用域 (if/for)，可以不初始化
- const 有块级作用域 (if/for)，必须初始化

## 继承

```javascript
// es6继承
class Animal {
  //构造函数，里面写上对象的属性
  constructor(props) {
    this.name = props.name || "Unknown";
  }
  //方法写在后面
  eat() {
    //父类共有的方法
    console.log(this.name + " will eat pests.");
  }
}

//class继承
class Bird extends Animal {
  //构造函数
  constructor(props, myAttribute) {
    //props是继承过来的属性，myAttribute是自己的属性
    //调用实现父类的构造函数
    super(props); //相当于获得父类的this指向
    this.type = props.type || "Unknown"; //父类的属性，也可写在父类中
    this.attr = myAttribute; //自己的私有属性
  }

  fly() {
    //自己私有的方法
    console.log(this.name + " are friendly to people.");
  }
  myattr() {
    //自己私有的方法
    console.log(this.type + "---" + this.attr);
  }
}

//通过new实例化
var myBird = new Bird(
  {
    name: "小燕子",
    type: "Egg animal", //卵生动物
  },
  "Bird class"
);
myBird.eat();
myBird.fly();
myBird.myattr();
```

# CSS

## 垂直水平居中

1. translate
2. flex
3. padding、margin

## px、em、rem 区别

- px：像素单位
- em：相对于当前文本的字体尺寸，默认为浏览器默认字体尺寸，会继承父元素的字体大小
- rem：相对于 HTML 根元素

## BFC

## clearfix

## 权重顺序

## position 三种属性

# HTML

## 元素类型

- 块元素
- 行内块
- 置换元素

## 语义化标签

# Vue

## 生命周期

[生命周期博客](https://www.cnblogs.com/smallstars/p/13172926.html)

## ref 使用在 Dom 和 组件 上有什么区别？

- Dom：获拿到的就是 Dom 元素
- 组件上：拿到的是一个组件

## Vue 有哪些指令

1. v-text
2. v-html
3. v-pre
4. v-cloak
5. v-once
6. v-if、v-else-if、v-else
7. v-show
8. v-for
9. v-bind
10. v-model
11. v-on

## v-show 和 v-if 的区别

## Vuex

## VueRouter

### 权限判断

1. token
2. 请求拦截
3. 导航守卫

## 组件通信方法

## 响应式原理

## computes 和 watch

# 数据库

## 三大范式

- 第一范式：每列属性具有原子性（不可再分）
- 第二范式：每一个元组的唯一性，不存在部分依赖（所有属性依赖一个主键）
- 第三范式：每一个元组的任何字段不能有其他的候选键（降低数据冗余性）

# 设计模式

# 小知识点

## MVC（Model、View、Controller）

![](https://upload-images.jianshu.io/upload_images/2002187-4d82bf5244e9d66e.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

## MVVM（Model、View、ViewModel）

把 Controller 的数据和逻辑处理部分从中抽离出来，专门使用 ViewModel 进行管理，再进行数据绑定即可

![](https://upload-images.jianshu.io/upload_images/2002187-ddcaae06ec00dadb.png?imageMogr2/auto-orient/strip|imageView2/2/w/673/format/webp)

## 浏览器适配

## 分辨率适配

## 强缓存和协商缓存

## map 和 forEach

## Http 和 Https

## cookies、sessionStorage 和 localStorage 解释及区别

## 怎么判断一个变量是对象还是数组

## Vue 如何阻止事件冒泡（总是阻止和只阻止一次）

## get 和 post 区别

## 状态码

# Byte Dance

## 输入 url 到展示出页面的过程

- 输入网址：历史记录、书签等地方进行匹配

- DNS 解析：hosts => 本地 DNS 服务器 => DNS 根服务器 => 域服务器 => 存储映射

- 建立 TCP 连接：TCP 三次握手 四次挥手

  - TCP 三次握手:ACK(Acknowledgement),SYN(Synchronous),seq(Sequence number),ack(Acknowledge number)

  ```mermaid
  sequenceDiagram
  	participant c as Client
  	participant s as Server

  	c->>s:SYN=1,seq=x
  	Note right of c:I like you
  	s->>c:SYN=1,ACK=1,ack=x+1,seq=y
  	Note left of s:me too
  	c->>s:ACK=1,ack=y+1,seq=z
  	Note right of c:Great! Let's get together

  ```

  - 四次挥手

  ```mermaid
  sequenceDiagram
  	participant c as Client
  	participant s as Server

  	c->>s:FIN=1,seq=x
  	Note right of c:I want close
  	Note left of s:OK, I got it, let me get ready
  	s->>c:ACK=1,ack=x+1,seq=y

  	s-->c:Data still can transferred during this period

  	s->>c:ACK=1,ack-x+1,seq=z
  	Note left of s:Okay, I'm ready
  	Note right of c:OK
  	c->>s:ACK=1,ack=z+1,seq=w

  ```

- 客户端发送 HTTP (HTTPS) 请求

  - HTTP (明文传输)

    - 1.0：**非持久连接** 每个 TCP 只能串行 HTTP 请求，每次请求都会重新发起一个新的 TCP 连接
    - 1.1：**持久连接** 每个 TCP 也只能串行 HTTP 请求，最大连接数为 6，模拟并行请求
    - 2.0：将多个 TCP 连接合并发送，发送的时候可以乱序发送，接收后根据帧头部标识符重组

  - HTTPS

    - 加密

      - 对称加密

        Client & Server 使用同一种加密

        ```mermaid
        sequenceDiagram
        	participant c as Client
        	participant s as Server

        	c-->s: Use the same key
        	c-->s: Client send data to Server
        	c->>c:data = encryptFunction(key, data)
        	c->>s:data
        	s->>s:decryptFunction(key,data)

        	c-->s: Server send data to Client
        	s->>s:data = encryptFunction(key, data)
        	s->>c:data
        	c->>c:decryptFunction(key,data)

        ```

      - 非对称加密

        ```mermaid
        sequenceDiagram
        	participant c as Client(pk1,sk1)
        	participant s as Server(pk2,sk2)

        	c-->s:Everyone knows the public key
        	c-->s:Only the owner konws the private key
        	c->>s:request the public key
        	s->>c:pk2
        	Note left of c:Client get the pk2
        	c->>c:key = encryptFunction(pk2,key)
        	Note left of c:Use the pk2 encrypt a key
        	c->>s:key
        	s->>s:key = decryptFunction(pk2,key)
        	Note right of s:Use the pk2 decrypt the key
        	s->>c:OK
        	c-->s:Exchange the data

        ```

        Hacker Attach

        ```mermaid
        sequenceDiagram
        	participant c as Client(pk1,sk1)
        	participant h as Hacker(pk3,sk3)
        	participant s as Server(pk2,sk2)

        	Note over c,s: Hacking
        	c->>+h: request public key
        	h->>-c: pk3
        	h->>+s: request public key
        	s->>-h: pk2

        	Note over c,s: Hacker get the pk2 and Client get the hacker's pk3
        	c->>+h: Agreement key eg: key1
        	h->>-c: OK
        	h->>+s: Agreement key eg: key1
        	s->>-h: OK

        	Note over c,s: Hacker get the agreement key, and can decrypt encrypted content
        	c->>+h: send data
        	h->>-s: send data
        	s->>+h: send data
        	h->>-c: send data

        ```

    - CA (Certificate Authority)验证: Prevent the public key obtained by the client from being fake

    - 对称+非对称+CA

      ```mermaid
      sequenceDiagram
      	participant c as Client(pk1,sk1)
      	participant h as Hacker(pk3,sk3)
      	participant s as Server(pk2,sk2)
      	participant ca as Certificate Authority(cpk,csk)

      	Note right of ca:Generate certificate
      	ca->>ca: license = f(csk,pk)
      	c->>c: The certificate is embedded in the operating system
      	h->>h: The certificate is embedded in the operating system
      	s->>s: Use certificate when deploying
      	c-->ca: Issue certificate, All have the certifficate

      	c->>s: Request the certificate
      	s->>c: Send the certificate
      	c->>c: Use the pck decrypt the pk2
      	Note left of h:Hacker don't know the csk
          c->>h: Encrypt data
      ```

    - 协商 key 的具体过程

      ```mermaid
      sequenceDiagram
      	participant c as Client(pk1,sk1)
      	participant s as Server(pk2,sk2)

      	c->>s: ①. I support the SSL version, algorithm, seq, etc.
      	s->>c: ②. Use the SSL x.x.x and xxx algorithm, ACK, seq, license etc.
      	c->>c: ③. Check the license
      	c->>s: ④. seq, a = hash(①,②), etc.
      	s->>s: ⑤. hash(①,②) === a ? key = SSLalgorithm(①,②,③) : error
      	s->>c: ⑥. seq, b = hash(①,②,④), etc.
      	c->>c: ⑦. hash(①,②,④) === b? key = SSLalgorithm(①,②,③) : error
      ```

- 服务器处理请求

- 服务器相应请求

  - 状态码

    > 2xx：请求成功
    >
    > > 200：成功返回，正常抓取
    > >
    > > 204：成功处理，但未返回任何数据
    > >
    > > 206：部分请求成功
    > >
    > > eg:
    > >
    > > 1. 浏览器要下载文件，先弹窗询问用户
    > > 2. 在线视频，边下边看
    > > 3. 断点续传
    >
    > 3xx：重定向
    >
    > > 301：**永久**重定向
    > >
    > > 302：**临时**重定向
    > >
    > > 304：资源未更新，不用再抓取，使用缓存即可
    >
    > 4xx：请求出错
    >
    > > 400：请求语法错误
    > >
    > > 401：用户无权限，需要身份验证
    > >
    > > 403：服务器拒绝请求
    > >
    > > 404：资源不存在
    > >
    > > 410：资源永久删除
    >
    > 5xx：服务器错误
    >
    > > 500：服务器内部错误 (后端报错)
    > >
    > > 503：服务器超载，暂时无法访问
    > >
    > > 505：web 不支持此 HTTP 协议版本

## OSI(Open System Interconnect)

- 七层模型

  |    OSI     |         Introduction         |
  | :--------: | :--------------------------: |
  |   应用层   |      为应用程序提供服务      |
  |   表示层   |      数据格式转化和加密      |
  |   会话层   |     建立、管理和维护会话     |
  |   传输层   | 建立、管理和维护端到端的连接 |
  |   网络层   |      IP 选择和路由选择       |
  | 数据链路层 |    提供介质访问和链路管理    |
  |   物理层   |           物理设备           |

- 五层模型

  |    OSI     |       Introduction       |
  | :--------: | :----------------------: |
  |   应用层   |          应用层          |
  |   传输层   |     四层交换机、路由     |
  |   网络层   |    路由器、三层交换机    |
  | 数据链路层 | 网桥、以太网交换机、网卡 |
  |   物理层   |  中继器、集线器、双绞线  |

## 子网掩码

- 子网掩码的 1、0 是连续的(未明确规定，但强烈建议，可以避免出现某些未知错误)
- 左边 1 的数量是网络位的长度
- 右边 0 的数量是主机位的长度
- /xx: 表示 1 的数量(192.168.1.0/26 ==> 子网掩码为 255.255.255.192 (26 个 1))

[案例](https://mp.weixin.qq.com/s/jAITB4o1nnO5M2wt0hDqjw)

```javascript
ip:				 192.168.1.0/26			11000000 10101000 00000001 00 000000
ip:				 192.168.1.64/26		11000000 10101000 00000001 01 000000
ip:				 192.168.1.128/26		11000000 10101000 00000001 10 000000
ip:				 192.168.1.192/26		11000000 10101000 00000001 11 000000
子网掩码:		  255.255.255.192		 11111111 11111111 11111111 11 000000

进行 & 运算 在同一个网段下就可以直接通信
192.168.1.192/26 和 192.168.1.1/26		  false
192.168.1.200/26 和 192.168.1.199/26 	true
```

## JavaScript

### ["1", "2", "3"].map(parseInt) what & why?

```js
// Array<string>.map<number>(callbackfn: (value: string, index: number, array: string[]) => number), thisArg?: any): number[]
// function parseInt(s: string, radix?: number): number
// Radix is 0 and string argument not beging with '0x' or '0' are treated as radix is 10
// "1" => parseInt("1", 0) => paresInt("1", 10) => 1
// "2" => parseInt("2", 1) => NaN, because the radix is smaller than string
// "3" => parseInt("3", 2) => NaN, because the radix is smaller than string
console.log(["1", "2", "3"].map(parseInt)); // [1, NaN, NaN]
```

### 树的深度和广度遍历

```js
// Tree
function Tree() {
  function Node(x) {
    this.value = x;
    this.children = [];
  }

  this.root = null;

  createTree = function (treeObj) {
    if (!treeObj) return;
    let root = new Node(treeObj.value);
    if (treeObj.children.length > 0) {
      let { children } = treeObj;
      children.forEach((item) => {
        root.children.push(this.createTree(item));
      });
    }
    return root;
  };
  Tree.prototype.create = function (treeObj) {
    this.root = createTree(treeObj);
  };

  // deepTraversal
  deepTraversalFc = function (rootNode, nodes) {
    if (!rootNode) return;
    nodes.push(rootNode.value);
    rootNode.children.forEach((item) => {
      this.deepTraversalFc(item, nodes);
    });
    return nodes;
  };
  Tree.prototype.deepTraversal = function (rootNode) {
    let res = [];
    deepTraversalFc(rootNode, res);
    return res;
  };

  // wideTraversal
  wideTraversalFc = function (rootNode, stack, nodes) {
    if (!rootNode) return;
    if (stack.length === 0) {
      stack.push(rootNode);
      nodes.push(rootNode.value);
    }
    let node = stack.shift();
    if (node.children.length > 0) {
      node.children.forEach((item) => {
        stack.push(item);
        nodes.push(item.value);
      });
      wideTraversalFc(node, stack, nodes);
    }
    return nodes;
  };

  Tree.prototype.wideTraversal = function (treeNode) {
    let res = [];
    let stack = [];
    wideTraversalFc(treeNode, stack, res);
    return res;
  };
}

// 构建树
/**
 *             ┌-------------1-------------┐
 *             │             │             │
 *             2             3             4
 *       ┌-----┴-----┐       │       ┌-----┼-----┐
 *       5           6       7       8     9     10
 */
const treeObj = {
  value: 1,
  children: [
    {
      value: 2,
      children: [
        {
          value: 5,
          children: [],
        },
        {
          value: 6,
          children: [],
        },
      ],
    },
    {
      value: 3,
      children: [
        {
          value: 7,
          children: [],
        },
      ],
    },
    {
      value: 4,
      children: [
        {
          value: 8,
          children: [],
        },
        {
          value: 9,
          children: [],
        },
        {
          value: 10,
          children: [],
        },
      ],
    },
  ],
};
let tree = new Tree();
tree.create(treeObj);
console.log(tree.wideTraversal(tree.root));
```

### async/await、Promise 和 setTimeout 执行顺序

```js
// Event Loop
// 1 4 10 5 6 7 2 3 9 8
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

// 1 4 7 5 2 3 6
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

// Return of await as a then()
// 4 1 3 6 8 2 7 5
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
```

### 数组扁平化加去重

```js
let arr = [[1, 2, 2], [3, 4, 5, 5], [6, 7, 8, 9, [11, 12, [12, 13, [14]]]], 10];

const flat = (arr) => {
  return arr.reduce((pre, cur) => {
    return Array.isArray(cur) ? pre.concat(flat(cur)) : pre.concat(cur);
  }, []);
};
let res = flat(arr);

console.log(
  res
    .filter((item, index) => {
      return res.indexOf(item) === index;
    })
    .sort((a, b) => {
      return a - b;
    })
);
```

### JS 异步解决方案

1. callback
2. promise
3. generator
4. async/await

### ["A1", "A2", "B1", "B2", "C1", "C2", "D1", "D2"] + ["A", "B", "C", "D"] => ["A1", "A2", "A", "B1", "B2", "B", "C1", "C2", "C", "D1", "D2", "D"]

```js
// charAt(pos: number): string (Return the character at the specified index)
// charCodeAt(index: number): number (charrcter => ASCII)
const concatArr = (...arrs) => {
  let res = [];
  arrs.forEach((item) => {
    res = res.concat(item);
  });
  return res;
};

const arr1 = ["A1", "A2", "B1", "B2", "C1", "C2", "D1", "D2"];
const arr2 = ["A", "B", "C", "D"];
console.log(
  concatArr(arr1, arr2).sort(
    (a, b) => a.charAt(0).charCodeAt() - b.charAt(0).charCodeAt()
  )
);
```

### 如何实现一个 new

1. 创建一个新对象，将他的引用赋给 this, 继承函数原型
2. 通过 this 将属性和方法赋给新对象
3. this 指向新对象(实例), 如果没有则手动返回其他对象

```js
const newNew = function (Parent, ...args) {
  let newObj = Object.create(Parent.prototype);
  let res = Parent.apply(newObj, args);

  return typeof res === "object" ? res : newObj;
};
```

### var a = ?; if(a == 1 && a == 2 && a == 3){ console.log(1); }

```js
// Object: When object compared to a base type, valueOf and to toString function are called
var a = {
  i: 1,
  valueOf/toString: function () {
    return this.i++;
  },
};

// Array: When array calls toString function will call the join function (separated by ',') and overwrite the join function
var a = [1, 2, 3];
a.join = a.shift;
```

### 变量提升

```js
// first find internally for variable declarations
// have ? internal variable : external search
// IFFE can be assigned
var a = 10;
(function () {
  console.log(a);
  a = 5;
  console.log(window.a);
  var a = 20;
  console.log(a);
})();
// undefined 10 20

var a = 10;
(function () {
  console.log(a);
  a = 5;
  console.log(window.a);
  a = 20;
  console.log(a);
})();
// 10 5 20

var a = 10;
(function a() {
  console.log(a);
  a = 5;
  console.log(window.a);
  a = 20;
  console.log(a);
})();
// fa 10 fa

var a = 10;
(function a() {
  console.log(a);
  a = 5;
  console.log(window.a);
  var a = 20;
  console.log(a);
})();
// undefined 10 20
```

### ArrayLike

```js
var obj = {
  2: 3,
  3: 4,
  length: 2,
  splice: Array.prototype.splice,
  push: Array.prototype.push,
};
// push is to insert value according to length
obj.push(2);
obj.push(3);
console.log(obj); // Object(4) [empty × 2, 2, 3, splice: ƒ, push: ƒ]
```

### 实现 (5).add(3).minus(2) 功能

```js
Number.prototype.numAdd = function (num) {
  return this.valueOf() + num;
};

Number.prototype.numMinus = function (num) {
  return this.valueOf() - num;
};

console.log((5).numAdd(3).numMinus(2));
```

### . 和 = 优先级

```js
var a = { n: 1 };
var b = a;
// a & b => {n:1}
a.x = a = { n: 2 };
// . has a higher priority than =
// olda.x & b => {n: 1, x: undefined}
// newa = {n: 2}
// olda.x & b = newa => {n: 1, x: {n: 2}}
console.log(a.x); // undefined
// newa.x => undefined
console.log(b.x); // { n: 2 }
// b => {n: 1, x: {n:2}}
```

### 随机生成一个长度为 10 的整数类型的数组，例如 [2, 10, 3, 4, 5, 11, 10, 11, 20]，将其排列成一个新数组，要求新数组形式如下，例如 [[2, 3, 4, 5], [10, 11], [20]]

```js
const generatorArr = (length, maxNumber = 100) =>
  Array.from({ length }, () => Math.floor(Math.random() * maxNumber));
let arr = generatorArr(20, 45).sort((a, b) => a - b);
const res = (arr) => {
  let res = [],
    flag = 0;
  let subres = [];
  arr.forEach((element) => {
    if (Math.floor(element / 10) === flag) {
      subres.push(element);
    } else {
      flag++;
      res.push(subres);
      subres = [];
      subres.push(element);
    }
  });
  // the last resArray
  res.push(subres);
  return res;
};

console.log(arr);
console.log(res(arr));
```

### 如何把一个字符串的大小写取反(大写变小写, 小写变大写), 例如 ’AbC' 变成 'aBc'

```js
let str = "DfffdsADFFfsSFDasf";
let processString = (str) => {
  let res = "";
  for (let i = 0; i < str.length; i++) {
    res +=
      str[i] === str[i].toUpperCase()
        ? str[i].toLowerCase()
        : str[i].toUpperCase();
  }
  return res;
};

console.log(str);
console.log(processString(str));
```

### 实现一个字符串匹配算法，从长度为 n 的字符串 S 中，查找是否存在字符串 T, T 的长度是 m，若存在返回所在位置

```js
let s = "abcdefghijk";
let t = "efg";
let find = (s, t) => {
  let n = s.length,
    m = t.length;
  if (n < m) return;
  for (let i = 0; i < s.length; i++) {
    if (s.slice(i, i + m) === t) return i;
  }
};

console.log(find(s, t));
```

### BFC、IFC、GFC 和 FFC

### 对象的键名

1. 只能是 String 或 Symbol
2. 其他都会转 String
3. 调用 toString

```js
var a = {},
  b = "123",
  c = 123;
a[b] = "b";
console.log(a); // {"123": "b"}
a[c] = "c";
console.log(a); // {"123": "c"}
console.log(a[b]); // c

var a = {},
  b = Symbol("123"),
  c = Symbol("123");
a[b] = "b";
console.log(a); // {Symbol("123"): "b"}
a[c] = "c";
console.log(a); // {Symbol("123"): "b", {Symbol("123"): "c"}}
console.log(a[b]); // b

var a = {},
  b = { key: "123" },
  c = { key: "456" };
a[b] = "b";
console.log(a); // {[object Object]: "b"}
a[c] = "c";
console.log(a); // {[object Object]: "c"}
console.log(a[b]); // c
```

### 版本号排序

- 字符串比较法 (适用范围小)

  ```js
  // ASCII: {.: 46, 0: 48}
  let versions = ["0.1.1", "2.3.3", "0.302.1", "4.2", "4.3.5", "4.3.4.5"];
  const versionSort = (versions) => {
    versions.sort((a, b) => a - b);
    return versions;
  };
  console.log(versionSort(versions));

  // 0.5.0 > 0.302.1 is error
  ```

- 大数加权法

  ```js
  // ["0.1.1", "0.5.1", "0.302.1", "2.3.3", "4.2", "4.3.4.5", "4.3.5"]
  const arr = ["0.5.1", "0.1.1", "2.3.3", "0.302.1", "4.2", "4.3.5", "4.3.4.5"];

  let p = 1000;
  let gen = (arr, maxLen) => {
    let res = 0;
    arr.forEach((element, index) => {
      res += Number(element) * Math.pow(p, maxLen - 1 - index);
    });
    return res;
  };

  let versionSort = (array) => {
    let arr = array;
    let maxLen = Math.max(...arr.map((item) => item.split(".").length));
    arr.sort((a, b) => gen(a.split("."), maxLen) - gen(b.split("."), maxLen));
    return arr;
  };

  console.log(versionSort(arr));

  // Large number overflow easily
  ```

- 循环比较法 (推荐)

  ```js
  const arr = ["0.5.1", "0.1.1", "2.3.3", "0.302.1", "4.2", "4.3.5", "4.3.4.5"];

  let versionSort = (array) => {
    let arr = array;
    arr.sort((a, b) => {
      let arr1 = a.split(".");
      let arr2 = b.split(".");

      let index = 0;

      while (true) {
        let s1 = arr1[index];
        let s2 = arr2[index++];

        if (!s1 || !s2) {
          return s1 - s2;
        }
        if (s1 === s2) continue;

        return s1 - s2;
      }
    });
    return arr;
  };
  ```

- 二维矩阵从左上角到右下角路径的最小和（动态规划）

  ```JS
  const arr = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
    [10, 11, 12],
  ];

  const res = (arr) => {
    let [n, m] = [arr.length, arr[0].length];

    let dp = [];

    let tempArr = [];
    tempArr.push(arr[0][0]);
    for (let i = 1; i < m; i++) {
      tempArr.push(tempArr[i - 1] + arr[0][i]);
    }
    dp.push(tempArr);

    for (let i = 1; i < n; i++) {
      tempArr = [];
      tempArr.push(dp[i - 1][0] + arr[i][0]);
      dp.push(tempArr);
    }

    for (let i = 1; i < n; i++) {
      for (let j = 1; j < m; j++) {
        dp[i].push(Math.min(dp[i - 1][j], dp[i][j - 1]) + arr[i][j]);
      }
    }

    return dp[n - 1][m - 1];
  };

  console.log(res(arr));
  ```
