---
title: vue2-vue3
abbrlink: 9359b45b
date: 2021-09-10 12:32:13
tags: vue
---

# Vue3 optimization

## Differences of Vue2 and Vue3

### Performance

- diff Algorithm
  - Vnodes in Vue2 will be completely compared
  - Vue3 adds a static flag(PatchFlat). While comparing, only nodes with static flags are compared, and the specific comparison content can be learned through the flags

![Vue2 diff]()

**In a word，The pointers on sides both are compared and move to the middle until oldCn or newCn traversed completely. When the object is too large will be slowly**

PatchFlag is roughly is divided into two types

1. Greater than 0，it's an element that can be optimized and updated during patchVNode or render
2. Less than 0, the element needs full diff

In this way, the element that don't need to update will only be created by once,and directly reused when render

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
// Patch flags can be combined using the | bitwise operator and can be checked using the & operator, e.g.
// const flag = TEXT | CLASS
// if (flag & TEXT)
export const enum PatchFlags {
  TEXT = 1,
  CLASS = 1 << 1,
  STYLE = 1 << 2,
  PROPS = 1 << 3,
  FULL_PROPS = 1 << 4,
  HYDRATE_EVENTS = 1 << 5,
  STABLE_FRAGMENT = 1 << 6,
  KEYED_FRAGMENT = 1 << 7,
  UNKEYED_FRAGMENT = 1 << 8,
  NEED_PATCH = 1 << 9,
  DYNAMIC_SLOTS = 1 << 10,
  // Special flags are not used in optimization
  HOISTED = -1,
  BAIL = -2,
}
```

- Cache of events

```react
<button @click="btnClick">SmallStars</button>
import { openBlock as _openBlock, createElementBlock as _createElementBlock } from "vue"
export function render(_ctx, _cache, $props, $setup, $data, $options){
  return (_openBlock(),
          _createElementBlock("button",
                              {onClick: _cache[0] || (_cache[0] = (...args) => (_ctx.btnClick && _ctx.btnClick(...args)))},
                              "SmallStars")
         )
} // Check the console for the AST
```

### Build of used

Vue3 introduces the [Tree shaking](https://developer.mozilla.org/zh-CN/docs/Glossary/Tree_shaking) feature, which removes useless code when packing, reduces the volume of the packaging, and reduces the execution time of the program

### Fragment, Teleport, Suspense

- Fragment

  **Vue2 instance only has only one root node, because it needs to be bound to a DOM element. Fragment label resolved this problem in Vue3, and it doesn't display in DOM**

- Teleport

  **Component Development encourages us build our UIs and actions to into components, these components are combined into a component tree. However, sometimes a part of components' template belong to those components logically, it would be preferable to move this part to somewhere else in DOM, outside of the Vue app. For example, the modal of full-screen.**

  ```vue
  <body>
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
    <div id="app"></div>
    // Add a new anchor point
    <div id="modal"></div>
    <!-- built files will be auto injected -->
  </body>

  app.component( 'modal-button', { template: `
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

  **Suspense is a component with slots. Before display content is fully rendered, the alternate content is displayed to occupy the place. If it's a asynchronous component, Suspense will wait for the components and use onErrorCaptured to capture the error**

  ```vue
  <template>
    <h1>{{ result }}</h1>
  </template>
  <script lang="ts">
  import { defineComponent } from "vue";
  export default defineComponent({
    setup() {
      //Suspense needs to return a Promise
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

### Custom Renderer API

Our template code will be converted into the html code through [createApp](https://vue3js.cn/global/createApp.html) of Vue. Also you can use this ability to render some other things what you need through Vue's [Custom Renderer API](https://ssr.vuejs.org/zh/api/#createrenderer)

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
    var textNode = document.createTextNode(text);
    node.appendChild(textNode);
  },

  createText(text) {
    return document.createTextNode(text);
  },
  /*...*/
});

export function createApp(rootComponent) {
  return renderer.createApp(rootComponent);
}
```

### The differences of lifecycle

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

## Responsive

![img](https://bytedance.feishu.cn/space/api/box/stream/download/asynccode/?code=MjA2ODFkMTEwYTIyNDIyZDdjMDVjYWZmOGI5ZjJlOTdfdThPcXMweDJacWtMSXdJaXl5SXZlSGJZS3dIaWNDeW5fVG9rZW46Ym94Y25KSzdrSVhNSFZMT0xaU2ZqNFQwU0plXzE2MzEyNDgwMTA6MTYzMTI1MTYxMF9WNA)

```js
// Vue3 Responsive principle
// utils
const isObject = (val) => {
  return !!val && typeof val === "object";
};

const dataLog = (title, arr) => {
  console.log(title);
  arr.forEach((i) => {
    console.log(i);
  });
};

/**
 * 13.To prevent the duplication of proxy
 * toProxy: {Original: Proxy}
 * toRaw: {Proxy: Original}
 */
// const info = { a: 1 };
// const b = reactive(info);
// reactive(b);
// reactive(info);

const toProxy = new WeakMap(),
  toRaw = new WeakMap();

// 4.Create responsive Object
const createReactiveObject = (target) => {
  // 5.Judging the target type
  if (!isObject(target)) return target;

  // 14.If the object has been proxied, return the proxied object
  const proxy = toProxy.get(target);
  if (proxy) return proxy;
  if (toRaw.has(target)) return target;

  const baseHandle = {
    get: (target, key, receiver) => {
      console.log("get");
      // 8.Use Reflect to get the value
      // return target[key]
      const res = Reflect.get(target, key, receiver);

      // 21.Collection dependencies
      // correspond the keys of current object width reactiveEffects
      track(target, key);

      // 9.consider the case of multi-level object
      return isObject(res) ? reactive(res) : res;
    },
    /**
     * target: original object ({name: 'smallstars'})
     * key: keyword (name)
     * value: new value
     * receiver: current proxied object (observer)
     */
    set: (target, key, value, receiver) => {
      console.log("set");
      // This way cannot determine whether the operation is successful
      // target[key] = value;
      // console.log(target, key, value, receiver);

      // 10.Use Reflect to determine the status of operation
      const res = Reflect.set(target, key, value, receiver);

      // 25.Determine whether it is a new attribute
      const hadKey = target.hasOwnProperty(key);
      // Avoid meaningless rendering views (eg: array.length)
      /*
        const arr = [1, 2, 3];
        let proxy = reactive(arr);
        // it will trigger operation twice. （1.push （2.length
        proxy.push(4);
      */
      if (!hadKey) {
        // data changed, need to rendering views
        // console.log("rendering");
        trigger(target, "add", key);
      } else {
        // property changed, don't need to rendering views
        // console.log("no rendering");
        trigger(target, "set", key);
      }

      // 11.Return the res
      return res;
    },
    // 12.Delete the property by key
    deleteProperty: (target, key) => {
      console.log("del");
      return Reflect.deleteProperty(target, key);
    },
  };
  // 6.Create the observer of target
  // Use the baseHandle(ProxyHandler) function to truncate the operation
  let observed = new Proxy(target, baseHandle);

  // 15.Mark the original object has been proxied
  toProxy.set(target, observed);
  toRaw.set(observed, target);

  // 7.Return the observed
  return observed;
};

// 3.Turn data into responsive
const reactive = (target) => {
  return createReactiveObject(target);
};

// collected of dependencies
// cache responsive effects
const activeEffectStack = [];

// if the target[key] changed, executed the effect
/*
targetsMap: {
  [target]: {
    [key]: deps;
  }
}
deps: [effects]
*/
// 22.use targetsMap to collect dependencies of per targets[key]
const targetsMap = new WeakMap();
const track = (target, key) => {
  // 23.Determine whether the stack is empty
  let effect = activeEffectStack[activeEffectStack.length - 1];

  // 24.Determine whether the targetsMap is empty, initialize it
  // console.log(target, key);
  if (effect) {
    let depsMap = targetsMap.get(target);
    if (!depsMap) targetsMap.set(target, (depsMap = new Map()));
    let deps = depsMap.get(key);
    if (!deps) depsMap.set(key, (deps = new Set()));

    if (!deps.has(effect)) deps.add(effect);
  }
};

// 26.Called the effect to update views
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
 * 19.Use stack cache fn(if this effect has other effect, there will be multiple values in the stack)
 * 1).push effect into stack
 * 2).execute fn
 * 3).pop the effect
 */
