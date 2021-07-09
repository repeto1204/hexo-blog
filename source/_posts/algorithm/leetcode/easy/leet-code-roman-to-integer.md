---
title: Roman to Integer
categories:
  - algorithm
  - leetcode
  - easy
tags:
  - leetcode
  - algorithm
thumbnail: images/leetcode_cover.jpg
cover: images/leetcode_cover.jpg
date: 2021-07-09 17:59:39
---



---

<!--more-->

*7개의 로마 숫자가 주어질 때 해당하는 숫자를 구하는 문제*

| Symbol | Value |
| ------ | ----- |
| I      | 1     |
| V      | 5     |
| X      | 10    |
| L      | 50    |
| C      | 100   |
| D      | 500   |
| M      | 1000  |

*제약사항*

*1. <code>1 <= s.length <= 15</code>*

*2. `s`는 `'I', 'V', 'X', 'L', 'C', 'D', 'M'` 중 하나이다.* 

*3. `s`의 범위는 `[1, 3999]`의 범위를 보장한다.*

*4. 정답의 경우는 오직 하나만 존재*

[LeetCode 13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

**Example 1:**

```shell
Input: s = "III"
Output: 3
```

**Example 2:**

```shell
Input: s = "III"
Output: 3
```

**Example 3:**

```shell
Input: s = "IX"
Output: 9
```

**Example 4:**

```shell
Input: s = "LVIII"
Output: 58
# L = 50, V= 5, III = 3.
```

**Example 5:**

```shell
Input: s = "MCMXCIV"
Output: 1994
# M = 1000, CM = 900, XC = 90 and IV = 4.
```

---

## Solution

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
const romanToInt = (s) => {
    let count = 0;
    const romanNumber = {
        'I': 1,
        'V': 5,
        'X': 10,
        'L': 50,
        'C': 100,
        'D': 500,
        'M': 1000
    }
    
    for(let i = 0; i < s.length; i++) {
        const currentValue = romanNumber[s[i]];
        const nextValue = romanNumber[s[i+1]];
        currentValue < nextValue ? count -= currentValue : count += currentValue;
    }
    
    return count;
};
```

<br />

먼저 로마 숫자에 해당하는 문자를 키로 갖는 객체를 생성하고 숫자를 값으로 설정한다.

문자열을 순회하면서 문자에 해당하는 숫자를 객체에서 꺼내 최종 결괏값에 더한다.

이때 현재 문자의 값이 다음 문자의 값보다 작은 경우 예외처리를 한다.

예를 들어 `XI`는 11이 되지만 `IX`는 10에서 1을 뺀 9가 된다.

그래서 현재 문자의 값이 다음 문자의 값보다 작은 경우 현재 문자의 값을 결괏값에서 빼고 다음 문자의 값만 더한다.

