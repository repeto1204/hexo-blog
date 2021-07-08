---
title: Reverse Integer
categories:
  - algorithm
  - leetcode
  - easy
tags:
  - leetcode
  - algorithm
thumbnail: images/leetcode_cover.jpg
cover: images/leetcode_cover.jpg
date: 2021-05-17 19:51:38
---

---

<!--more-->

*부호 있는 32bit 정수 x가 주어질 때 숫자를 뒤집어서 반환*

*반환되는 x가 범위를 벗어나면 0 반환*

*제약사항*

*1. <code>-2<sup>31</sup> <= x <= 2<sup>31</sup>-1</code>*

[LeetCode 7. Reverse Integer](https://leetcode.com/problems/reverse-integer/)

**Example 1:**

```shell
Input: x = -123
Output: 321
```

**Example 2:**

```shell
Input: x = 120
Output: 21
```

**Example 3:**

```shell
Input: x = 0
Output: 0
```

---

## Solution

```javascript
/**
 * @param {number} x
 * @return {number}
 */
const reverse = function(x) {
    const isMinus = x < 0;    
    const result = parseInt(x.toString().split('').reverse().join(''));
    
    if(outOfRange(result)) return 0;
    
    return isMinus ? -result : result;
    
    function outOfRange(num) {
        const MAX_NUM = 2**31 - 1;
        const MIN_NUM = -(2**31);
        return num < MIN_NUM || num > MAX_NUM;
    }
};
```

<br />

x가 음수인지 양수인지를 판단하여 `isMinus`에 저장한다.

x를 문자열 변환 후 split, reverse, join을 사용하여 역순 정렬 된 문자열을 생성한다.

해당 문자열을 다시 숫자형으로 형 변환 하여 뒤집힌 숫자를 저장한다.

뒤집힌 숫자가 범위를 벗어나면 0을 반환하고 그렇지 않으면 `isMinus` 값에 따라 음수 변환 후 반환한다.