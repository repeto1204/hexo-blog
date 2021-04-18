---
title: Add Two Numbers
categories:
  - algorithm
tags:
  - leetcode
  - algorithm
thumbnail: images/leetcode_cover.jpg
cover: images/leetcode_cover.jpg
date: 2021-04-18 12:53:57
---

---

<!--more-->

*역순으로 숫자가 저장된 링크드 리스트 `l1, l2`가 주어질 때 두 숫자의 합의 역순을 링크드 리스트로 반환하는 문제*

*제약사항*

*1. 링크드 리스트의 노드 수의 범위는 1 ~ 100*

*2. `0 <= Node.val <= 9`*

*3. 숫자는 0으로 시작하지 않는다*

[LeetCode 2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

**Example 1:**

```shell
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
```

**Example 2:**

```shell
Input: l1 = [0], l2 = [0]
Output: [0]
```

**Example 3:**

```shell
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

---

## Solution

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
const addTwoNumbers = (l1, l2) => {
    let carry = 0;
    let head;
    let current;
    while(l1 || l2 || carry) {
        const val1 = l1 ? l1.val : 0;
        const val2 = l2 ? l2.val : 0;
        let sum = val1 + val2 + carry;
        carry = 0;
        if(sum >= 10) {
            carry = Math.floor(sum / 10);
            sum %= 10;
        }
        
        if(!head) {
            head = new ListNode(sum);
            current = head;
        } else {
            current.next = new ListNode(sum);
            current = current.next;
        }
        
        if(l1) l1 = l1.next;
        if(l2) l2 = l2.next;  
    }
    
    return head;
};
```

`head`부터 `tail`까지 수의 덧셈을 진행하는 문제로 두 수의 결과가 10 이상이 되면 올림을 해주어야 한다.

먼저 올림 수를 저장할 변수 `carry`를 선언하고 반환할 `head`와 현재 노드를 저장할 `current`를 선언한다.

```javascript
let carry = 0;
let head;
let current;
```

<br />

`l1, l2, carray` 세 값이 모두 존재하지 않을 때까지 반복하는 `while `문을 선언한다.

```javascript
while(l1 || l2 || carry) {}
```

<br />

반복문 내부에서 값이 있으면 값을 선택하고 없으면 0을 선택하여 더한 후 10보다 큰 경우 올림수와 더한 값을 변경한다.

```javascript
const val1 = l1 ? l1.val : 0;
const val2 = l2 ? l2.val : 0;
let sum = val1 + val2 + carry;
carry = 0;
// 10보다 큰 경우 10의 배수만큼 올림수에 저장하고
// 10으로 나눈 나머지를 sum에 저장한다
if(sum >= 10) {
	carry = Math.floor(sum / 10);
	sum %= 10;
}
```

<br />

만약 `head`에 값이 없다면 링크드 리스트를 만들어 저장 후 `current`에 `head`를 저장한다.

`head`에 값이 있다면 다음 노드에 값을 만들어 저장 후 `current`에 다음 노드를 저장한다.

`l1, l2`에는 반복이 한 번 끝날 때마다 다음 노드를 저장하여 끝까지 순회할 수 있도록 한다.

```javascript
if(!head) {
	head = new ListNode(sum);
	current = head;
} else {
	current.next = new ListNode(sum);
	current = current.next;
}

if(l1) l1 = l1.next;
if(l2) l2 = l2.next;  
```

<br />

마지막으로 `head`를 반환하면 각 수의 합을 역순으로 저장한 링크드 리스트가 반환된다.