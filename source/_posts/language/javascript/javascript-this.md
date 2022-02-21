---
title: 자바스크립트 this
categories:
  - language
  - javascript
tags:
  - javascript
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg 
date: 2022-02-21 22:32:50
---

---

<!--more-->

## **this**

this는 자신이 속한 객체 또는 생성할 인스턴스를 가리키는 참조 변수이다.

this를 통해 this가 가리키고 있는 객체의 프로퍼티나 메서드 또는 객체 자체를 참조할 수 있다.

this는 자바스크립트 엔진에 의해 암묵적으로 생성되고 this의 값은 함수 호출 방식에 의해 동적으로 결정된다.

전역 실행 시 this는 언제나 전역 객체를 참조한다. (브라우저의 경우 window)

```javascript
console.log(this); // Window { ... }
const user = {
  userName: 'myUser',
  getUserName() {
    return this.userName;
  }
}

console.log(user.getUserName()); // myUser
```

---

## **함수 호출 방식에 따른 this**

this는 함수 호출 방식에 의해 동적으로 결정된다.

함수 호출 방식은 다음과 같다.

1. 일반 함수 호출
2. 화살표 함수 호출
3. 메서드 호출
4. 생성자 함수 호출
5. `apply, call, bind`에 의한 호출

<br />

**일반 함수 호출**

일반 함수에서 this는 엄격모드와 비엄격모드에서의 동작이 다르다.

비엄격모드에서는 해당 환경의 전역 객체가 바인딩 되고 엄격 모드에서는 undefined가 바인딩 된다.

```javascript
// non strict mode
function nonStrictTest() {
  console.log(this); // Window { ... }
}

// strict mode
function strictTest() {
  'use strict';
  console.log(this); // undefined
}
```

모든 일반 함수 호출에서 this는 전역 객체가 바인딩 된다.

메서드 내부에서 함수가 선언된 경우, 콜백함수 등 일반 함수로 호출된다면 this는 전역 객체이다.

```javascript
const user = {
  userName: 'myUser',
  getUserName() {
    console.log(this); // {userName: 'myUser', getUserName: ƒ}
    console.log(this.userName); // myUser
    
    function thisBindTest() {
      console.log('메서드 내부의 일반함수', this); // 메서드 내부의 일반함수, Window { ... }
    }
    
    setTimeout(function() {
      console.log('콜백 함수', this); // 콜백 함수, Window { ... }
    }, 1000)
    
    thisBindTest();
  }
}

user.getUserName();
```

위 경우 strict mode를 적용하더라도 setTimeout의 콜백 함수의 this는 전역 객체를 반환한다.

```javascript
'use strict';

setTimeout(function() {
  'use strict';
  console.log(this); // Window { ... }
})
```

'use strict'를 어느 곳에 적용하더라도 전역 객체가 바인딩 된다.

setTimeout은 window 객체의 메서드이기 때문에 이는 정상 동작이다.

```javascript
window.setTimeout(function() {
  console.log(this)
})
```

<br />

**화살표 함수 호출**

화살표 함수에서 this는 일반 함수에서 호출과 달리 바인딩할 때 정적으로 결정된다.

화살표 함수의 this는 항상 상위 스코프의 this를 가리킨다.

```javascript
const greeting = {
  prefix: 'Hi',
  greetingUser(arr) {
    return arr.map(x => `${this.prefix}  ${x}`);
  }
}
console.log(greeting.greetingUser(['user1', 'user2'])) // ['Hi  user1', 'Hi  user2']
```

<br />

**메서드 함수 호출**

함수를 메서드로 호출했을 때 그 내부의 this는 메서드를 호출한 객체가 바인딩 된다.

메서드가 선언된 또는 소유한 객체가 아닌 호출 당시의 객체가 바인딩 되는 것이다.

```javascript
const user = {
  id: 'J',
  getThis() {
    return this;
  }
}

const newUser = {
  id: 'K'
}

newUser.getThis = user.getThis;
const getThis = user.getThis;

console.log(user.getThis()); // {id: 'J', getThis: ƒ}
console.log(newUser.getThis()); // {id: 'K', getThis: ƒ}
console.log(getThis()); // Window or undefined
```

user의 getThis 다른 객체와 일반 변수에 할당하여 실행했을 때 getThis는 호출한 객체를 반환했다.

이를 통해 메서드에서 this는 메서드를 호출한 객체임을 할 수 있다.

일반 함수로 호출된 getThis는 전역 객체를 반환하거나 엄격 모드에서 undefined를 반환한다.

같은 개념으로 프로토타입 체인에서도 마찬가지이다.

```javascript
function User(id) {
  this.id = id;
}

User.prototype.getThis = function() { return this; };

const myUser = new User('myUser');

User.prototype.id = 'newId';

console.log(myUser.getThis()); // User {id: 'myUser'}
console.log(User.prototype.getThis()); // {id: 'newId', getThis: ƒ, constructor: ƒ}
```

<br />

**생성자 함수 호출**

생성자 함수의 this는 생성자 함수에 의해 새로 생긴 객체가 바인딩 된다.

```javascript
function Pserson(age) {
  this.age = age;
  this.getAge = function() {
    return this.age;
  }
}

const person1 = new Pserson(99);
const person2 = new Pserson(40);

console.log(person1.getAge()); // 99
console.log(person2.getAge()); // 40
```

함수를 new 연산자와 함께 호출하면 생성자 함수로 동작하고 위와 같은 동작이 발생한다.

만약 new 연산자를 사용하지 않으면 일반 함수처럼 동작한다.

<br />

**apply, call, bind에 의한 호출**

apply, call, bind는 유사하지만 약간의 차이점이 있다. 첫 번째 인자로 this로 사용할 객체로 받는 점은 동일하다.

|         | apply | call  | bind                   |
|---------|-------|-------|------------------------|
| **인수**  | 인수 배열 | 인수 목록 | <center>인수 목록</center> |
| **반환값** | 함수 결과 | 함수 결과 | 새로운 함수                 |

다른 메서드는 인수를 목록으로 받지만 apply는 인수를 배열로 받는다.

반환 값의 경우 apply와 call은 함수의 실행 결괏값을 반환하지만 bind는 this가 바인딩 된 새로운 함수를 반환한다.

```javascript
function getThis() {
  console.log(arguments)
  return this;
}

const bindThis = {
  val: 'new This'
}

console.log(getThis()); // Window
console.log(getThis.apply(bindThis, [1, 2, 3])); // Arguments(3) [ 1, 2, 3, ... ]{ val: 'new This' }
console.log(getThis.call(bindThis, 1, 2, 3)); // Arguments(3) [ 1, 2, 3, ... ]{ val: 'new This' }
console.log(getThis.bind(bindThis, 1, 2, 3)()); // Arguments(3) [ 1, 2, 3, ... ]{ val: 'new This' }
```

bind의 경우 함수를 다시 실행해주어야 올바른 결괏값을 받을 수 있다.

또 apply의 경우 인수를 배열 형식으로 받아야 하는 것을 알 수 있다.