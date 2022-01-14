---
title: 자바스크립트 strict mode
categories:
  - language
  - javascript
tags:
  - javascript 
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg
date: 2022-01-14 20:10:21
---

---

<!--more-->

## **strict mode**

strict mode는 문자 그대로 엄격 모드이다.

반대되는 개념은 공식 용어는 아니지만 느슨한 모드(sloppy mode)이다.

엄격 모드는 적용 후 해제할 수 없으며 전역적 적용으로 문제가 발생할 가능성이 있다면 부분적으로 적용할 수 있다.

엄격 모드 실행 시 다음과 같은 변화가 일어난다.

1. 무시되던 에러들을 명시적으로 반환한다.
2. 자바스크립트 엔진의 성능 최적화를 어렵게 하는 실수들을 바로잡는다.
3. ECMAScript의 차기 버전에 반영될 문법들을 예약어로 등록하여 사용을 금지한다. 

---

## **strict mode 적용**

strict mode는 전역적 또는 부분적으로 적용할 수 있다.

단, `{}` 괄호로 묶인 블럭문에는 적용되지 않는다.

<br />

**전역 적용**

스크립트 최상단에 `"use strict";` 또는 `'use strict';`를 삽입하면 사용할 수 있다.

strict mode는 적용 후 해제할 수 없으며 최상단이 아닌 중간에 삽입하면 적용되지 않는다

<br />

**부분 적용**

코드의 호환성 문제 때문에 전역적으로 적용하는 경우 오류가 발생할 가능성이 있다면 부분적으로 strict mode를 적용할 수 있다.

```js
// no strict mode

function strict() {
  'use strict';
  // strict mode;
  function nested() {
    // strict mode
  }
}

// no strict mode
```

이때도 마찬가지로 적용하려는 함수 블록 최상단에 `"use strict";` 또는 `'use strict';`를 삽입한다.

<br />

**모듈과 클래스**

1. 모듈을 사용한다면 항상 strict mode로 동작한다.

2. 클래스를 사용한다면 클래스 내부의 모든 코드는 strict mode로 동작한다.

위 두 가지 사례는 `'use strict';`를 삽입하지 않아도 자동으로 실행된다.

---

## **strict mode error**

sloppy mode에서 문제없었지만 strict mode에서 에러를 던지는 경우는 아래와 같다.

**전역 변수 생성**

sloppy mode에서 var, let, const 키워드 없이 변수를 사용하거나 오류가 발생하지 않고 전역 변수가 생성되었다.

하지만 strict mode에서는 이를 금지한다.

```js
"use strict";

testGlobal = 3; // ReferenceError: testGlobal is not defined
```

<br />

**할당할 수 없는 할당**

strict mode에서 쓸 수 없는 프로퍼티에 할당하면 에러를 반환한다.

이와 마찬가지로 읽기 전용 프로퍼티에 할당하는 경우, 확장 불가 객체에 새 프로퍼티를 할당하는 경우 에러를 반환한다.

또한 primitive 값에 프로퍼티를 설정하는 경우도 에러를 반환한다.

```js
"use strict";

undefined = 5; // TypeError: Cannot assign to read only property 'undefined' of object
Infinity = 5; // TypeError: Cannot assign to read only property 'Infinity' of object

const obj = {};
Object.defineProperty(obj, "test", { value: 42, writable: false });
obj.test = 9; // TypeError: Cannot assign to read only property 'test' of object

// getter-only 프로퍼티에 할당
const getObj = { get x() { return 17; } };
obj2.x = 5; // TypeError: Cannot set property x of #<Object> which has only a getter

// 확장 불가 객체에 새 프로퍼티 할당
const preventExtensionObject = {};
Object.preventExtensions(preventExtensionObject);
preventExtensionObject.newProp = "ohai"; // TypeError: Cannot add property newProp, object is not extensible

false.true = 3; // TypeError: Cannot create property 'true' on boolean 'false'
('test').description = 'test word'; // TypeError: Cannot create property 'description' on string 'test'
```

<br />

**삭제할 수 없는 프로퍼티 삭제**

```js
"use strict";
const a = 3;

delete a; // SyntaxError: Delete of an unqualified identifier in strict mode.

delete Object.prototype; // TypeError: Cannot delete property 'prototype' of function Object()
```

<br />

**중복된 함수 파라미터**

아래 코드에서 1)과 2) 둘 중 한 곳에 "use strict"가 있으면 에러를 반환한다.

```js
"use strict"; // 1)
function test(a, b, b){ // !!! 구문 에러
  "use strict"; // 2)
  return a + b + c; // 코드가 실행되면 잘못된 것임
}
// SyntaxError: Duplicate parameter name not allowed in this context
```

<br />

**8진수 문법**

브라우저에서 8진수는 숫자 앞에 0을 붙이는 것으로 지원된다. ex) `0123 === 83`

하지만 일부 개발자들이 숫자 앞의 0을 정렬용으로 사용하면서 의미가 잘못 사용될 여지가 있어 엄격 모드에서는 이를 금지한다.

대신 접두사 `0o`를 붙여서 8진수 표현을 지원하도록 했다.

```js
"use strict";

const octal = 010; // SyntaxError: Octal literals are not allowed in strict mode.

const newOctal = 0o10;
```

<br />

**with 금지**

with은 스코프 체인에 접근해야 하는 경우 편리하게 사용될 수 있지만 사용자나 자바스크립트 엔진에 혼동을 줄 수 있고 버그를 유발할 수 있어 strcit mode에서는 사용이 금지된다.

```js
"use strict";
const x = {y: 3};
with(x) {
  console.log(y); // SyntaxError: Strict mode code may not include a with statement
}
```

---

## **arguments 객체의 동작 변경**

strict mode에서 arguments 객체의 동작은 기존과 조금 다르게 변경된다.

**원본 인수 유지**

기존 코드의 동작에서는 함수 내부에서 arguments의 첫 번째 인수를 변경하면 함수의 인수가 변경되고 그 반대의 경우도 똑같이 동작했다.

하지만 strict mode에서 arguments 객체는 원본 인수를 유지하고 더는 함수의 인수를 추적하지 않는다.

```js
function test(num) {
  'use strict';
  
  num = 100;
  return [num, arguments[0]];
}

console.log(test(10)); // [100, 10];
```

<br />

**arguments.callee 미지원**

이전의 자바스크릡트에는 기명 함수식을 쓸 수 없었다.

이 경우 재귀함수를 만들 수 없어 arguments.callee가 추가되었다.

```js
// 기명 함수가 있는 경우
function factorial (n) {
    return !(n > 1) ? 1 : factorial(n - 1) * n;
}

[1,2,3,4,5].map(factorial);

// 기명 함수가 없는 경우
[1,2,3,4,5].map(function (n) {
  return !(n > 1) ? 1 : arguments.callee(n - 1) * n;
});
```

하지만 이는 다음과 같은 문제가 발생한다.

```js
var global = this;

var sillyFunction = function (recursed) {
  if (!recursed) { arguments.callee(true); }
  if (this !== global) {
    console.log(this); // Arguments 객체
  } else {
    console.log("This is the global");
  }
}

sillyFunction();
```

arguments.callee로 재귀호출 하면 this의 값이 달라지고 내부에서 this 사용하는 로직에 문제가 발생할 수 있다.

이러한 이유로 strict mode에서 arguments.callee는 금지되었다.

```js
"use strict";
const f = function() { return arguments.callee; };
f(); // TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them at f
```