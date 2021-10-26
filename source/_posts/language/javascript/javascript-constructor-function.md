---
title: 자바스크립트 생성자 함수
categories:
  - language
  - javascript
tags:
  - javascript
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg
date: 2021-10-26 03:22:44
---


---

<!--more-->

## **생성자 함수로 객체 생성**

객체 리터럴을 사용한 객체 생성 방식은 자바스크립트에서 일반적이고 간단한 객체 생성 방식이다.

객체는 객체 리터럴을 사용하지 않고 생성자 함수를 사용하여 생성할 수 있다.

---

## **Object 생성자 함수**

new 연산자와 Object 생성자 함수를 함께 호출하면 빈 객체를 생성하여 반환한다.

```javascript
// 빈 객체 생성
const author = new Object();

// 프로퍼티 추가
author.name = 'Handsome J';
author.greeting = function() {
    console.log(`Hello! I'm ${this.name}!`);
};
```

생성자 함수란 new 연산자와 함께 호출하여 객체를 생성하는 함수를 뜻한다.

생성자 함수를 사용하여 생성된 객체를 인스턴스라고 한다.

자바스크립트에는 Object 생성자 함수 이외에 String, Number, Boolean, Function, Array 등의 빌트인 생성자 함수를 제공한다.

```javascript
const stringObject = new String('string object');
console.log(typeof stringObject); // object
console.log(stringObject); // String { 'string object' }

const numberObject = new Number(100);
console.log(typeof numberObject); // object
console.log(numberObject); // Number { 100 }

const booleanObject = new Boolean(true);
console.log(typeof booleanObject); // object
console.log(booleanObject); // Boolean { true }
```

객체를 생성하는 방법은 생성자 함수보다 객체 리터럴을 생성하는 것이 더 간편하다.

---

## **생성자 함수**

**객체 리터럴 생성 방식의 문제점**

객체 리터럴은 직관적이고 간편하지만 여러 개의 객체를 생성해야 하는 경우 매번 같은 코드를 기술해야 한다.

```javascript
const user1 = {
    userName: 'user1',
    getUserName() {
        return this.userName;
    }
}

console.log(user1.getUserName()); // user1

const user2 = {
    userName: 'user2',
    getUserName() {
        return this.userName;
    }
}

console.log(user2.getUserName()); // user2
```

만약 상당히 많은 수의 동일한 객체를 생성해야 한다면 위와 같은 방식은 문제가 될 수 있다.

<br />

**생성자 함수 생성 방식의 장점**

생성자 함수를 객체를 생성하기 위한 템플릿처럼 사용하여 구조가 동일한 객체를 간편하게 생성할 수 있다.

```javascript
function User(userName) {
    this.userName = useName;
    this.getUserName = function () {
        return this.userName;
    }
}

const user1 = new User('user1');
const user2 = new User('user2');

console.log(user1.getUserName()); // user1
console.log(user2.getUserName()); // user2
```

생성자 함수는 말 그대로 함수이며 new 연산자와 함께 호출하면 생성자 함수로 동작한다.

만약 new 연산자를 사용하지 않으면 일반 함수로 동작한다.

<br />

**생성자 함수의 객체 생성 과정**

생성자 함수는 다음과 같은 과정을 통해 객체를 생성한다.

1. 인스턴스 생성 및 this 바인딩
   - 생성자 함수를 실행하면 암묵적으로 빈 객체가 생성되며 생성된 빈 객체는 this에 바인딩 된다.
2. 인스턴스 초기화
   - 생성자 함수 내부의 코드를 실행하여 this에 바인딩 돼 있는 객체를 초기화한다.
3. 인스턴스 반환
   - 생성자 함수 내부의 모든 처리가 끝나면 객체가 반환된다.
   - 만약 다른 객체를 명시적으로 반환하면 this가 아닌 해당 객체가 반환된다.

```javascript
function User(userName) {
    // 1
    
    // 2
    this.userName = userName;
    this.getUserName = function () {
        return this.userName;
    }
    
    // 3
    // 반환 객체를 명시하면 명시한 객체가 반환된다.
    // reutnr {};
}

