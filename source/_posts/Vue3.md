---
title: Vue3
abbrlink: 3a42db2d
date: 2021-08-15 20:18:49
tags: Vue
---

# Vue3

**Vue.js is a progressive JavaScript Framework**

## install

```bash
1. Via CDN
<script src="https://unpkg.com/vue@next"></script>

2. Scaffold via vite
npm init vite project-name -- --template vue
yarn create vite project-name --template vue

3. Scaffold via vue-cli
npm install -g @vue/cli
yarn global add @vue/cli
vue create project-name
```

## Lifecycles

**This is the complete life cycles of Vuejs, but the life cycle has changed in the setup in Vue3**

<img src="https://vue3js.cn/docs/zh/images/lifecycle.png" alt="Vuejs Lifecycles" style="zoom:40%;" />

## Command

### v-once

**The itself and its subcomponent are only renderd once, and the view is no longer update when if the value changes**

```vue
<div v-once>{{ counter }}</div>
<button @click="increment">+</button>
<button @click="decrement">-</button>
```

### v-text

```vue
<!-- Their effect is the same -->
<h2 v-text="message"></h2>
<h2>{{ message }}</h2>
```

### v-html

**Parse the string of HTML code**

```vue
<!-- message:<span style="color:red"> SmallStars </span> -->
<h2>{{ message }}</h2>
<h2 v-html>{{ message }}</h2>
```

### v-pre

**Skip the compilation process of itself and its child elements**

```vue
<!-- {{ message }} -->
<h2 v-pre>{{ message }}</h2>
```

### v-cloak

**Before the element is compiled, add the v-cloak class attribute**

```vue
<template>
  <div v-cloak>{{ counter }}</div>
</template>

<script>
export default {
  data() {
    return {
      counter: 100,
    };
  },
};
</script>

<style scoped>
[v-cloak] {
  display: none;
}
</style>
```

### v-bind

**Dynamic binding of element**

```vue
<a v-bind:src="aLink"></a>
<img :src="imgLink"></img>

<!-- binding class -->
<!-- class: actived title -->
<div :class="{actived: isActive, 'title': false}">div</div>
<!-- this 'title' will be parsed  -->
<div :class="[actived, 'title']">div</div>
<!-- class: actived title abc-->
<div :class="[actived, 'title', isAbc ? 'abc' : '']">div</div>
<div :class="[actived, 'title', {abc: isAbc}]">div</div>

<!-- binding style -->
<div style="color: red"></div>
<div :style="{color: 'red', 'font-size': '14px'}"></div>
<div :style="{color: 'red', fontSize: finalFontSize + 'px'}"></div>
<div :style="{color: 'red', fontSize: finalFontSize + 'px'}"></div>
<div :style="[{color: 'red'}, {fontSize: finalFontSize + 'px'}]"></div>

<!-- binding attributes -->
<!-- userName: 'name' -->
<!--
info: {
	nickName = 'BlackAngel',
	age = 19,
}
-->
<!-- <div name = 'smallstars'></div> -->
<div :[userName]='smallstars'></div>
<div :[userName]='smallstars'></div>
<div v-bind='info'></div>
<div :='info'></div>
```

### v-on

**Used to bind events and monitor**

```vue
<button v-on:click="btnClick">btn1</button>
<button @mousemove="mouseMove">btn2</button>

<!-- binding the expression -->
<button @mousemove="counter++">{{counter}}</button>
<!-- binding object listens to multiple events -->
<button v-on="{ click: btnClick, mousemove: mousemove }">{{counter}}</button>
<button @="{click: btnClick, mousemove: mousemove}">{{counter}}</button>
<!-- Incoming parameters -->
<!-- An event parameter will be passed in by default -->
<button @click="btnClick">btn1</button>
<!-- $event to get event parameter -->
<button @click="btnClick($event, 'smallstars')">btn1</button>

<!-- Use of modifiers -->
<!-- 
.stop: event.stopPropagation()
.prevent: event.preventDefault()
.capture: Event listener uses capture mode
.once: Trigger only once
.left: Only click the left mouse button to take effect
.right: Only click the right mouse button to take effct
.middle: Only click the middle mouse button to take effct
-->
<button @click.stop="btnClick">btn1</button>
```

