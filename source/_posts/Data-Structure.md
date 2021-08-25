---
title: Algorithm and Data Structure
abbrlink: 17f44e1a
date: 2021-04-05 23:00:51
tags: Algorithm
---

# Algorithm and Data Structure

**Algorithm and Data Structure.**

## Data Structure

### String

**The position of substrings (pattern string) is called pattern matching of strings.**

#### [Implement strStr()](https://leetcode-cn.com/problems/implement-strstr/)
- Violent match

  ```js
  var strStr = function (haystack, needle) {
    let [len1, len2] = [haystack.length, needle.length];
  
    let i = (j = 0);
  
    while (i < len1 && j < len2) {
      // console.log(i, j);
      if (haystack[i] === needle[j]) {
        i++;
        j++;
      } else {
        i = i - j + 1;
        j = 0;
      }
    }
  
    if (j >= len2) return i - j;
    else return -1;
  };
  ```

- KMP

  ```js
  
  ```

  



### Hash Table

**Hash Table is a structure of key-value.**



### Tree

#### Array to Tree

```js
// the stuct of TreeNode
class TreeNode {
  id = 0;
  pid = 0;
  name = "";
  children = [];

  constructor(node) {
    const { id, pid, name } = node;

    this.id = id;
    this.pid = pid;
    this.name = name;
  }
}

const createTreeNode = (node) => {
  let res = new TreeNode(node);
  return res;
};

// the compare of sort
const compare = (properties) => {
  return (a, b) => {
    if (a[properties[0]] !== b[properties[0]]) {
      return a[properties[0]] - b[properties[0]];
    } else {
      return a[properties[1]] - b[properties[1]];
    }
  };
};

const arrayToTree = (arr) => {
  // judge the boundary conditions
  if (!arr || !arr.length) return;
  // sort the data according to rules, pid -> id
  arr.sort(compare(["pid", "id"]));

  /**
   * root
   * curNodes: record which nodes have been traversed
   * curParNode: record who the current parent node is
   */
  let root = createTreeNode(arr.shift()),
    curNodes = [root],
    curParNode = curNodes.shift();

  for (let node of arr) {
    let curNode = createTreeNode(node);
    // Mark the curNode(current node) has been traversed, put it into the curNodes
    curNodes.push(curNode);

    //  Determine whether the parent node of curNode(current node) is the curParNode(current parent node)
    while (curParNode.id !== curNode.pid) {
      // if not, changed the curParNode
      curParNode = curNodes.shift();
    }
    // put the children node into the curParNode's children array
    curParNode.children.push(curNode);
  }
  return root;
};

console.log(arrayToTree(data));
```





## Algorithm

### Sort

```js
const swap = (arr, a, b) => {
  let temp = arr[a]
  arr[a] = arr[b]
  arr[b] = temp
  return arr
}
```

#### BubbleSort

**If a larger value is encountered, pass it backward.**

```js
const bubbleSort = (arr) => {
  let len = arr.length
  while(len--){
    for(let i = 0; i < len; i++){
      // Tow adjacent elements compare and swap.
      if (arr[i] > arr[i + 1]) swap(arr, i, i + 1)
      // Current element and the last element compare and swap.
      // if(arr[i] > arr[len]) swap(arr, i, len)
    }
  }
  return arr
}
```

#### SelectSort

**Select the maximum value in current scope and place it last.**

```js
const selectSort = (arr) => {
  let len = arr.length
  
  while(len--){
    let maxIndex = 0,
	      maxVal = arr[0]
    
    // because len--, here sholud is len+1
    for(let i = 0; i < len + 1; i++){
      if(arr[i] > maxVal){
	      maxVal = arr[i]
        maxIndex = i
      }
      swap(arr, maxIndex, len)
    }
  }
  
  return arr
}
```

#### InsertSort

**From the beginning of traversal, asuume that the n-1 has been sorted, and compare and insert from back to front.**

```js
const insertSort = (arr) => {
  let len = arr.length
  
  // Assume the first value is ordered.
  for(let i = 1; i < len; i++){
    let j = i,
        temp = arr[j]
    // Find the current correct position.
    while(j > 0 && arr[j] < temp){
      arr[j] = arr[j-1]
      j--
    }
    arr[j] = temp
  }
  
  return arr
}
```

#### ShellSort

**The advanced version of  insert sort.**

```js
const shellSort = (arr) => {
  let len = arr.length
  let gap = Math.floor(len / 2)
  
  while(gap){
    
    // shellSort is start from the second part, so our i start from gap to the end of array.
    // This gap is rounded down, so there may be extra parts in lower half.
    for(let i = gap; i < len; i++){
      let j = i,
          temp = arr[i]
      
      while(j > 0 && arr[j - gap] > temp){
        arr[j] = arr[j - gap]
        j -= gap
      }
      
      arr[j] = temp
    }
    
    gap = Math.floor(gap / 2)
  }
}
```

#### QuickSort

```js
const quickSort = (arr) => {
  let len = arr.length
  quick(arr, 0, len-1)
  return arr
}
```

