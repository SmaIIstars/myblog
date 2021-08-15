---
title: Event Flow in HTML
tags:
  - HTML
  - JavaScript
abbrlink: 7c75da10
date: 2021-03-29 22:41:37
---

# Event Flow in HTML

**An event is a specific moment of interaction that occurs in a document or brower window and act as a bridge between JavaScript and DOM. Event Flow describes the order in which events are received from the page.**

## W3C Event Flow

**The W3C specifies that the DOM event flow has three phases.**

1. The event capture phase.
2. The in-target phase.
3. The event bubbling phase.

![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/Event-Flow1.png)



## addEventListener

### Syntax

```js
target.addEventListener(type, listener [, options]);
target.addEventListener(type, listener [, useCapture]);
```

### Parameters

- **type** (String)

  A case-sensitive string representing the [event type](https://developer.mozilla.org/en-US/docs/Web/Events) to listen for.

- **listener** (Object, Function)

  An object that implements the EventListener interface, or a JavaScript function.

- **useCapture** (Boolean)

  The default value is false. If the value is false, the listener is executed during the bubbling phase. if true during the capture phase.



## Events  Executed Order 

**Capture stars at the outermost layer and bubbling from the response layer. But in the in-target phase, the order is determined by the code.**

<div
  id="event_flow_page"
  style="
    background-color: red;
    width: 150px;
    height: 150px;
    display: flex;
    align-items: center;
    justify-content: space-around;
    left: 0;
    right: 0;
    margin: 0 auto;
  "
>
  <div
    id="event_flow_content"
    style="
      background-color: blue;
      width: 75px;
      height: 75px;
      display: flex;
      align-items: center;
      justify-content: space-around;
    "
  >
    <button id="event_flow_buttom" style="background-color: white; color: black">
      execute
    </button>
  </div>
</div>

```js
// Event execution sequence is caputer -> aims -> bubble
// But the execution order in the aims is determined by the code order
const eventFlowPrint = (e, name, isCaputer) => {
  console.log(`${name}: ${isCaputer ? "Caputer" : "bubble"}`);
  // console.log(`${name}: ${isCaputer ? "Caputer" : "bubble"}`, e.target);
};

/*
	Code Order:
	page:			capture -> bubbling
	content:	bubbling -> capture
	buttom: 	bubbling -> capture
*/
const bindEventFlow = () => {
  // page
  document.getElementById("event_flow_page").addEventListener(
    "click",
    (e) => {
      eventFlowPrint(e, "page", true);
    },
    true
  );

  document.getElementById("event_flow_page").addEventListener(
    "click",
    (e) => {
      eventFlowPrint(e, "page", false);
    },
    false
  );

  // content
  document.getElementById("event_flow_content").addEventListener(
    "click",
    (e) => {
      eventFlowPrint(e, "content", false);
    },
    false
  );

  document.getElementById("event_flow_content").addEventListener(
    "click",
    (e) => {
      eventFlowPrint(e, "content", true);
    },
    true
  );

  // buttom
  document.getElementById("event_flow_buttom").addEventListener(
    "click",
    (e) => {
      eventFlowPrint(e, "buttom", false);
    },
    false
  );

  document.getElementById("event_flow_buttom").addEventListener(
    "click",
    (e) => {
      eventFlowPrint(e, "buttom", true);
    },
    true
  );
};
```



- Click Page

  ```js
  page: Caputer
  page: bubble
  ```

- Click Content

  ```js
  // In the in-target phase, the order is determined by the code.
  page: Caputer
  content: bubble
  content: Caputer
  page: bubble
  ```

- Click Buttom

  ```js
  // In the in-target phase, the order is determined by the code.
  page: Caputer
  content: Caputer
  buttom: bubble
  buttom: Caputer
  content: bubble
  page: bubble
  ```




## Event Agent

**Event agent users the event bubbling to manage all events of a certain type by  specifying only one event handler.**

```html
<ul id="list">
  <li>item 1</li>
  <li>item 2</li>
  <li>item 3</li>
  ......
  <li>item n</li>
</ul>
```

Bind the event to the tag 'ul' to manage the tag 'li' event. This reduces memory consumption and efficiency.




## References

[JS 事件冒泡和事件捕获](https://www.cnblogs.com/wubh/p/javascript-event-flow.html)

[addEventListener MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener)

[Event Type](https://developer.mozilla.org/en-US/docs/Web/Events)

