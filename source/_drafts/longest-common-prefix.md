---
title: Longest Common Prefix
categories:
  - algorithm
  - leetcode
  - easy
tags:
  - leetcode
  - algorithm
thumbnail: images/leetcode_cover.jpg
cover: images/leetcode_cover.jpg
date: 2021-07-09 19:17:39
---



---

<!--more-->

*문자열로 구성된 배열이 주어질 때 공통으로 사용할 수 있는 가장 긴 접두어를 찾는 문제*

*접두어가 없는 경우 빈 문자열을 반환*



*제약사항*

*1. <code>1 <= strs.length <= 200</code>*

*2. <code>0 <= strs[i].length <= 200</code>*

*3. `strs[i]`는 소문자로 구성되어있다.*



[LeetCode 14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

**Example 1:**

```shell
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```shell
Input: strs = ["dog","racecar","car"]
Output: ""
# 공통된 문자열이 존재하지 않는다.
```

---

## Solution

```javascript
/**
 * @param {string[]} strs
 * @return {string}
 */
const longestCommonPrefix = (strs) => {
    let prefix = strs[0];
    
    for(let i = 0; i < strs.length; i++) {
        prefix = getCommonPrefix(prefix, strs[i]);
        if(prefix === '') break;
    }
    
    return prefix || '';
}
    
const getCommonPrefix = (prefix, str) => {
    let i = 0;
    while(i < prefix.length && i < str.length) {
        if(prefix[i] !== str[i]) break;
        i++;
    }
    
    return prefix.slice(0, i);
    
}
```

<br />

배열의 첫 번째 문자를 접두어로 두고 그 후 문자부터 공통된 부분만 찾아낸다.

공통부분을 찾아내는 함수 `getCommonPrefix`에서 공통된 부분의 인덱스를 찾고 해당 인덱스까지 기존의 접두어를 `slice`하여 반환한다.

만약 반복문 도중 `prefix`가 빈 문자열이 된다면 반복문을 빠져나온다.
