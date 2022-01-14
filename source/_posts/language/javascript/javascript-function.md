---
title: 자바스크립트 함수
categories:
  - language
  - javascript
tags:
  - javascript
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg
date: 2021-08-18 01:15:42
---

---

<!--more-->

## **함수**

자바스크립트에서 함수는 다른 핵심 개념들과 깊은 연관이 있기 때문에 매우 중요하다.

함수는 입력을 받아 연산하여 결괏값을 반환하거나 어떠한 기능을 수행한다.

함수에는 항상 입출력이 필요한 것은 아니며 만약 함수에서 아무것도 반환하지 않는다면 자동으로 undefined를 반환한다.

프로그램에서 어떠한 반복적인 연산이나 반복적인 일련의 과정을 함수로 정의하여 같은 코드를 줄이고 재사용성을 증가시킨다.

---

## **함수의 정의**

```javascript
// 함수 리터럴로 함수 정의
const myFunction = function add(parameter1, parameter2) {
    return parameter1 + parameter2;
}
```
함수의 구성은 아래와 같다.

| 구성                                   | 설명                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| 함수 이름<br />(add)                   | - 함수 이름은 식별자 네이밍 규칙을 준수해야 한다.<br />- 함수 이름은 생략할 수 있고 이름이 있으면 기명 함수(named function), 이름이 없으면 익명 함수(anonymous function)라 한다. |
| 매개변수<br />(parameter1, parameter2) | - 0개 이상의 매개변수가 존재할 수 있고 소괄호 내부에 위치하며 쉼표로 구분한다.<br />- 각 매개변수는 호출 시 인수가 순서대로 할당된다.<br />- 매개변수는 함수 내부에서 변수와 같이 취급되며 식별자 네이밍 규칙을 준수해야한다. |
| 반환값<br />(parameter1 + parameter2)  | - return 키워드를 사용하여 함수의 연산 결과 또는 원하는 결괏값을 반환할 수 있다.<br />- 명시적으로 값을 반환하지 않으면 undefined를 반환한다. |

<br />

함수를 정의하는 방법은 함수 선언문, 함수 표현식, Function 생성자 함수, 화살표 함수가 있으며 네 가지 방법은 모두 함수를 정의하는 방법이지만 약간의 차이가 있다.

<br />

**함수 선언문**

```javascript
function add(x, y) {
    return x + y;
}
```

함수 선언문은 함수 리터럴과 형태가 동일하지만 이름을 생략할 수 없다.

```javascript
function(x, y) {
    return x + y;
}
// SyntaxError: Function statements require a function name
```

함수를 실행하면 반환값이 반환되어 변수에 담긴다. 만약 명시적인 반환값이 없다면 undefined가 반환된다.

<br />

**함수 표현식**

함수는 객체 타입의 값이고 변수에 할당할 수도 있다. 함수는 프로퍼티 값이 될 수 있고 배열의 요소도 될 수 있다.

이렇게 값의 성질을 갖는 객체를 **일급객체**라고 하며 이는 곧 함수를 값처럼 자유롭게 사용할 수 있다는 의미이다.

함수 리터럴로 생성한 함수를 변수에 할당하는 정의 방식을 함수 표현식이라고 한다.

```javascript
const add = function(x, y) {
    console.log(x + y);
}

add(1, 2); // 3


const foo = function bar() {
    console.log('bar');
}

foo(); // 'bar'

bar(); // bar is not defined
```

함수 표현식에 사용하는 함수 리터럴은 함수 이름을 생략할 수 있다. 이를 익명 함수라고 지칭한다.

만약 함수 할당 시 함수에 이름을 붙인 기명 함수를 할당한다고 해도 함수 이름으로 호출할 수는 없다.

함수 이름은 함수 몸체 내부에서만 사용할 수 있기 때문에 함수를 할당한 변수를 통하여 호출해야 한다.

```javascript
function foo() {
    console.log('foo!');
}

foo(); // 'foo!'
```

그렇다면 위 코드는 동작하지 않아야 하는데 실제로 실행해보면 정상 동작한다.

foo는 함수 이름이고 함수 이름은 함수 몸체에서만 사용할 수 있기 때문에 호출이 불가능해야 하는데 호출이 가능하다.

