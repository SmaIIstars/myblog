---
title: Mustache
abbrlink: b1f95fde
date: 2021-08-15 21:13:09
tags: Mustache
---

# [Mustache](https://github.com/janl/mustache.js/)

**mustache.js is a zero-dependency implementation of the mustache template system in JavaScript. Mustache is a logic-less template syntax. It can be used for HTML, config files, source code - anything. It works by expanding tags in a template using values provided in a hash or object.**

## Useage in Vue

```vue
<template>
  <div>
    <!-- mustache -->
    <!-- 1. Basic usage -->
    <h2>{{ message }}</h2>

    <!-- 2. expression -->
    <h2>{{ counter * 10 }}</h2>

    <!-- 3. function -->
    <h2>
      {{ getReverseMessage() }}
    </h2>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      message: "Small Stars",
      counter: 100,
    };
  },
  methods: {
    getReverseMessage() {
      return this.message.split(" ").reverse().join(" ");
    },
  },
};
</script>

<style></style>
```

## References

- [Official Document](https://mustache.github.io/mustache.5.html)
- [学习笔记《Mustache》](https://www.jianshu.com/p/7f1cecdc27e1)