const run = (effect, fn) => {
  console.log("run");
  // prevent errors when fn is executed
  try {
    activeEffectStack.push(effect);
    // 20. execute fn
    fn();
  } finally {
    activeEffectStack.pop();
  }
};

// 18.Create responsive effect
const createReactiveEffect = (fn) => {
  let effect = () => {
    return run(effect, fn);
  };
  return effect;
};

// 17.Turn effect into responsive
const effect = (fn) => {
  let reactiveEffect = createReactiveEffect(fn);
  // executed firstly
  reactiveEffect();
  return reactiveEffect;
};

// let proxy = reactive({ name: "smallstars" });
// // This effect is executed once when the dependencies are collected, and then called when the subsequent data is updated.
// effect(() => {
//   console.log(1);
// });
// proxy.name = "blackangel";

// test
// const info = { name: { firstName: "small", lastName: "stars" } };
// let proxy = reactive(info);

// // We need to prevent duplication of proxy
// // use WeakMap to store it
// reactive(info);
// reactive(info);
// // reactive(info);

// const arr = [1, 2, 3];
// let proxy = reactive(arr);
// // it will trigger operation twice. 1.push 2.length
// proxy.push(4);

// 1.Defined the data (ref/reactive/computed)
const info = reactive({ counter1: 1, counter2: 1 });

// 16.Side effects of data changes
effect(() => {
  console.log("counter1:", info.counter1);
  // console.log("counter2:", info.counter2);
});

// 2.Change the data value
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

## References

> [Usage of bit masks](https://cloud.tencent.com/developer/article/1654981)

> [Vdom](https://vue-next-template-explorer.netlify.app)

> [patchFlags](https://vue3.w2deep.com/source-code/runtime/patchFlags.html)