```js
const quick = (arr, left, right) => {
  if(left >= right) return arr
  
  // find the pivot
  let pivot = median(arr, left, right)
  
  // double pointers traversal exchange value
  let i = left
  let j = right-1
  while(true){
    while(i < j && arr[i] <= pivot) i++
    while(i < j && arr[j] >= pivot) j--
    if(i >= j) break
    swap(arr, i, j)
  }
  swap(arr, j, right-1)
  
  quick(arr, left, j-1)
  quick(arr, j+1, right)
}
```

```js
const median(arr, left, right){
  let mid = Math.floor((left + right) / 2)
  
  // first, sort the three values
  if(arr[left] > arr[right]) swap(arr, left, right)
  if(arr[left] > arr[mid]) swap(arr, left, mid)
  if(arr[mid] > arr[right]) swap(arr, mid, right)
	
  // second, place the middle value in the correct position
  swap(arr, mid, right-1)
  return arr[right-1]
}
```

#### MergeSort

```js
const mergeSort = (arr) => {
  let len = arr.length
  if(len <= 1) return arr
  
  let mid = Math.floor(len / 2)
  let leftArr = arr.slice(0, mid),
      rightArr = arr.slice(mid)
  
  return merge(mergeSort(leftArr), mergeSort(rightArr))
}
```

```js
const merge = (leftArr, rightArr) => {
  let [leftArrLen, rightArrLen] = [leftArr.length, rightArr.length]
  let leftIndex = 0, rightIndex = 0
  
  let mergeArr = []
  while(leftIndex < leftArrLen && rightIndex < rightArrLen){
    mergeArr.push(leftArr[leftIndex] < rightArr[rightIndex] ? leftArr[leftIndex++] : rightArr[rightIndex++])
  }
  
  while(leftIndex < leftArrLen) mergeArr.push(leftArr[leftIndex++])
  while(rightIndex < rightArrLen) mergeArr.push(rightArr[rightIndex++])
  
  return mergeArr
}
```

#### CountingSort

```js
const countingSort = (arr) => {
  let bucket = {};

  arr.forEach((val) => {
    if (!bucket[val]) bucket[val] = 0;
    bucket[val]++;
  });

  let index = 0;
  for (let key in bucket) {
    for (let i = 0; i < bucket[key]; i++) {
      arr[index] = parseInt(key);
      index++;
    }
  }

  return arr;
};
```

#### HeapSort

##### Maximum Heap

```js
const heapSort = (arr) => {
  let len = arr.length
  buildHeap(arr)
  
  for(let i = len - 1; i >= 0; i--){
    swap(arr, 0, i)
    heapify(arr, i, 0)
  }
  
  return arr
}
```

```js
const buildHeap = (arr) => {
  let len = arr.length,
      last_node = len-1,
      buildIndex = Math.floor((last_node - 1) / 2) 
  
  for(let i = buildIndex; i >= 0; i--){
    heapify(arr, len, i)
  }
}
```

```js
const heapify = (arr, len, parent) => {
  let left = parent * 2 + 1,
      right = parent * 2 + 2,
      maxPos = parent
  
  while(left < len && arr[left] > arr[maxPos]) maxPos = left
  while(right < len && arr[right] > arr[maxPos]) maxPos = right

  if(maxPos !== parent){
    swap(arr, parent, maxPos)
    heapify(arr, len, maxPos)
  }
}
```

##### Minimum Heap

```js
const heapify = (arr, len, parent) => {
  let left = parent * 2 + 1,
      right = parent * 2 + 2,
      minPos = parent
  
  while(left < len && arr[left] < arr[minPos]) maxPos = left
  while(right < len && arr[right] < arr[minPos]) maxPos = right

  if(maxPos !== parent){
    swap(arr, parent, minPos)
    heapify(arr, len, minPos)
  }
}
```

#### bucketSort

```js
const bucketSort = (arr, bucketSize) => {
  let len = arr.length
  let minValue = Math.min(...arr),
      maxValue = Math.max(...arr)
  
  // Initialize the bucket
  bucketSize = bucketSize || 5
  let bucketCount = Math.floor((maxValue - minValue) / bucketSize) + 1,
      buckets = new Array(bucketCount)
  
  // Note here must be a new array and cannot refer to the same array address.
  for(let i = 0; i < bucketCount; i++) bucket[i] = []
  
  // putting the number to corresponding scope.
  arr.forEach((i) => buckets[Math.floor((i - minValue) / bucketSize)].push(i))
  
  // reset the arr
  arr.length = 0
  for(let i = 0; i < buckets.length; i++){
    insertSort(buckets[i])
    for(let j = 0; j < buckets[i]; j++){
      arr.push(buckets[i][j])
    }
  }
  
  return arr
}
```



#### Reference

[十大排序算法详解](https://blog.csdn.net/weixin_40596016/article/details/79711682)

[十大经典排序算法](https://www.runoob.com/w3cnote/ten-sorting-algorithm.html)

[HeapSort Video](https://www.bilibili.com/video/BV1Eb41147dK?from=search&seid=16277008371724387204)

