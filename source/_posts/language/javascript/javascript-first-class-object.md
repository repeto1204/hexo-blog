---
title: 자바스크립트 일급 객체
categories:
  - language
  - javascript
tags:
  - javascript
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg
date: 2021-11-05 11:12:25
---


---

<!--more-->

## **일급 객체**

컴퓨터 프로그래밍 언어 디자인에는 일급 객체(first class object)라는 개념이 있는데 [위키의 정의](https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B8%89_%EA%B0%9D%EC%B2%B4)를 인용하면 다음과 같다.

> 일급 객체란 다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체를 가리킨다.

이를 조건으로 나열하면 다음과 같다.

1. 변수나 데이터 구조 안에 담을 수 있다.
2. 파라미터로 전달할 수 있다.
3. 반환 값으로 사용할 수 있다.

자바스크립트에서는 일급 객체의 조건에 해당하는 것은 함수이다.

즉, 자바스크립트의 함수는 일급 객체이다.

---

## **함수 객체의 프로퍼티**

함수는 객체이기 때문에 프로퍼티를 가질 수 있다.

브라우저 콘솔에서 함수 내부를 확인하면 다음과 같다.

![](images/javascript-first-class-object/function_dir.png)

해당 함수를 Object.getOwnPropertyDescriptors 메서드로 확인하면 다음과 같다.

![](images/javascript-first-class-object/function_property.png)

위에서 확인할 수 있듯 arguments, caller, length, name, prototype 프로퍼티는 함수 객체 고유의 데이터 프로퍼티이다.

[[Prototype]]은 접근자 프로퍼티로 Object.prototype 객체의 프로퍼티를 상속받은 것이다.

<br />

**arguments 프로퍼티**

arguments 객체는 함수의 프로퍼티로 호출하는 것은 표준에서 제거되었고 strict mode에서 에러를 발생시킨다.

```javascript
'use strict';
function test() {}
console.log(test.arguments);
// Uncaught TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them at <anonymous>:3:18
```

arguments 프로퍼티의 값은 arguments 프로퍼티 객체이다.

명확히는 유사 배열 객체로 length 속성을 가지고 있고 순회가 가능하지만, Array의 내장 메서드는 가지고 있지 않다.

arguments 객체는 함수 내부에서 이용 가능한 지역 변수로 함수 내의 인수를 참조할 수 있다.

```javascript
function test(x, y) {
    console.log(arguments);
    arguments[0] = 10;
    arguments[1] = 10;
    
    return x * y;
};

test();
test(1);
test(1,2);
test(1,2,3);
```

![](images/javascript-first-class-object/function_arguments.png)

또한 arguments 객체를 사용하여 전달된 인수를 설정하거나 재할당 할 수도 있다.

```javascript
function multiply(x, y) {
    arguments[0] = 10;
    arguments[1] = 10;
    
    return x * y;
};

/*
예상 반환 값은 9이지만 내부에서 인수를 전부 10으로 변경했기 때문에 100이 반환됐다.
*/
multiply(3,3); // 100
```

arguments 객체는 가변 인수가 전달될 때 유용하게 사용될 수 있다.

```javascript
function sumAnything() {
    let result = 0;
    for(const param of arguments) {
        result += param;
    }
    
    return result;
}

sumAnything(1,2); // 3
sumAnything(1,2,3); // 6
sumAnything(1,2,3,4,5); // 15
sumAnything(1,2,3,4,5,6,7,8,9,10); // 55
```

<br />

**caller 프로퍼티**

caller는 함수의 프로퍼티로 호출하는 것은 표준에서 제거되었고 strict mode에서 에러를 발생시킨다.

```javascript
'use strict';
function test() {}
console.log(test.caller);
// Uncaught TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them at <anonymous>:3:18
```

caller 프로퍼티는 함수 자신을 호출한 함수를 가리킨다.

함수가 최상단 레벨에서 호출되었다면 caller는 null이 될 것이고 그렇지 않으면 호출한 함수를 가리킨다.

```javascript
function foo(cb) {
    return cb();
}

function bar() {
    return 'caller : ' + bar.caller;
}

console.log(foo(bar)); // caller : function foo(cb) {...}
console.log(bar()); // caller : null
```

<br />

**length 프로퍼티, name 프로퍼티**

length 프로퍼티는 함수 정의 시 선언한 매개변수의 개수를 가리킨다.

name 프로퍼티는 함수의 이름을 나타내고 익명 함수의 경우 식별자를 값으로 갖는다.

```javascript
function foo() {
    console.log(foo.length); // 0
    console.log(foo.name); // foo
}

const func = function bar(x, y) {
    console.log(func.length); // 2
    console.log(func.name); // bar
}

const anonymous = function(x) {
    console.log(anonymous.length); // 1
    console.log(anonymous.name); // anonymous
}
```

