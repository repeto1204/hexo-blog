---
title: Two Sum
categories: 
tags:
  - leetcode
  - algorithm
thumbnail: images/leetcode_cover.jpg
cover: images/leetcode_cover.jpg
date: 2021-04-05 22:33:39
---


---

<!--more-->

*정수 배열 `nums`와 정수 `target`이 주어질 때 더해서 `target`이 되는 두 수를 구하는 문제*

*제약사항*

*1. <code>2 <= nums.length <= 10<sup>3</sup></code>*

*2. <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>*

*3. <code>-10<sup>9</sup> <= target <= 10<sup>9</sup></code>*

*4. 정답의 경우는 오직 하나만 존재*

[LeetCode 1. Two Sum](https://leetcode.com/problems/two-sum/)

**Example 1:**

```shell
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**

```shell
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**

```shell
Input: nums = [3,3], target = 6
Output: [0,1]
```

---

## Solution

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
const twoSum = (nums, target) => {
    const numsMap = {};
    for(let i=0; i<nums.length; i++){
        if(numsMap[nums[i]]>=0){
            return [numsMap[nums[i]],i]
        }
        numsMap[target-nums[i]] = i
    }
};
```

<br />

정답이 항상 존재하고 하나만 존재한다는 가정이 있기 때문에 객체에 `target-nums[i]`를 key로 등록해두면

합이 되는 숫자일 때 key 탐색 시 `undefined` 아닌 값이 반환된다.

또 반환 값은 두 수의 인덱스 배열이기 때문에  value에는 인덱스를 저장해두면 반환 시 바로 쓸 수 있다.

<br />

**Example 1**의 경우 처음 `nums[i]`의 값은 2가 되고 이때 `numsMap[nums[i]]`는 `undefined`가 된다.

이때 `numsMap`에는 `target-nums[i] : i`가 저장된다.

```javascript
const numsMap = {
	7 : 0
}
```

그리고 다음 `nums[i]`의 값은 7이고 `numsMap[nums[i]]`는 `undefined`이 아니기 때문에 합이 `target`이 되는 케이스이다.

이때 `numsMap[nums[i]]`의 값과 현재 숫자의 인덱스를 반환하면 두 수의 합이 `target`이 되는 인덱스 배열을 반환하게 된다.