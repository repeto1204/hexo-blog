---
title: Valid Parentheses
categories:
  - algorithm
  - leetcode
  - easy
tags:
  - leetcode
  - algorithm
thumbnail: images/leetcode_cover.jpg
cover: images/leetcode_cover.jpg
date: 2021-05-01 20:37:22
---


---

<!--more-->

*`'(', ')', '{'. '}'. '['. ']'`로 이루어진 문자열 s가 주어질 때  문자열 s가 유효한지 판단하는 문제*

*다음의 경우를 유효하다고 판단한다.*

*1. 열린 괄호는 같은 유형의 괄호로 닫아야 한다.*

*2. 열린 괄호는 올바른 순서로 닫아야 한다.*

*제약사항*

*1. <code>1 <= s.length <= 10<sup>4</sup></code>*

*2. s는 `(){}[]` 로만 구성된다.*

[LeetCode 20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

**Example 1:**

```shell
Input: s = "()"
Output: true
```

**Example 2:**

```shell
Input: s = "()[]{}"
Output: true
```

**Example 3:**

```shell
Input: s = "(]"
Output: false
```

**Example 4:**

```shell
Input: s = "([)]"
Output: false
```

**Example 5:**

```shell
Input: s = "{[]}"
Output: true
```

---

## Solution

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */

const targetChar = {
    '(': ')',
    '{': '}',
    '[': ']'
}
const isValid = (s) => {
    const temp = [];
    for(let i = 0; i < s.length; i++) {
        const char = s[i];
        const tempChar = targetChar[char];
        if(tempChar) {
            temp.push(tempChar);
        } else if(temp.pop() !== char) {
            return false;
        }
    }
    return temp.length === 0;
};
```
<br />

같은 유형의 괄호로 닫아야 유효한 문자열이기 때문에 각 괄호 문자를 key, value 형태로 targetChar에 저장한다.

isValid에서 s를 차례로 순회하며 여는 괄호일 때 같은 유형의 닫는 괄호를 temp 배열에 저장한다.

유효한 문자열이라면 닫는 괄호 직전에 나온 문자는 동일한 유형의 괄호여야 한다.

닫는 괄호일 때 temp의 마지막 요소를 pop 하여 비교한다.

여는 괄호일 때 같은 유형의 닫는 괄호를 temp에 넣었기 때문에 동일 문자열 비교를 해서 같지 않으면 false를 리턴하면된다.

반복문이 끝나고 temp에 문자열이 남아있으면 false, 그렇지 않으면 true를 반환하면 문제가 해결된다.