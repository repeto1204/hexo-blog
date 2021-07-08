---
title: Palindrome Number
categories:
  - algorithm
  - leetcode
  - easy
tags:
  - leetcode
  - algorithm
thumbnail: images/leetcode_cover.jpg
cover: images/leetcode_cover.jpg
date: 2021-07-08 17:41:22
---



---

<!--more-->

*정수형 변수 `x`가 주어질 때 `x`가 회문인지 판별하는 문제*

*제약사항*

*1. <code>-2<sup>31</sup> <= x <= 2<sup>31</sup> - 1</code>*

*2. s는 `(){}[]` 로만 구성된다.*

[LeetCode 9. Valid Parentheses](https://leetcode.com/problems/palindrome-number/)

**Example 1:**

```shell
Input: x = 121
Output: true
```

**Example 2:**

```shell
Input: x = -121
Output: false
# 왼쪽에서부터 읽으면 -121, 오른쪽에서부터 읽으면 121- 이므로 회문이 아니다.
```

**Example 3:**

```shell
Input: x = 10
Output: false
# 왼쪽에서 읽으면 10, 오른쪽에서부터 읽으면 01 이므로 회문이 아니다.
```

**Example 4:**

```shell
Input: x = -101
Output: false
```

---

## Solution

```javascript
/**
 * @param {number} x
 * @return {boolean}
 */
const isPalindrome = (x) => {
    if(x < 0) return false;
    let num = x, reverse = 0;
    while(num !== 0) {
        reverse = reverse * 10 + (num % 10);
        num = Math.floor(num / 10);
    }
    
    return x === reverse;
};
```
<br />

회문은 어느 방향으로 읽어도 같은 문자가 나와야 하기 때문에 음수는 `false`를 반환한다.

그 후 `reverse` 변수에 `num`의 끝자리부터 차례대로 더하며 자릿수를 올리고 `reverse`와 `x`를 비교하여 결과를 반환한다.

`x`를 `toString()` 메소드를 사용하여 문자열화 한 후 `.split('').reverse().join('')`를 사용하여 비교할 수도 있다.