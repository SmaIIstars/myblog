---
title: ACM Problems
tags: algorithm
abbrlink: 643e93d7
date: 2021-04-17 17:10:39
mathjax: true
---

# ACM Problems

**Here are some of the problems I encountered in ACM.**

## Array

### [Merge Intervals](https://leetcode-cn.com/problems/merge-intervals/)

```js
// Intervals that can be merged must be contiguous.
var merge = function (intervals) {
  // Sort the intervals and start from the minimum value.
  intervals.sort((a, b) => a[0] - b[0]);
  let res = [];

  for (let i = 0; i < intervals.length; i++) {
    let len = res.length;
    // compare with the last value of res.
    if (res.length === 0 || res[len - 1][1] < intervals[i][0]) {
      res.push([...intervals[i]]);
    } else {
      // Take the maximum value when merge.
      res[len - 1][1] = Math.max(res[len - 1][1], intervals[i][1]);
    }
  }
  return res;
};
```

#### [Diagonal Traverse](https://leetcode-cn.com/problems/diagonal-traverse/)

```js
// Traverse the array with each value in the first row as starting point in the upper right corner, and finally reverse the odd traversal array.
var findDiagonalOrder = function (mat) {
  let [h, w] = [mat.length, mat[0].length],
    // n is the number of traversal
    n = h + w - 1,
    res = [];

  for (let k = 0; k < n; k++) {
    let j, i;
    // When hight is larger than the width, the starting point moves down.
    if (k >= w) {
      j = w - 1;
      i = k - (w - 1);
    } else {
      j = k;
      i = 0;
    }

    let sub = [];
    while (j >= 0 && i < h) {
      sub.push(mat[i][j]);
      i++;
      j--;
    }
    if (!((i + j) % 2)) {
      sub = sub.reverse();
    }
    // console.log(res, sub);
    res = res.concat(sub);
  }

  return res;
};
```

## String

### [Longest Common Prefix (LCP)](https://leetcode-cn.com/problems/longest-common-prefix/)

```js
// The longest common prefix is continuously updated by the first string standard.
var longestCommonPrefix = function (strs) {
  if (strs.length === 0) return "";
  if (strs.length === 1) return strs[0];
  let res = strs[0];
  for (let i = 1; i < strs.length; i++) {
    for (let j = 0; j < res.length; j++) {
      // console.log(j, res[j], strs[i][j]);
      if (res[j] !== strs[i][j]) {
        res = res.substring(0, j);
        // console.log(res);
      }
    }
  }
  return res;
};
```

## Hash Table

### [Valid Sudoku](https://leetcode-cn.com/problems/valid-sudoku/)

```js
// Double traversal, putting different values into different arrays, filtering according to the rules.
var isValidSudoku = function (board) {
  let rowArr = new Array(9).fill(0).map(() => new Set()),
    colArr = new Array(9).fill(0).map(() => new Set()),
    boxArr = new Array(9).fill(0).map(() => new Set());

  for (let i = 0; i < 9; i++) {
    for (let j = 0; j < 9; j++) {
      // if there is not a number, skip it.
      if (board[i][j] === ".") continue;
      // according the i and j to judge the index of sub-box.
      let boxIndex = Math.floor(i / 3) * 3 + Math.floor(j / 3);
      // check row
      if (rowArr[i].has(board[i][j])) return false;
      // check column
      if (colArr[j].has(board[i][j])) return false;
      // check box
      if (boxArr[boxIndex].has(board[i][j])) return false;

      // add the new valid value.
      rowArr[i].add(board[i][j]);
      colArr[j].add(board[i][j]);
      boxArr[boxIndex].add(board[i][j]);
    }
  }
  // console.log(rowArr);
  // console.log(colArr);
  // console.log(boxArr);
  return true;
};
```

## Dynamic Programming

### [Coin Change](https://leetcode-cn.com/problems/coin-change/)

**Transition equations:** $F(i) = \min_{j=0...n-1}F(i - Cj) + 1$

**_F(i)_** is the minimum number of coins required to make up current amount. **_Cj_** is the value of j-th coins.

```js
// From 1 to amount, find the optimal solution for each step.
var coinChange = function (coins, amount) {
  if (amount <= 0) return 0;
  if (coins.length === 0) return -1;

  let len = coins.length,
    dp = [0];

  for (let i = 1; i <= amount; i++) {
    // Initial
    dp.push(Infinity);
    // optimal solution of currenting money
    for (let j = 0; j < len; j++) {
      if (i >= coins[j] && dp[i - coins[j]] !== Infinity) {
        // with the different coin update the dp[i] to minimum constantly.
        dp[i] = Math.min(dp[i - coins[j]] + 1, dp[i]);
      }
    }
    // console.log(dp);
  }
  return dp[amount] === Infinity ? -1 : dp[amount];
};
```

### [ Jump Game](https://leetcode-cn.com/problems/jump-game/)