const user1 = new User('user1');
console.log(user1); // User { userName: 'user1', getUserName: f }
```

<br />

**내부 메서드 [[Call]], [[Constructor]]**

함수는 객체지만 일반 객체와 다르게 호출이 가능하다.

따라서, 함수는 일반 객체가 가지고 있는 내부 슬롯, 내부 메서드 뿐만 아니라 함수 객체만 가지는 내부 슬롯과 내부 메서드를 가지고 있다.

함수를 일반 함수로서 호출하면 내부 메서드 [[Call]]이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Constructor]]가 호출된다.

내부 메서드 [[Call]]을 갖는 함수를 callable이라고 하며, 내부 메서드 [[Constructor]]를 갖는 함수를 constructor, 그렇지 않은 함수를 non-constructor라고 한다.

모든 함수 객체는 호출할 수 있기 때문에 항상 [[Call]]을 갖고 있지만 모든 함수 객체가 생성자로서 호출될 수는 없기 때문에 constructor일 수도 non-constructor일 수도 있다.

<br />

**constructor, non-constructor 구분**

자바스크립트 엔진은 함수의 정의를 평가하여 constructor와 non-constructor를 구분한다.

- constructor: 함수 선언문, 함수 표현식, 클래스
- non-constructor: ES6 메서드 축약표 표현, 화살표 함수

```javascript
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {};
const bar = function() {};
// 프로퍼티 x에 할당된 것은 일반 함수
const baz = {
    x: function() {}
}

new foo(); // foo{}
new bar(); // bar{}
new baz.x(); // x{}

// 화살표 함수
const arrow = () => {}; // TypeError: arrow is not a constructor
new arrow();

// 메서드 정의
const obj = {
    x() {}
}

new obj.x(); // TypeError: obj.x is not a constructor
```

<br />

**new 연산자**

일반 함수와 생성자 함수의 형식적 차이는 없다. 호출 시 [[Call]]을 호출하는지 [[Constructor]]를 호출하는지의 차이가 있을 뿐이다.

new 연산자를 사용하면 [[Call]]이 아닌 [[Constructor]]를 호출한다. 단, new 연산자와 함께 호출하는 함수는 constructor이어야 한다.

```javascript
function User(userName) {
    this.userName = userName;
    this.getUserName = function() {
        return this.userName;
    }
}

const userFunction = User('user1');
const userConstructor = new User('user2');

// 함수 호출로 전역에 생성된 userName
console.log(userName); // user1

// 생성자 함수로 반환 된 객체의 프로퍼티
console.log(userConstructor.userName); // user2
```

일반 함수와 생성자 함수의 형식 차이는 존재하지 않지만 구분을 위해 생성자 함수는 일반적으로 첫 문자를 대문자로 기술한다.

<br />

**new.target**

생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 첫 문자를 대문자로 기술한다.

하지만 실수는 언제나 발생할 수 있고 이를 방지하기 위해 new.target을 사용한다.

new.target은 생성자 함수가 호출되면 함수 내부에서 함수 자신을 가리킨다.

만약 new 연산자 없이 일반 함수로 호출되면 new.target은 undefined이다.

```javascript
function User(userName) {
    
    // 만약 new 연산자와 함께 호출되지 않았으면 new 연산자를 붙여 재귀 호출한다.
    if(!new.target) {
        return new User(userName);
    }
    
    this.userName = userName;
    this.getUserName = function() {
        return this.userName;
    }
}

const user1 = User('user1');
console.log(user1.getUserName()); // user1
```

대부분의 빌트인 생성자 함수는 new 연산자와 함께 호출되었는지 확인 후 적절한 값을 반환한다.

단, String, Number, Boolean 생성자 함수는 new 연산자와 함께 호출하면 해당 객체를 그렇지 않으면 해당 데이터 타입의 값을 반환한다.

```javascript
const str = String(123);
console.log(str, typeof str); /// 123 string

const num = Number('123');
console.log(num, typeof num); // 123 number

const bool = Boolean('true');
console.log(bool, typeof bool); // true boolean
```