### v-if v-else-if v-else

**v-if is lazy. When the condition is false, the component will not be rendered or will be destroyed**

```vue
<input type="text" v-model="score"></input>
<h2 v-if="score > 90">excellent</h2>
<h2 v-else-if="score > 60">qualified</h2>
<h2 v-else>failed</h2>
```

### v-show

**First of all, v-show does not support template. The difference from**

```vue
<h2 v-show="isShow">excellent</h2>
```

### v-for

**Dynamic rendering the data**

```vue
<!-- array -->
<ul>
  <li v-for="(item, index) in items">{{index}}: {{item}}</li>
</ul>
<!-- object -->
<ul>
  <li v-for="(value, key, index) in info">{{key}}: {{value}}</li>
</ul>
<!-- number -->
<ul>
  <li v-for="(num, index) in 100">{{index}}: {{num}}</li>
</ul>
```

## Diff

### Vnode

**The essence of VNode(Virtual Node) is a JavaScript Object**

```js
// <div class="title" style="font-size: 30px; color: red;">Vnode</div>
const vnode = {
  type: 'div',
  props: {
    class: "title",
    style: {
      "font-size": "30px",
      color: "red"
    }
  },
  children: "Vnode"
}
```

```mermaid
graph LR
  template --> VNode --> RealDOM
```

```html
<!-- First convert the template to virtual DOM, and then mount it to the real DOM -->
<div>
  <p>
    <i>1</i>
    <b>2</b>
  </p>
  <span>3</span>
  <strong>4</strong>
</div>
```

```mermaid
graph TB
	div --> p
	div --> span
	div --> strong
	p --> i
	p --> b
```

### Diff

#### Unkeyed

**Different letters  represent different nodes (The number is added here to facilitate drawing). The nodes are compared in turn, and if they are different, the nodes are updated. Finallym if there are more old nodes than new nodes, the remaining nodes are removed, otherwish all of them are added.**

```mermaid
graph TB
 a1-->a2
 b1-->b2
 c1--update-->f2 
 d1--update-->c2
 null--new-->d2
```

#### Keyed



## Responsive principle

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
const toProxy = new WeakMap(),
  toRaw = new WeakMap();

