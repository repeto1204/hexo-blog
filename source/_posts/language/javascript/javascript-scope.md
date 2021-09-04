---
title: 자바스크립트 스코프
categories:
  - language
  - javascript
tags:
  - javascript
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg
date: 2021-09-03 15:53:26
---

---

<!--more-->

## **스코프**

스코프는 유효범위라고도 하며 변수의 범위 또는 접근성, 함수의 범위 또는 함수 내부에서 접근할 수 있는 변수 등이 스코프의 개념 중 일부이다.

함수의 매개변수는 함수의 몸체 내부에서만 접근이 가능한 것도 마찬가지로 스코프의 개념 중 일부이다.

자바스크립트에서 모든 식별자는 자신이 선언된 위치에 의해 유효범위가 결정된다. 이를 스코프라고 한다.

---

## **스코프의 종류**

스코프는 전역(global)과 지역(local)으로 구분할 수 있다.

전역 스코프는 코드의 가장 바깥 영역이 유효 범위인 것을 뜻하고 지역 스코프를 해당 지역이 유효 범위인 것을 뜻한다.

```javascript
// global scope
const x = 'global x';
const y = 'global y';
// global scope

function outerFunc() {
    // outerFunc local scope
    const z = 'outerFunc local z';
    
    console.log(x); // global x
    console.log(y); // global y
    console.log(z); // outFunc local z
    // outerFunc local scope
    
    function innerFunc() {
        // innerFunc local scope
        const x = 'innerFunc local x';
        
        console.log(x); // innerFunc local x
        console.log(y); // global y
        console.log(z); // outFunc local z
        // innerFunc local scope
    }
    
    innerFunc();
}

outerFunc();

console.log(x); // global x
console.log(z); // ReferenceError: z is not defined
```

**전역 스코프**

위의 예제에서 가장 바깥의 영역이 전역 영역이다.

해당 영역에 변수를 선언하면 변수는 전역 스코프를 갖는 전역 변수가 된다.

전역 변수는 어디서든 참조할 수 있다.

x 와 y는 전역 영역에 선언되어 어디서든 참조가 가능하고 함수 내부에서도 참조 가능하다.

<br />

**지역 스코프**

지역 스코프는 지역 영역에 변수를 선언했을 때  변수가 갖게 되는 스코프이다.

지역은 함수 몸체 내부 또는 블록 내부를 의미하는데 위 예제에서는 함수 몸체 내부이다.

지역 변수는 선언된 지역이나 하위 지역(지역 내부의 지역)에서만 참조할 수 있다.

outerFunc 내부에 선언된 변수 z는 지역 변수이고 해당 지역 내부에 innerFunc에서 참조가 가능하다.

innerFunc는 outerFunc의 하위 지역이기 때문이다.

---

## **스코프 체인**

![](images/javascript-scope/scope.jpg)

지역 스코프에서는 상위 지역 스코프를 포함하고 나아가 전역 스코프도 포함한다.

이처럼 스코프가 계층적으로 연결된 것을 스코프 체인이라고 하며 모든 스코프의 최상위 스코프는 전역 스코프이다.

```javascript
const x = 'global x';
const g = 'global g';

function outer() {
    const y = 'outer y';
    const g = 'outer g';
    
    function inner() {
        const z = 'inner z';
        const g = 'inner g';
        
        console.log(x); // global x
        console.log(y); // outer y
        console.log(z); // inner z
        console.log(g); // inner g
    }
    inner();
}

outer();
```

자바스크립트 엔진은 변수 참조 시 스코프 체인을 통해 변수를 검색한다.

inner function에서 변수 x를 참조할 때 inner function의 지역 스코프에 x가 없기 때문에 그 상위 스코프인 outer function 지역 스코프를 검색하고 outer function 지역 스코프에도 x가 없기 때문에 전역 스코프의 x를 검색하여 출력한다.

변수 g의 경우 모든 스코프에 선언되어 있지만, inner function의 지역 스코프에 g가 있기 때문에 검색을 중단하고 해당 변수를 참조하여 반환한다.

이처럼 변수 참조 시 현재 스코프부터 상위 스코프를 순차적으로 탐색하여 변수를 검색하고 이때 이용하는 것이 스코프 체인이다.

스코프 체인은 하위에서 상위로 진행하며 하위로 진행하는 경우는 존재하지 않는다.

---

## **렉시컬 스코프**

일반적으로 함수의 상위 스코프를 결정하는 패턴은 두 가지로 볼 수 있다.

1. 함수를 호출한 위치에 따라 상위 스코프 결정 (동적 스코프, dynamic scope)
2. 함수를 정의한 위치에 따라 상위 스코프를 결정 (렉시컬 스코프, lexical scope)

```javascript
const x = 'global x';

function foo() {
    const x = 'foo x';
    bar(); // global x
}

function bar() {
    console.log(x);
}

foo();
```

만약 자바스크립트의 상위 스코프 결정 패턴이 1번이라면 함수 bar에서 x의 참조 값은 foo x일 것이다.

하지만 자바스크립트는 2번 패턴 즉, 렉시컬 스코프 방식이므로 함수 bar에서 x의 참조 값은 global x이다.

이처럼 함수는 함수가 정의될 때의 상위 스코프를 기억하여 함수가 실행될 때마다 기억한 상위 스코프를 참조한다.