그 이유는 자바스크립트 엔진은 생성된 함수 객체를 가리키는 식별자가 없으면 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생각하고 거기에 함수 객체를 할당한다.

이러한 이유로 호출이 가능한 것이다.

<br />

**Function 생성자 함수**

```javascript
const sum = new Function('x', 'y', 'return a + b');

console.log(sum(4,5)); // 9
```

자바스크립트가 기본으로 제공하는 Function 생성자 함수에 매개변수와 함수 몸체를 문자열로 전달하면 함수 객체를 생성하여 반환한다.

Function 생성자 함수는 new 키워드를 붙인 것과 붙이지 않은 것 모두 같은 함수 객체를 생성하며 코드 크기 측면에서 new 키워드를 붙이지 않는 것이 더 좋다.

Function 생성자 함수는 보안 문제와 eval과 유사한 문제들이 발생할 수 있어 권장되는 방법은 아니다.

```javascript
const closureTest1 = (function(){
    let init = 10;
    return function(x, y) {
        return x + y + init;
    }
}());

console.log(closureTest1(1,2)); // 13

const closureTest2 = (function(){
    let init = 10;
    return new Function('x', 'y', 'return x + y + init');
}());

console.log(closureTest2(1,2)); // init is not defined
```

Function 생성자 함수는 클로저를 생성하지 않는다.

정확히 말해서 전역 범위로 한정된 함수를 생성하기 때문에 클로저 활용이 어렵다.

만약 closureTest2 함수가 동작할 수 있게하고 싶다면 `window.init = 10`과 같이 전역 변수를 등록해주면 가능하다.

하지만 이는 꽤 바보 같은 행위이다.

<br />

**화살표 함수**

ES6에서 도입된 화살표 함수는 function 키워드를 화살표를 사용함으로써 조금 더 간략하게 함수를 선언할 수 있게 해준다.

```javascript
const sum = (x, y) => x + y;

console.log(sum(1,2)); // 3
```

화살표 함수는 항상 익명이다.

화살표 함수는 function 키워드를 대체하기 위해 나온 것은 아니고 모양이 간소화된 만큼 내부 동작도 간소화되었다.

화살표 함수는 this, arguments, super를 바인딩하지 않고 new.target 또한 바인딩하지 않는다. 그래서 화살표 함수는 생성자로 사용할 수 없다.

<br />

**함수 생성 시점과 호이스팅**

```javascript
console.dir(hoistingTest1); // f hoistingTest1(x, y)
console.dir(hoistingTest2); // undefined

console.log(hoistingTest1(1,2)); // 3
console.log(hoistingTest2(1,2)); // TypeError: hoistingTest2 is not a function

// 함수 선언문
function hoistingTest1(x, y) {
    return x + y;
}

// 함수 표현식
var hoistingTest2 = function(x, y) {
    return x + y;
}
```

함수의 경우 자바스크립트 호이스팅이 되어 런타임 이전에 선언이 끌어올려 진다.

함수 선언문의 경우 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행되고 선언 후 암묵적으로 같은 이름의 식별자를 생성하고 함수 객체를 할당한다.

이러한 이유로 함수 선언문으로 선언한 함수는 선언문 이전에 참조도 가능하고 호출도 가능하다.

반면에, 함수 표현식으로 작성한 함수는 변수 호이스팅 규칙을 따른다.

var 키워드는 호이스팅이 되지만 undefined로 초기화되어 참조 시 undefined가 반환된다.

또 선언 이전에 호출을 시도하면 함수 객체가 할당되어있지 않아 에러가 발생한다.

---

## **함수 호출**

정의된 함수는 함수 이름 또는 선언된 함수가 할당된 변수명 뒤에 인수가 나열된 소괄호를 붙이면 함수가 호출된다.

<br />

**매개변수와 인수**

함수 실행 시 외부 값을 함수 내부로 전달이 필요할 때 매개변수(parameter)를 통해 인수(argument)를 전달한다.

```javascript
function myFunc(x, y) {
    return x * y;
}

const result = myFunc(10, 5);
```

매개변수는 함수를 정의할 때 지정하고 호출할 때 인수를 지정한다.

