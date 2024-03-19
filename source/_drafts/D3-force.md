---
title: D3-force
tags: d3.js
mathjax: true
---

# D3-force

**[D3.js](https://github.com/d3/d3) is a web standards-based JavaScript visualization library that uses SVG, Canvas and HTML for data visualization. In data visualization, we often use graph to express the information contained in data. They can convenient for us to clearly sort out the connections between various nodes and quickly extract useful information. Graphical layout algorithms can make the scattered information clear and conform to the corresponding aesthetic standards. (The information is mostly carried in the relationship of points and edges).**

**Force-guided layout is based on physical theory, simulation nodes as atoms, and finally reaching equilibrium through the interaction of forces between atoms, reducing the overlap between nodes through mutual repulsion between nodes, but it is inevitable that all nodes will be overlapping in a limited space.**

![d3-force-example-img](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/d3-force-example-img.png)

## Principle

### Initialize the nodes

Import the nodes into the d3 and wrap them around the genesis node with a certain radius and angle.

```js
// d3-force/simulation.js
var initialRadius = 10,
  initialAngle = Math.PI * (3 - Math.sqrt(5));

function initializeNodes() {
  for (var i = 0, n = nodes.length, node; i < n; ++i) {
    (node = nodes[i]), (node.index = i);
    if (node.fx != null) node.x = node.fx;
    if (node.fy != null) node.y = node.fy;
    // initial nodes
    if (isNaN(node.x) || isNaN(node.y)) {
      var radius = initialRadius * Math.sqrt(0.5 + i),
        angle = i * initialAngle;
      node.x = radius * Math.cos(angle);
      node.y = radius * Math.sin(angle);
    }
    // (vx, vy) is the initial velocity of (x, y)
    if (isNaN(node.vx) || isNaN(node.vy)) {
      node.vx = node.vy = 0;
    }
  }
}

initializeNodes();
```

## References

- [d3.js Github](https://github.com/d3/d3-force)
- [d3-force Example](https://jsfiddle.net/smallstars/h8y6e031/51/)