// 5.Create responsive Object
const createReactiveObject = (target) => {
  // 6.Judging the target type
  if (!isObject(target)) return target;

  // 14.If the object has been proxied, return the proxied object
  const proxy = toProxy.get(target);
  if (proxy) return proxy;
  if (toRaw.has(target)) return target;

  const baseHandle = {
    get: (target, key, receiver) => {
      console.log("get");
      // 7.Use Reflect to get the value
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
  // 7.Create the observer of target
  // Use the baseHandle(ProxyHandler) function to truncate the operation
  let observed = new Proxy(target, baseHandle);

  // 15.Mark the original object has been proxied
  toProxy.set(target, observed);
  toRaw.set(observed, target);

  // 8.Return the observed
  return observed;
};

// 4.Turn data into responsive
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



## Source Code

**Here will brief explain part of the source code**

1. [vue@3.2.2](https://unpkg.com/vue@3.2.2/dist/vue.global.js)
2. [Search the vue-next in Github](https://github.com/vuejs/vue-next)

### Diff

```typescript
// packages/runtime-core/src/renderer.ts
// diff
const patchChildren: PatchChildrenFn = (
  n1, // old nodes
  n2, // new nodes
  container,
  anchor,
  parentComponent,
  parentSuspense,
  isSVG,
  slotScopeIds,
  optimized = false
) => {
  const c1 = n1 && n1.children
  const prevShapeFlag = n1 ? n1.shapeFlag : 0
  const c2 = n2.children

  const { patchFlag, shapeFlag } = n2
  // fast path
  if (patchFlag > 0) {
    if (patchFlag & PatchFlags.KEYED_FRAGMENT) {
      // this could be either fully-keyed or mixed (some keyed some not)
      // presence of patchFlag means children are guaranteed to be arrays
      // keyed
      patchKeyedChildren(
        c1 as VNode[],
        c2 as VNodeArrayChildren,
        container,
        anchor,
        parentComponent,
        parentSuspense,
        isSVG,
        slotScopeIds,
        optimized
      )
      return
    } else if (patchFlag & PatchFlags.UNKEYED_FRAGMENT) {
      // unkeyed
      patchUnkeyedChildren(
        c1 as VNode[],
        c2 as VNodeArrayChildren,
        container,
        anchor,
        parentComponent,
        parentSuspense,
        isSVG,
        slotScopeIds,
        optimized
      )
      return
    }
  }

  // children has 3 possibilities: text, array or no children.
  if (shapeFlag & ShapeFlags.TEXT_CHILDREN) {
    // text children fast path
    if (prevShapeFlag & ShapeFlags.ARRAY_CHILDREN) {
      unmountChildren(c1 as VNode[], parentComponent, parentSuspense)
    }
    if (c2 !== c1) {
      hostSetElementText(container, c2 as string)
    }
  } else {
    if (prevShapeFlag & ShapeFlags.ARRAY_CHILDREN) {
      // prev children was array
      if (shapeFlag & ShapeFlags.ARRAY_CHILDREN) {
        // two arrays, cannot assume anything, do full diff
        patchKeyedChildren(
          c1 as VNode[],
          c2 as VNodeArrayChildren,
          container,
          anchor,
          parentComponent,
          parentSuspense,
          isSVG,
          slotScopeIds,
          optimized
        )
      } else {
        // no new children, just unmount old
        unmountChildren(c1 as VNode[], parentComponent, parentSuspense, true)
      }
    } else {
      // prev children was text OR null
      // new children is array OR null
      if (prevShapeFlag & ShapeFlags.TEXT_CHILDREN) {
        hostSetElementText(container, '')
      }
      // mount new if array
      if (shapeFlag & ShapeFlags.ARRAY_CHILDREN) {
        mountChildren(
          c2 as VNodeArrayChildren,
          container,
          anchor,
          parentComponent,
          parentSuspense,
          isSVG,
          slotScopeIds,
          optimized
        )
      }
    }
  }
}
```

```typescript
// unked
const patchUnkeyedChildren = (
  c1: VNode[],
  c2: VNodeArrayChildren,
  container: RendererElement,
  anchor: RendererNode | null,
  parentComponent: ComponentInternalInstance | null,
  parentSuspense: SuspenseBoundary | null,
  isSVG: boolean,
  slotScopeIds: string[] | null,
  optimized: boolean
) => {
  c1 = c1 || EMPTY_ARR
  c2 = c2 || EMPTY_ARR
  // get length of old nodes
  const oldLength = c1.length
  // get length of new nodes
  const newLength = c2.length
  // get the minimum length
  const commonLength = Math.min(oldLength, newLength)
  let i

  for (i = 0; i < commonLength; i++) {
    const nextChild = (c2[i] = optimized
      ? cloneIfMounted(c2[i] as VNode)
      : normalizeVNode(c2[i]))

    // Compare nodes one by one
    patch(
      c1[i],
      nextChild,
      container,
      null,
      parentComponent,
      parentSuspense,
      isSVG,
      slotScopeIds,
      optimized
    )
  }

  // if the number of new nodes is less
  if (oldLength > newLength) {
    // remove old
    unmountChildren(
      c1,
      parentComponent,
      parentSuspense,
      true,
      false,
      commonLength
    )
  } else {
    // mount new
    mountChildren(
      c2,
      container,
      anchor,
      parentComponent,
      parentSuspense,
      isSVG,
      slotScopeIds,
      optimized,
      commonLength
    )
  }
}
```

