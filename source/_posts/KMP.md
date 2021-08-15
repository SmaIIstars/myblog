---
title: KMP
tags:
  - Algorithm
  - JavaScript
mathjax: true
abbrlink: ad32db8f
date: 2021-04-19 18:09:42
---

# KMP

**KMP algorithm is an improved version of pattern matching. It's reduced the number of repeated mathcing.**

## Partial Match Array

### Define

**Prefix string:** All header substrings of a string except the last character.

**Suffix string:** All trailing substrings of a string execept the first character.

**PM:** PM is the maximum length of string at the intersection of prefix and suffix string.

|     Index     |  1  |  2  |  3  |  4  |  5  |
| :-----------: | :-: | :-: | :-: | :-: | :-: |
| **S="ababa"** |  a  |  b  |  a  |  b  |  a  |
|    **PM**     |  0  |  0  |  1  |  2  |  3  |

1. 'a': $pre=suf=\varnothing$, PM = 0.
2. 'ab': $pre=\{a\}, \ suf=\{b\}, \ pre \cap suf = \varnothing$, PM = 0.
3. 'aba':$pre=\{a,ab\}, \ suf=\{a,ba\}, \ pre \cap suf = \{a\}$, PM = 1.
4. 'abab':$pre=\{a,ab,aba\}, \ suf=\{b,ab,bab\}, \ pre \cap suf = \{ab\}$, PM = 2.
5. 'ababa':$pre=\{a,ab,aba,abab\}, \ suf=\{a,ba,aba,baba\}, \ pre \cap suf = \{a,aba\}$, PM = 3.

### Matching Process

**j** is mathcing pointer position of current substring.

**The number of digits moving to the right = The number of characters have been matched - PM value of corresponding section.**

$$
txt = ababcabcacbab,\  pat=abcac
$$

$$
Move = (j-1)\  - \  PM[j-1]
$$

|     Index     |  1  |  2  |  3  |  4  |  5  |
| :-----------: | :-: | :-: | :-: | :-: | :-: |
| **S="abcac"** |  a  |  b  |  c  |  a  |  c  |
|    **PM**     |  0  |  0  |  0  |  1  |  0  |

**step 1:**

|  a  |  b  |  a  |  b  |  c  |  a  |  b  |  c  |  a  |  c  |  b  |  a  |  b  |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|  a  |  b  |  c  |     |     |     |     |     |     |     |     |     |     |

$$
Move = (3 - 1) - PM[2] = 2 - 0 = 2
$$

**step 2:**

|  a  |  b  |  a  |  b  |  c  |  a  |  b  |  c  |  a  |  c  |  b  |  a  |  b  |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|     |     |  a  |  b  |  c  |  a  |  c  |     |     |     |     |     |     |

$$
Move = (5 - 1) - PM[4] = 4 - 1 = 3
$$

**step 3:**

|  a  |  b  |  a  |  b  |  c  |  a  |  b  |  c  |  a  |  c  |  b  |  a  |  b  |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|     |     |     |     |     |  a  |  b  |  c  |  a  |  c  |     |     |     |

## Principle

|  a  |  b  |  a  |  b  |  c  |  a  |  b  |  c  |  a  |  c  |  b  |  a  |  b  |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|  a  |  b  |  c  |     |     |     |     |     |     |     |     |     |     |
|     |     |  a  |  b  |  c  |  a  |  c  |     |     |     |     |     |     |
|     |     |     |     |     |  a  |  b  |  c  |  a  |  c  |     |     |     |

For PM, when matching failed, we query the PM value of previous element. We move the PM values one place to right, this new array is called Next, we use -1 to fill in the first place. Now we can use Next value of current position when matching failed.

|     Index     |  1  |  2  |  3  |  4  |  5  |
| :-----------: | :-: | :-: | :-: | :-: | :-: |
| **S="abcac"** |  a  |  b  |  c  |  a  |  c  |
|   **Next**    | -1  |  0  |  0  |  0  |  1  |

$$
Move = (j - 1)\  - \ next[j]
$$

$$
j = j - Move = j - ((j-1)-next[j]) = next[j] + 1
$$

So we can simplify the formula. Add 1 to all values in Next.

|     Index     |  1  |  2  |  3  |  4  |  5  |
| :-----------: | :-: | :-: | :-: | :-: | :-: |
| **S="abcac"** |  a  |  b  |  c  |  a  |  c  |
|   **Next**    |  0  |  1  |  1  |  1  |  2  |

$$
j = next[j]
$$

## Get Next

**We can get it form the previous PM, but it's too troublesome.**

$$
txt = s_1s_2...s_n
$$

$$
pat = p_1p_2...p_m
$$

**k** means it's being compared to the k-th character in the pattern string. i is the current matching pointer of main string.

$$
p_1p_2...p_{k-1} = p_{j-k+1}p_{j-k+2}...p_{j-1}
$$

Here refer to the calculation of PM value. PM is the maximum value, so they're the same when it doesn't reach the maximum.

**The strings in parentheses are the same.**

| $s_1$ |  [...  | ... |    ...]    | ... | ... | [$s_{i-k+1}$ | ... | $s_{i-1}$] | $s_i$ | ... |   ...   |   ...   | ... | $s_n$ |
| :---: | :----: | :-: | :--------: | :-: | :-: | :----------: | :-: | :--------: | :---: | :-: | :-----: | :-----: | :-: | :---: |
|       | [$p_1$ | ... | $p_{k-1}$] | ... | ... | [$p_{i-k+1}$ | ... | $p_{j-1}$] | $p_j$ | ... | $p_{m}$ |         |     |       |
|       |        |     |            |     |     |    [$p_1$    | ... | $p_{k-1}$] | $p_k$ | ... |   ...   | $p_{m}$ |     |       |

$$
next[j] =
\begin{cases}
0, &(j = 0)
\\
max(next[k])+1, &(1 \lt k \lt j \ and \ p_1...p_k = p_{j-k+1}...p_{j-1})
\\
1, &else
\end{cases}
$$

## Code

```js
const getNext = (s) => {
  let len = s.length,
    j = -1;
  let next = new Array(len).fill(-1);

  for (let i = 0; i < len; next[++i] = ++j) {
    while (j !== -1 && s[i] !== s[j]) {
      j = next[j];
    }
  }

  return next;
};
```

```js
const kmp = (s, t) => {
  let next = getNext(t),
    [slen, tlen] = [s.length, t.length],
    i = (j = 0);

  while (i < slen && j < tlen) {
    // next[0] is -1
    if (j === 0 || s[i] === t[j]) {
      i++;
      j++;
    } else {
      j = next[j];
    }
  }

  if (j >= tlen) return i - tlen;
  return -1;
};
```

## References

[KMP 算法之求 next 数组代码讲解](https://www.bilibili.com/video/BV16X4y137qw/?spm_id_from=333.788.recommend_more_video.1)