**Transition equations:** $F(j) = F(i) \ and \ (i+u(i)) \geq j \ \ (0\le i \lt j)$

**_F(j)_** records whether the position can be reached. **_u(i)_** is the displacement at **_i_**.

```js
// Record whether current position can be reached.
var canJump = function (nums) {
  if (!nums || nums.length === 0) return false;
  let dp = [true],
    len = nums.length;

  for (let i = 1; i < len; i++) {
    dp.push(false);
    for (let j = 0; j < i; j++) {
      if (dp[j] && j + nums[j] >= i) {
        dp[i] = true;
        break;
      }
    }
  }

  // console.log(dp);
  return dp[len - 1];
};
```

### [Unique Paths](https://leetcode-cn.com/problems/unique-paths/)

**Transition equations:** $f(i,j) = f(i−1, j) + f(i, j−1)$

**_F(i,j)_** is the number of paths to record the current location.

```js
// Current number of path is equal to the sum of values of upper cell and left cell.
var uniquePaths = function (m, n) {
  let dp = new Array(m).fill(0).map(() => new Array(n).fill(0));

  for (let i = 0; i < m; i++) {
    for (let j = 0; j < n; j++) {
      if (i === 0 || j === 0) dp[i][j] = 1;
      else dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
    }
  }
  // console.log(dp);
  return dp[m - 1][n - 1];
};
```

### [Unique Paths II](https://leetcode-cn.com/problems/unique-paths-ii/)

**Transition equations:** $f(i,j) =
\begin{cases}
0, &u(i, j) = 0	\\
f(i-1, j) + f(i, j-1), &u(i, j) \neq 0
\end{cases}$

**_u(i,j)_** is to record whether there is an obstacle at the current location.

```js
var uniquePathsWithObstacles = function (obstacleGrid) {
  let [h, w] = [obstacleGrid.length, obstacleGrid[0].length];
  let dp = new Array(h).fill(0).map(() => new Array(w).fill(0));

  for (let i = 0; i < h; i++) {
    for (let j = 0; j < w; j++) {
      if (obstacleGrid[i][j] === 1) dp[i][j] = 0;
      else {
        if (i === 0 && j === 0) {
          dp[i][j] = 1;
        } else if (obstacleGrid[i][j] !== 1) {
          if (i >= 1) dp[i][j] += dp[i - 1][j];
          if (j >= 1) dp[i][j] += dp[i][j - 1];
        }
      }
    }
  }

  // console.log(dp);
  return dp[h - 1][w - 1];
};
```

## Greedy

### [ Jump Game](https://leetcode-cn.com/problems/jump-game/)

```js
// Constantly update maximum distance, observe whether the maximum distance can reach the finish line.
var canJump = function (nums) {
  if (nums.length === 1) return true;

  let maxPos = nums[0],
    len = nums.length;

  // Whether you can continue to expand scope within scope of accessibility.
  for (let i = 0; i <= maxPos; i++) {
    maxPos = Math.max(maxPos, i + nums[i]);
    // console.log(i, maxPos);
    if (i < len - 1 && maxPos >= len - 1) return true;
  }

  return false;
};
```

## BFS

![](https://cdn.jsdelivr.net/gh/SmaIIstars/imgCDN/myBlog/ACMer1.png)

```js
const dx = [0, 0, -1, 1],
  dy = [-1, 1, 0, 0];

const polymerize = (arr) => {
  let len = arr.length,
    res = [];

  let h,
    w = -Infinity;

  for (let i = 0; i < len; i++) {
    h = h > arr[i][0] ? h : arr[i][0] + 1;
    w = w > arr[i][1] ? w : arr[i][1] + 1;
  }

  let flag = new Array(h).fill(0).map(() => new Array(w).fill(false)),
    lake = new Array(h).fill(0).map(() => new Array(w).fill(false));

  for (let i = 0; i < len; i++) {
    lake[arr[i][0]][arr[i][1]] = true;
  }

  // console.log(lake);

  for (let i = 0; i < len; i++) {
    if (flag[arr[i][0]][arr[i][1]]) continue;

    flag[arr[i][0]][arr[i][1]] = true;
    let cur = [arr[i]],
      index = 0;
    while (index !== cur.length) {
      let [m, n] = cur[index];
      // console.log(m, n);
      for (let i = 0; i < 8; i++) {
        // console.log(m + dy[i]);
        if (
          m + dy[i] >= 0 &&
          m + dy[i] < h &&
          n + dx[i] >= 0 &&
          n + dx[i] < w &&
          !flag[m + dy[i]][n + dx[i]] &&
          lake[m + dy[i]][n + dx[i]]
        ) {
          cur.push([m + dy[i], n + dx[i]]);
          flag[m + dy[i]][n + dx[i]] = true;
        }
      }
      index++;
      // console.log(cur);
    }

    res.push([...cur]);
  }

  return res;
};
```