개수와 타입에 제한은 없으며 값으로 평가될 수 있어야 한다. 예약어나 괄호처럼 값으로 평가될 수 없는 것은 허용되지 않는다.

```javascript
function myFunc(x, y) {
    return x * y;
}

const result = myFunc(10);
console.log(result); // NaN
const newResult = myFunc(10, 20, 30);
console.log(newResult); // 200
console.log(x, y); // x is not defined
```

매개변수와 인수의 개수가 일치하지 않아도 에러가 발생하지 않는다.

인수가 부족하면 자동으로 undefined가 할당되며 그대로 함수 내부의 구문이 실행된다. 

다만 그 결괏값이 달라지거나 변수의 프로퍼티를 참조해야 하는 경우라면 에러가 발생할 수도 있다.

인수가 더 많은 경우 초과한 인수는 무시된다.

또 인수는 함수 내부에서만 참조할 수 있다. 함수 외부에서는 참조할 수 없으며 이를 시도하면 에러가 발생한다.

<br />

**반환문**

함수는 return을 통해 특정 값이나 함수 내부 구문의 결괏값을 반환할 수 있다.

return을 명시적으로 사용하지 않으면 함수는 항상 undefined를 반환한다.

```javascript
function returnTest1() {
    return true;
    // 반환문 이후의 코드는 실행되지 않음
    console.log('after return');
}

function returnTest2() {}

console.log(returnTest1()); // true
console.log(returnTest2()); // undefined
```

위 코드를 보면 반환문 이후의 코드는 실행되지 않으며 반환문이 존재하지 않으면 함수는 undefined를 반환한다.

---

## **인수 참조 및 상태 변경**

함수 내부로 전달되는 인수도 변수와 동일하게 취급되기 때문에 값에 의한 전달, 참조에 의한 전달 방식을 그대로 따른다.

```javascript
const obj = {};
const num = 30;

function changeValue(obj, num) {
    obj.test = true;
    num = 100;
    console.log(obj); // {test: true}
    console.log(num); // 100
}

changeValue(obj, num);
console.log(obj); // {test: true}
console.log(num); // 30
```

함수 내부에서 인수를 전달받을 때 원시 타입의 인수는 변경할 수 없기 때문에 내부에서 재할당하여 새로운 원시값으로 교체한다.

그 결과로 함수 내부에서 num 값은 변경한 100인데 함수 실행 후 num을 다시 확인해보면 원래의 값인 30이 반환된다.

원시타입은 값에 의한 전달을 하므로 이러한 결과가 나온다.

반면에 객체 타입은 참조에 의한 전달이기 때문에 함수 내부에서 값을 변경하면 변경 사항이 그대로 유지된다.

함수 내부에서 obj의 test 프로퍼티를 만들고 true라는 값을 할당했다.

함수 실행 후 obj의 값을 확인해보면 변경 사항이 유지된 채로 결과가 반환된다.

하지만 이는 자칫 예기치 못한 오류를 발생시킬 수도 있다.

하나의 객체를 여러 코드 또는 여러 함수에서 공유할 경우 각 함수에서 객체를 변형시킨다면 다른 함수에서 일관된 값이 반환되지 않을 수 있다.

이를 방지하고자 불변의 객체를 만들어 사용하거나 함수 내부에서 객체의 복사본을 새롭게 생성하는 방법을 사용한다.

---

## **함수의 여러 형태**

**즉시 실행 함수**

함수 정의와 동시에 즉시 호출되는 함수를 즉시 실행 함수(IIFE, Immediately invoked Function Expression)라고 한다.

즉시 실행 함수는 단 한 번만 호출되며 다시 실행할 수 없다.

```javascript
(function(){
    console.log('IIFE START');
    return 'IIFE';
}());
```

즉시 실행 함수는 기본적으로 익명 함수이다.

기명 함수를 사용할 수도 있지만, 이때는 함수 리터럴로 평가되어 함수 몸체에서만 함수 이름을 참조할 수 있어 사실상 의미 없는 식별자가 된다.

<br />

**재귀 함수**

자기 자신을 호출하는 함수를 재귀 함수(recursive function)라고 한다.

반복되는 처리를 위해 반복문을 쓰는 대신 재귀 함수를 사용하면 간단하게 구현할 수 있다.

