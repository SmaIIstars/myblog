# SmallStars's blog

## command

```bash
# hexo

## new layout(post, page, draft, scaffold)
#
#     post -> source/_posts
#     page -> source
#    draft -> source/_draft_
# scaffold -> template in scaffold
#
hexo new [layout] <title>

## publish draft to post
hexo publish [layout] <title>

```

## tips

1. post index

```js
// /node_modules/hexo-generator-index-pin-top/lib/generator.js
"use strict";
var pagination = require("hexo-pagination");
module.exports = function (locals) {
  var config = this.config;
  var posts = locals.posts;
  posts.data = posts.data.sort(function (a, b) {
    if (a.top && b.top) {
      // if(a.top == b.top) return b.date - a.date; // sort by date
      if (a.top == b.top) return b.updated - a.updated;
      // sort by edit date
      else return b.top - a.top; // sort by top
    } else if (a.top && !b.top) {
      // compare top & !top
      return -1;
    } else if (!a.top && b.top) {
      return 1;
    }
    // else return b.date - a.date; // sort by date
    else return b.updated - a.updated; // sort by edit date
  });
  var paginationDir = config.pagination_dir || "page";
  return pagination("", posts, {
    perPage: config.index_generator.per_page,
    layout: ["index", "archive"],
    format: paginationDir + "/%d/",
    data: {
      __index: true,
    },
  });
};
```
