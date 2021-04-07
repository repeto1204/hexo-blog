---
title: Remove Nth Node From End of List
categories: 
tags:
  - leetcode
  - algorithm
thumbnail: images/leetcode_cover.jpg
cover: images/leetcode_cover.jpg
date: 2021-03-29 23:49:31
---

---

<!--more-->
*연결 리스트의 `head`가 주어질때 뒤에서 `n`번째 노드를 제거한 후 `head`를 리턴*

*제약사항*
*1. 리스트의 노드의 숫자는 `size`로 한다.*
*2. `1 <= size <= 30`*
*3. `0 <= Node.val <= 100`*
*4. `1 <= n <= size`*

[LeetCode 19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

**Example 1:**

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```
**Example 2:**

```
Input: head = [1], n = 1
Output: []
```

**Example 3:**

```
Input: head = [1,2], n = 1
Output: [1]
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
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
const removeNthFromEnd = (head, n) => {
    let prev = head, next = head;
    // block 1)
    while(n--) {
        next = next.next;
    }
    
    // block 2)
    while(next && next.next) {
        prev = prev.next;
        next = next.next;
    }
    
    // block 3)
    if(!next) {
        head = head.next;
    // block 4)
    } else {
        prev.next = prev.next ? prev.next.next : null;
    }
    
    return head;
};
```
<br />

**Block 1)**

```javascript
let prev = head, next = head;
while(n--) {
	next = next.next;
}
```
먼저 제거할 타겟의 이전 노드와 다음 노드를 알아내기 위해 `prev`, `next`변수를 선언하고 `next`를 `n`번 만큼 움직인다.

<br />

**Block 2)**

```javascript
while(next && next.next) {
	prev = prev.next;
	next = next.next;
}
```
그 뒤 `next`가 끝에 도달할 때 까지 `prev`와 같이 움직여주면 `prev`는 타겟의 이전 노드가 된다.

`1 - 2 - 3 - 4(target) - 5` 인 리스트가 있을 때 먼저 `n`만큼 next를 움직이면 다음과 같다.

`1(prev) - 2 - 3(next) - 4(target) - 5`

그리고 `next`가 끝에 도달할 때 까지 `prev`를 같이 움직이면 최종적으로 다음과 같다.

`1 - 2 - 3(prev) - 4(target) - 5(next)`

<br />

**Block 3)**

```javascript
if(!next) {
	head = head.next;
}
```
두 번째 `while`문은 `next`의 다음 노드가 없을 때는 실행하지 않기 때문에 위의 블록에 진입했다면

첫 번째 `while`문에서 이미 `next`가 `null`이 된것이다. 이 경우는 `head`가 지워지는 노드이다.

따라서, `head`에 `head.next`를 주입한다. `head.next`에 아무것도 없다면 자연스럽게 `null`을 리턴할것이다.

<br />

**Block 4)**

```javascript
else {
	prev.next = prev.next ? prev.next.next : null;
}
```
`prev`는 타겟의 왼쪽 노드로 만약 타겟이 있다면 `prev.next`에 타겟의 `.next`를 주입한다.

만약 아무것도 없다면 `null`을 주입한다.

<br />

```javascript
return head;
```
마지막으로 `head`를 리턴하면 문제가 해결된다.