```javascript
// for문을 사용한 팩토리얼
function factorialUseFor(n) {
    let result = 1;
    for(let i = n; i >= 1; i--) {
        result *= i;
    }
    return result;
}

// 재귀 함수를 사용한 팩토리얼
function factorialUseRecursive(n) {
    if(n <= 1) return 1;
    return n * factorialUseRecursive(n - 1);
}

console.log(factorialUseFor(10)); // 3628800
console.log(factorialUseRecursive(10)); // 3628800
```

재귀 함수를 사용할 때 주의할 점은 반드시 멈출 수 있는 조건을 만들어야 한다는 것이다.

재귀 함수는 기본적으로 자기 자신을 무한히 호출하기 때문에 탈출 조건이 없거나 무의미한 탈출 조건을 만든다면 함수가 무한히 호출되어 스택 오버플로를 발생시키고 이는 브라우저를 멈추게 할 수도 있다.

<br />

**중첩 함수**

함수 내부에 정의된 함수를 중첩 함수(nested function)라고 한다.

이는 내부 함수(inner function)라고도 불리며 중첩 함수는 외부 함수의 내부에서만 호출할 수 있다.

```javascript
function outer() {
    let temp = 3;
    function inner() {
        temp += 5;
        console.log(temp); // 8
    }
    
    inner();
}
```

어떠한 함수에서 필요한 기능을 함수로 만들 때 함수 내부에 선언하여 해당 함수를 모듈화하고 외부에서 접근할 수 없게 한다.

이는 함수 외부를 오염시키지 않는 방법이다.

만약 특정 조건이 생성되었을 때(if, while, for) 함수를 정의하는 코드를 작성하게 되면 혼란이 발생할 수 있기 때문에 내부 함수를 선언하는 것이 바람직하다.

<br />

**콜백 함수**

함수의 매개변수를 통해 함수의 내부로 전달되는 함수를 콜백 함수(callback function)라고 한다.

이때 매개변수를 통해 콜백함수를 전달받는 함수를 고차 함수(Higher-Order Function)라고 한다.

콜백 함수로 전달된 함수는 고차 함수가 호출될 때마다 평가되어 함수 객체를 생성한다.

만약 고차함수가 여러 번 호출된다면 콜백 함수를 정의한 후 함수 참조를 전달하는 것이 바람직하다.

```javascript
function hof(char, callbackFunc) {
    callbackFunc(char);
}

const callBack = (char) => { console.log(char); };

hof('test', function(char) { console.log(char)}); // test
hof('test', callBack); // test

```

hof 함수에서 받을 콜백 함수를 선언하고 함수 몸체에서 해당 콜백 함수를 실행했다.

hof 호출 시 익명 함수를 정의하여 전달해도, 정의된 함수의 참조를 전달해도 콜백 함수의 내용이 같다면 그 결과도 같다.

<br />

**순수 함수와 비순수 함수**

어떠한 외부 상태를 의존하지 않고 변경하지 않는 함수를 순수 함수(pure function), 외부 상태에 의존하거나 변경하는 함수를 비순수 함수(impure function)라고 한다.

```javascript
let pureCount = 0;
let impureCount = 0;

// 순수 함수
function increasePureFunction(n) {
    return ++n;
}

// 비순수 함수
function increaseImpureFunction() {
    return ++impureCount;
}

pureCount = increasePureFunction(pureCount);
console.log(pureCount);
pureCount = increasePureFunction(pureCount);
console.log(pureCount);

impureCount = increaseImpureFunction();
console.log(impureCount);
impureCount = increaseImpureFunction();
console.log(impureCount);
```

순수 함수 increasePureFunction는 외부 상태에 의존하지 않으며 외부 상태를 변경하지도 않는다.

인수 n에 의존하여 반환값이 결정된다.

반면에 비순수 함수 increaseImpureFunction는 외부의 impureCount에 의해 반환값이 결정되고 외부 상태도 변경한다.

만약 함수 내부에서 외부 상태를 참조하지 않더라도 매개변수를 통해 참조하여 변경한다면 그것은 비순수 함수에 해당한다.

비순수 함수는 외부 상태를 변경하여 상태 변화 추적을 어렵게 만들기 때문에 최대한 억제하고 순수 함수를 사용하는 것이 좋다.
