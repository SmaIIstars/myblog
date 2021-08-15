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

### VNode

**The essence of VNode(Virtual Node) is a JavaScript Object**

## Source Code

**Here will brief explain part of the source code**

1. [vue@3.2.2](https://unpkg.com/vue@3.2.2/dist/vue.global.js)
2. [Search the vue-next in Github](https://github.com/vuejs/vue-next)
