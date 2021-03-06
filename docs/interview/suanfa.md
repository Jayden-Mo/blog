# 算法面试题

- 排序
- 分治
- 数学运算
- 查找
- 递归和循环
- 回溯算法
- 贪心算法
- 动态规划

## 递归和循环

属于暴利解法，使用递归或循环，将复杂的逻辑拆解，核心在于优化代码的量级，减少循环次数。

- 两数相除
- 在排序数组中查找元素的第一个和最后一个位置

### 两数相除

[leetcode 29](https://leetcode-cn.com/problems/divide-two-integers/)

```js
// string 模拟小学除法
// 除此之外，可以使用 2** 0 2** 1 2** 2 逐渐靠近除数的方式循环求解
// 使用 << 左移 >> 右移 可以模拟乘法
var divide = function(dividend, divisor) {
    var res=0
    var sign=dividend>0?divisor>0?'':'-':divisor>0?'-':''
    dividend=Math.abs(dividend)
    divisor=Math.abs(divisor)
    var strdiv=String(dividend)
    var quot='',remainder=''
    for(var i=0;i<strdiv.length;i++){
        remainder+=strdiv[i]
        var temp=0
        var m=parseInt(remainder)
        while(divisor<=m){
            m=m-divisor
            temp++
        }
        quot+=temp
        remainder=String(m)
    }
    var res=parseInt(sign+quot)
    if(res>Math.pow(2,31)-1||res<Math.pow(-2,31))return Math.pow(2,31)-1
    return res
};
```

### 在排序数组中查找元素的第一个和最后一个位置

[leetcode 34](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```js
var searchRange = function (nums, target) {
  const result = [-1, -1];
  let leftIndex = binarySearch(nums, target, true);
  if (leftIndex == nums.length || nums[leftIndex] != target) {
    return result;
  }
  result[0] = leftIndex;
  result[1] = binarySearch(nums, target, false) - 1;

  return result;
};

// flag=true  表示左边坐标
// flag=false 表示右边坐标
function binarySearch(nums, target, flag) {
  let start = 0;
  let end = nums.length;
  while (start < end) {
    const mid = Math.floor((start + end) / 2);
    if (nums[mid] > target || (nums[mid] === target && flag)) {
      end = mid;
    }
    if (nums[mid] < target || (nums[mid] === target && !flag)) {
      start = mid + 1;
    }
  }
  return start;
}
// binarySearch([5, 7, 7, 8, 8, 10], 8, false);
```

## 回溯算法

回溯算法实际上就是一个决策树的遍历过程，好像是在走路，先从列表中选一步，然后在剩下的丼中选择第二步，不断重复，直到走到路的尽头，然后再倒回来，把没有走过的路再走一遍。

- 78 子集
- 46 全排列
- 89 格雷编码
- 17 电话号码的字母组合
- 22 括号生成

### 78 子集

[leetcode 78](https://leetcode-cn.com/problems/subsets/)

```js
var subsets = function(nums) {
  var result = [];
  var temp = [];
  loop(nums, result, temp, 0);
  return result;
};

function loop(nums, result, temp, index) {
  result.push([...temp]);
  for (var i = index; i < nums.length; i++) {
    temp.push([nums[i]]);
    loop(nums, result, temp, i + 1);
    temp.pop();
  }
}
```

### 46 全排列

[leetcode 46](https://leetcode-cn.com/problems/permutations/)

```js
var permute = function(nums) {
  var result = [];
  loop(nums, result, []);
  return result;
};

function loop(nums, result, temp) {
  if (temp.length === nums.length) {
    result.push([...temp]);
  }
  for (var i = 0; i < nums.length; i++) {
    if (temp.includes(nums[i])) continue;
    temp.push(nums[i]);
    loop(nums, result, temp);
    temp.pop();
  }
}
```

### 格雷编码

[leetcode 89](https://leetcode-cn.com/problems/gray-code/)

```js
/**
  关键是搞清楚格雷编码的生成过程, G(i) = i ^ (i/2);
  如 n = 3:
  G(0) = 000,
  G(1) = 1 ^ 0 = 001 ^ 000 = 001
  G(2) = 2 ^ 1 = 010 ^ 001 = 011
  G(3) = 3 ^ 1 = 011 ^ 001 = 010
  G(4) = 4 ^ 2 = 100 ^ 010 = 110
  G(5) = 5 ^ 2 = 101 ^ 010 = 111
  G(6) = 6 ^ 3 = 110 ^ 011 = 101
  G(7) = 7 ^ 3 = 111 ^ 011 = 100
**/
function grayCode(n) {
  var array = [];
  for (var i = 0; i < 2 ** n; i++) {
    array.push(i ^ (i / 2));
  }
  return array;
}
```

### 电话号码的字母组合

[leetcode 17](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

```js
var letterCombinations = function (digits) {
  const map = {
    2: "abc",
    3: "def",
    4: "ghi",
    5: "jkl",
    6: "mno",
    7: "pqrs",
    8: "tuv",
    9: "wxyz",
  };
  const tempArrays = [];
  const phoneNumbers = digits.split("");
  phoneNumbers.forEach((phoneNumber) => {
    const charts = map[phoneNumber].split("");
    tempArrays.push(charts);
  });
  let result = [];
  loop(tempArrays, result, [], 0);
  return result;
};

function loop(array, result, temp, index) {
  if (temp.length === array.length && array.length>0) {
    result.push(temp.join(""));
  }
  if (index > array.length - 1) {
    return;
  }
  for (var i = 0; i < array[index].length; i++) {
    temp.push(array[index][i]);
    loop(array, result, temp, index + 1);
    temp.pop();
  }
}
```

### 括号生成

[leetcode 22](https://leetcode-cn.com/problems/generate-parentheses/)

```js
var generateParenthesis = function (n) {
  let result = [];
  loop(result, [], n);
  return result;
};

function loop(result, temp, n) {
  const leftTempLength = temp.filter((item) => item === "(").length;
  const rightTempLength = temp.filter((item) => item === ")").length;
  if (temp.length === n * 2 && leftTempLength === rightTempLength) {
    const item = temp.join("");
    result.push(item);
    return;
  }

  if (leftTempLength < n) {
    temp.push("(");
    loop(result, temp, n);
    temp.pop();
  }

  if (leftTempLength > rightTempLength) {
    temp.push(")");
    loop(result, temp, n);
    temp.pop();
  }
}
```

## 贪心算法

### 1、老师分饼干

老师分饼干，每个孩子只能得到一块饼干，但每个孩子想要的饼干大小不尽相同。目标是尽量让更多的孩子满意。 如孩子的要求是 [1, 3, 5, 4, 2]，饼干大小是[1, 1]，最多能让 1 个孩子满足。 如孩子的要求是 [10, 9, 8, 7, 6]，饼干大小是[7, 6, 5]，最多能让 2 个孩子满足。

符合**贪心算法**思想，在满足孩子的情况下，使孩子的饼干尽可能小。

```js
function splitCake(childrenIssue, cake) {
  var sortChildrenIssue = childrenIssue.sort((item1, item2) => item1 - item2);
  var sortCake = cake.sort((item1, item2) => item1 - item2);
  var result = [];
  for (
    var i = 0, j = 0;
    i < sortChildrenIssue.length && j < sortCake.length;
    j++
  ) {
    if (sortChildrenIssue[i] <= sortCake[j]) {
      result.push(sortChildrenIssue[i]);
      i++;
    }
  }
  return result;
}
```

排序的时间复杂度为 `O(n*log(n))`，两根指针的时间的复杂度为 `O(n)`，总的时间复杂度为`O(n*log(n))`
