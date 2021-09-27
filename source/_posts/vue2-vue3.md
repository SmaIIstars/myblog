---
title: vue2-vue3
abbrlink: 9359b45b
date: 2021-09-10 12:32:13
tags:
---

# Vue3 优化简述

## Vue2 与 Vue3 的区别

1. 性能比 Vue2 高
2. 按需打包，打包体积更小
3. [Composition API](https://bytedance.feishu.cn/docs/doccnlTJKSq2edPrLtucFHlQgYe) （类似 React hooks）
4. Fragment, Teleport, Suspense
5. 更友好的支持兼容 TS
6. Custom Renderer API（自定义渲染 API）

### 性能更高

- diff 算法优化
  - Vue2 中 Vnode 全量比较
  - Vue3 新增静态标记（patch flag），对比时只对比有静态标记的节点，可通过标记得知具体的比较内容

![img](https://bytedance.feishu.cn/space/api/box/stream/download/asynccode/?code=MjVmMTBiNmM1ZmEwMGVhMTk5Mzc0YTY3YmEzOGZmZWFfTmgyd2Zac1ZRcUhSOVVRd3hvOEg2RGxzSnBWMTQweFRfVG9rZW46Ym94Y25VWUJjNUF6aVVlVVcxVDVJVk9CZHBjXzE2MzEyNDgwMTA6MTYzMTI1MTYxMF9WNA)

Vue2 的双端比较

简单概括就是，两边指针向中间移动进行比较（详细见[Vue diff 算法](https://bytedance.feishu.cn/docs/doccnK9Zm2M6otPVkghVYt39cIf#)）一直到 oldCh 或 newCh 一个遍历完。在 Vue2 中，虚拟 dom 是一个全量比较的过程，当项目过大的时候就会有 VNode 更新缓慢的问题。

Vue3 中使用在动态标签末尾加 Patch Flag 去解决这个问题，Patch Flag 大致分为两种：

1. 大于 0，在 patchVNode 或 render 时是可以被优化生成或更新的元素
2. 小于 0，元素需要 full diff

通过这种方式，对于不参与更新的元素，只会创建一次，渲染的时候直接复用。

```html
<div>Hello World!</div>
<div>{{msg}}</div>
<div>SmallStars</div>
```

```js
import {
  createElementVNode as _createElementVNode,
  toDisplayString as _toDisplayString,
  Fragment as _Fragment,
  openBlock as _openBlock,
  createElementBlock as _createElementBlock,
} from "vue";

const _hoisted_1 = /*#__PURE__*/ _createElementVNode(
  "div",
  null,
  "Hello World!",
  -1 /* HOISTED */
);
const _hoisted_2 = /*#__PURE__*/ _createElementVNode(
  "div",
  null,
  "SmallStars",
  -1 /* HOISTED */
);

export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (
    _openBlock(),
    _createElementBlock(
      _Fragment,
      null,
      [
        _hoisted_1,
        _createElementVNode(
          "div",
          null,
          _toDisplayString(_ctx.msg),
          1 /* TEXT */
        ),
        _hoisted_2,
      ],
      64 /* STABLE_FRAGMENT */
    )
  );
}
```

```ts
export const enum PatchFlags {
  // 动态文字内容
  TEXT = 1,
  // 动态 class
  CLASS = 1 << 1,
  // 动态样式
  STYLE = 1 << 2,
  // 动态 props
  PROPS = 1 << 3,
  // 有动态的key，也就是说props对象的key不是确定的
  FULL_PROPS = 1 << 4,
  // 合并事件
  HYDRATE_EVENTS = 1 << 5,
  // children 顺序确定的 fragment
  STABLE_FRAGMENT = 1 << 6,
  // children中有带有key的节点的fragment
  KEYED_FRAGMENT = 1 << 7,
  // 没有key的children的fragment
  UNKEYED_FRAGMENT = 1 << 8,
  // 只有非props需要patch的，比如`ref`
  NEED_PATCH = 1 << 9,
  // 动态的插槽
  DYNAMIC_SLOTS = 1 << 10,
  // 以下是特殊的flag，不会在优化中被用到，是内置的特殊flag
  // 表示他是静态节点，他的内容永远不会改变，对于hydrate的过程中，不会需要再对其子节点进行diff
  HOISTED = -1,
  // 用来表示一个节点的diff应该结束
  BAIL = -2,
```

1. 很容易进行复合，我们可以通过`TEXT|CLASS`来得到`0000000011`，而这个值可以表示他即有`TEXT`的特性，也有`CLASS`的特性
2. 方便进行对比，我们拿到一个值`FLAG`的时候，想要判断他有没有`TEXT`特性，只需要`FLAG & TEXT > 0`就行
3. 方便扩展，在足够位数的情况下，我们新增一种特性就只需要让他左移的位数加一就不会重复

> [PatchFlag 原理](https://cloud.tencent.com/developer/article/1654981)

> [Vue3 官方演示 Vdom 网站](https://vue-next-template-explorer.netlify.app)

> [patchFlags 优化](https://vue3.w2deep.com/source-code/runtime/patchFlags.html)

- 事件侦听器缓存

默认情况 onClick 被视为动态绑定，每次都会动态追踪变化，事实上是同一函数，所以直接缓存复用即可。

```vue
<button @click="btnClick">SmallStars</button>
import { openBlock as _openBlock, createElementBlock as _createElementBlock }
from "vue" export function render(_ctx, _cache, $props, $setup, $data, $options)
{ return (_openBlock(), _createElementBlock("button", { onClick: _cache[0] ||
(_cache[0] = (...args) => (_ctx.btnClick && _ctx.btnClick(...args))) },
"SmallStars")) } // Check the console for the AST
```

### 按需打包

Vue3 引入[Tree shaking](https://developer.mozilla.org/zh-CN/docs/Glossary/Tree_shaking)特性，在打包的时候去除掉无用的代码，减少打包的体积，减少程序的执行时间。

### Fragment, Teleport, Suspense

- Fragment

在 Vue2 中，只允许有一个根结点，原因是因为一个 Vue 组件的实例需要绑定到一个 DOM 元素上，在 Vue3 中引入了 Fragment 去解决这个问题，使 template 中可以有多个根元素。并且 Fragment 是一个虚拟元素，他不会在 DOM 中呈现，我们不需要再多创一个 DOM 包裹节点。

- Teleport

组件化开发，鼓励我们将功能组件化，将相关的 UI 与交互进行封装并支持引入其他组件，以此形成一颗组件树。但是，有时组件模板的一部分逻辑上属于该组件，而从技术角度来看，最好将模板的这一部分移动到 DOM 中 Vue app 之外的其他位置。比如全屏弹窗

```vue
<body>
  <noscript>
    <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
  </noscript>
  <div id="app"></div>
  //先行定义一个锚点
  <div id="modal"></div>
  <!-- built files will be auto injected -->
</body>
app.component('modal-button', { template: `
<button @click="modalOpen = true">
        Open full screen modal! (With teleport!)
    </button>

<teleport to="modal">
      <div v-if="modalOpen" class="modal">
        <div>
          <span> Content </span>
          <button @click="modalOpen = false">Close</button>
        </div>
      </div>
    </teleport>
`, data() { return { modalOpen: false } } })
```

- Suspense

Suspense 只是一个具有插槽的组件，在展示组件完全渲染之前，显示备用内容进行占位。如果是异步组件，Suspense 可以等待组件，并使用 onErrorCaptured 捕获错误

```vue
<template>
  <h1>{{ result }}</h1>
</template>
<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  setup() {
    //使用Suspense需要返回一个对象
    return new Promise((resolve) => {
      setTimeout(() => {
        return resolve({
          result: "10000",
        });
      }, 3000);
    });
  },
});
</script>
<Suspense>
  <template #default>
    <async-show />
  </template>
  <template #fallback>
    Loading...
  </template>
</Suspense>
```

### 自定义渲染函数

vue 官方实现的 createApp 会给我们的 template 映射生成 html 代码，但是要是你不想渲染生成 html ，而是要渲染生成到 canvas 之类的不是 html 的代码的时候，那就需要用到 Custom Renderer API 来定义自己的 render 渲染生成函数了。

在 vue2 中我们使用 new 的方式去创建一个 Vue 实例

```js
new Vue({
  render: (h) => h(App),
  router,
  store,
}).$mount("#app");
```

Vue3 中我们使用 createApp 去创建并返回一个实例

```js
const app = createApp(App);
app.use(router);
app.use(store);
app.mount("#app");
```

https://vue3js.cn/global/createApp.html

https://ssr.vuejs.org/zh/api/#createrenderer

```ts
import { createRenderer } from "@vue/runtime-core";
const renderer = createRenderer({
  createElement(type) {
    let element;
    switch (type) {
      case "div":
        element = document.createElement("div");
        break;
    }
    return element;
  },
  setElementText(node, text) {
    // 例(html)
    var textNode = document.createTextNode(text);
    node.appendChild(textNode);
  },

  createText(text) {
    return document.createTextNode(text);
  },
  /*...*/
});

// 最后导出 createApp （没错 就是Vue3.0创建根对象那个createApp）
export function createApp(rootComponent) {
  return renderer.createApp(rootComponent);
}
```

### 生命周期的变化

![img](https://bytedance.feishu.cn/space/api/box/stream/download/asynccode/?code=NmQxZDk0MDU1YWRjMTQ0YmJkNzNhODFkNmM5ZTMzM2RfN2ZQbzlNWU1iaktrQjNGRWpRTE80NmNPd0E3QUdRNnpfVG9rZW46Ym94Y25uZm5qR1lkOHR2a1pKcDRTa0tBZDlnXzE2MzEyNDgwMTA6MTYzMTI1MTYxMF9WNA)

| Vue2          | Vue3            |
| ------------- | --------------- |
| beforeCreate  | setup           |
| created       | setup           |
| beforeMount   | onBeforeMount   |
| mounted       | onMounted       |
| beforeUpdate  | onBeforeUpdate  |
| updated       | onUpdated       |
| beforeDestroy | onBeforeDestroy |
| destroyed     | onDestroyed     |
| errorCaptured | onErrorCaptured |

## 响应式

![img](https://bytedance.feishu.cn/space/api/box/stream/download/asynccode/?code=MjA2ODFkMTEwYTIyNDIyZDdjMDVjYWZmOGI5ZjJlOTdfdThPcXMweDJacWtMSXdJaXl5SXZlSGJZS3dIaWNDeW5fVG9rZW46Ym94Y25KSzdrSVhNSFZMT0xaU2ZqNFQwU0plXzE2MzEyNDgwMTA6MTYzMTI1MTYxMF9WNA)

vue3 中的响应式原理简单实现（reactive）

```js
// Vue3 响应式原理
// 工具函数
const isObject = (val) => {
  return !!val && typeof val === "object";
};

/**
 * 13.防止重复代理
 * 1）原对象被多次代理
 * toProxy: {原对象: 代理对象}
 * 2）代理过的对象重复代理
 * toRaw: {代理对象: 原对象}
 */
const toProxy = new WeakMap(),
  toRaw = new WeakMap();

// 5.创建一个响应式对象
const createReactiveObject = (target) => {
  // 6.判断 target 是否是一个对象
  if (!isObject(target)) return target;

  // 14.如果对象已经被代理，或已经是一个代理对象，返回代理对象或本身
  const proxy = toProxy.get(target);
  if (proxy) return proxy;
  if (toRaw.has(target)) return target;

  const baseHandle = {
    get: (target, key, receiver) => {
      console.log("get");
      // 7.使用 Reflect 获取值, Reflect 有返回值
      // return target[key]
      const res = Reflect.get(target, key, receiver);

      // 21.依赖收集
      // 通过当前对象和其关键字收集相应依赖
      track(target, key);

      // 9.考虑当前是否仍然是一个的对象（递归调用
      return isObject(res) ? reactive(res) : res;
    },
    /**
     * target: 原对象 ({name: 'smallstars'})
     * key: 关键字 (name)
     * value: 新值
     * receiver: 当前代理对象，可以改变所指向的 this (观察者)
     */
    set: (target, key, value, receiver) => {
      console.log("set");
      // 这种方式不能确定是否取值成功
      // target[key] = value;

      // 10.根据 Reflect 的返回值确定相应的操作
      const res = Reflect.set(target, key, value, receiver);

      // 25.判断是否是新增属性
      // 避免无意义的视图渲染 (eg: array.length)
      const hadKey = target.hasOwnProperty(key);
      if (!hadKey) {
        // 数据变换需要更新视图
        trigger(target, "add", key);
      } else {
        // 已有属性变更, 不需要更新视图
        trigger(target, "set", key);
      }

      // 11.返回 res
      return res;
    },
    // 12.通过关键字删除相应属性
    deleteProperty: (target, key) => {
      console.log("del");
      return Reflect.deleteProperty(target, key);
    },
  };
  // 7.对原对象创建一个观察者
  // 使用 baseHandle(ProxyHandler) 函数去拦截操作
  let observed = new Proxy(target, baseHandle);

  // 15.标记已经被代理过
  toProxy.set(target, observed);
  toRaw.set(observed, target);

  // 8.返回观察者
  return observed;
};

// 4.将数据转化成响应式对象
const reactive = (target) => {
  return createReactiveObject(target);
};

// 依赖收集
// 缓存响应式 effect
const activeEffectStack = [];

// 如果 target[key] 变换后, 执行相对应的副作用
/*
targetsMap: {
  [target]: {
    [key]: deps;
  }
}
deps: [effects]
*/
// 22.使用 targetsMap 去收集每个 targets[key] 所对应的依赖的副作用
const targetsMap = new WeakMap();
const track = (target, key) => {
  // 23.判断响应式副作用栈中是否有存在副作用
  let effect = activeEffectStack[activeEffectStack.length - 1];

  // 24.是否是第一次，初始化
  if (effect) {
    let depsMap = targetsMap.get(target);
    if (!depsMap) targetsMap.set(target, (depsMap = new Map()));
    let deps = depsMap.get(key);
    if (!deps) depsMap.set(key, (deps = new Set()));

    // 在对应的 deps 中加入相应的副作用
    if (!deps.has(effect)) deps.add(effect);
  }
};

// 26.调用相应的副作用（刷新视图
const trigger = (target, type, key) => {
  const depsMap = targetsMap.get(target);
  if (depsMap) {
    const deps = depsMap.get(key);
    if (deps) {
      deps.forEach((effect) => {
        effect();
      });
    }
  }
};

/**
 * 19.使用栈缓存相应的函数
 * 1).push effect into stack
 * 2).execute fn
 * 3).pop the effect
 */
const run = (effect, fn) => {
  console.log("run");
  // 防止函数执行的时候出错终端
  try {
    // 先将副作用缓存到栈中
    // 如果在一个副作用中还存在其他副作用, 栈中将会有多个值
    activeEffectStack.push(effect);
    fn();
  } finally {
    // 无论是否执行成功，栈都需要弹出当前执行的函数
    activeEffectStack.pop();
  }
};

// 18.创建一个响应式的副作用
const createReactiveEffect = (fn) => {
  let effect = () => {
    // 这里将自己传入是为了将自己缓存到栈中
    return run(effect, fn);
  };
  return effect;
};

// 17.将副作用转为响应式
const effect = (fn) => {
  let reactiveEffect = createReactiveEffect(fn);
  // 并会有第一次调用
  reactiveEffect();
  return reactiveEffect;
};

// 1.定义一个数据 (ref/reactive/computed)
const info = reactive({ counter1: 1, counter2: 1 });

// 16.定义数据改变后的副作用
effect(() => {
  console.log("counter1:", info.counter1);
  // console.log("counter2:", info.counter2);
});

// 2.改变值
setInterval(() => {
  info.counter1++;
}, 3000);

// setInterval(() => {
//   info.counter2 *= 2;
// }, 1000);
```

## script setup

https://github.com/vuejs/rfcs/blob/master/active-rfcs/0040-script-setup.md

## Proxy & Reflect

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

https://bytedance.feishu.cn/docs/doccnVB2CKT5JGgZ1o1TmhlCKkc#
