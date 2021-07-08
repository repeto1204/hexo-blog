---
title: 자바스크립트 객체
categories:
  - language
  - javascript
tags:
  - javascript
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg
date: 2021-07-04 10:13:06
---


---

<!--more-->

## **객체**

자바스크립트에서는 원시 자료형을 제외한 나머지 자료형은 모두 객체이다.

원시 자료형과 달리 객체는 다양한 데이터를 담을 수 있고 키(key)와 값(value)으로 구성된 프로퍼티를 여러 개 넣을 수 있다.

객체의 값은 자바스크립트에서 사용할 수 있는 모든 값이 허용되며 함수도 가능하다.

```javascript
const Owner = {
	name: 'HandsomeJ',
    age: 500,
    greeting: () => { console.log('Hello!'); }
}
```

객체의 값을 프로퍼티(property)라고 부르며 프로퍼티가 함수인 경우 메서드(method)라고 부른다.

---

## **객체의 생성**

객체를 생성하는 방법은 아래와 같다.

- 객체 리터럴
- 생성자 함수
- Object.create 메서드
- 클래스

<br />

**객체 리터럴**

가장 일반적인 객체 생성 방법으로 중괄호를 사용하고 중괄호 내부에 프로퍼티를 정의하면 된다.

```javascript
const User = {
    id: 'USER',
    password: '*********',
    name: 'AAA',
    printInfo: function() {
        console.log(this.id);
        console.log(this.password);
        console.log(this.name);
    }
}
```

<br />

**생성자 함수**

생성자 함수를 사용하여 객체를 생성할 수 있다. 원시 자료형도 생성자 함수로 객체를 생성할 수 있다.

생성자 함수 사용 시  반드시 `new` 키워드와 같이 호출해야 한다.

만약 생성자 함수에서 객체를 반환한다면 결괏값은 반환한 객체가 되고 객체가 아닌 다른 값을 반환한다면

생성자 함수에서 선언한 프로퍼티를 가진 객체가 반환된다.

```javascript
// 1. 생성자 함수를 정의하여 사용
function User() {
    this.id = 'USER';
    this.password = '*********';
    this.name ='AAA';
    
    // return {test: 'test'}
}

const user = new User();
console.log(user);
/*
1. return 비활성화 상태일 때
User {
    id: "USER"
    name: "AAA"
    password: "*********"
}

2. return 활성화 상태일 때
User {
	test: 'test'
}
*/

// 2. 원시 타입을 생성자 함수를 사용하여 생성
const myString = new String('string');
console.log(myString);
/*
String {"string"}
*/
```

<br />

**Object.create 메서드**

Object 객체의 메서드로 인자로 부여한 객체를 프로토타입으로 하는 새 객체를 생성한다.

두 번째 인자에 객체의 프로퍼티 정보를 입력하면 해당 프로퍼티를 가진 객체가 생성된다.

```javascript
function ParentObject() {
    this.x = 3;
}

const Child = Object.create(new ParentObject(), {x: {value: 12}});
```

<br />

**클래스**

ES6부터 사용할 수 있게 된 문법으로 `class` 키워드로 다른 언어의 클래스와 유사한 객체를 생성할 수 있다.

생성자 함수와 차이점은 클래스의 경우 호이스팅이 발생하지 않는다는 점이다.

클래스 내부에서는 `constructor` 메서드를 사용할 수 있는데 이는 클래스를 초기화하는 특수한 메서드로 클래스 내부에 한 개만 존재할 수 있다.

```javascript
class UserTemplate {
    constructor(id, name, age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }
    
    getUserName() {
        return this.name;
    }
}

// extends로 상속이 가능하고 상속 시 super()를 호출하여 부모 객체를 생성해야한다.
class Customer extends UserTemplate {
    constructor(id, name, age, order) {
        super(id, name, age);
        this.order = order;
    }
}

const myCustomer = new Customer('testId', 'testName', 100, 'beer');
myCustomer.getUserName(); // testName
```

---

## **프로퍼티**

객체의 프로퍼티는 키(key)와 값(value)으로 구성되며 각 프로퍼티는 쉼표로 구분한다. 키와 값에 사용할 수 있는 값은 다음과 같다.

- 프로퍼티 키 : 모든 문자열 또는 심볼
- 프로퍼티 값 : 자바스크립트에서 사용 가능한 모든 값

```javascript
const zoo = {
    name: 'myZoo',
    lion: 5,
    monkey: 2
}
```

<br />

**프로퍼티 생성과 접근**

객체 생성 후 프로퍼티를 생성하거나 접근하기 위해서 객체 뒤에 .이나 []를 사용해야한다.

.은 일반적인 키를 선언하거나 해당하는 키로 접근하기 위해 사용하고 []는 접근하려는 키가 문자열로 평가되어야 하거나 계산이 필요할 때 사용한다.

```javascript
const myObject = {};
const key = 'secondKey';

// 선언 시 문자열을 키로 선언하고 싶다면 대괄호를 사용해야한다.
// 또한 결괏값이 문자열로 계산되는 식을 키로 선언할 때에도 대괄호를 사용해야한다.
myObject.firstKey = 'first Value';
myObject[key] = 'second Value';
myObject['thirdKey'] = 'third Value';
myObject['four' + 'th' + 'Key'] = 'fourth Value';

myObject.'lastKey' = 'last Value'; //error, 문자열 키를 대괄호를 사용하지 않으면 에러가 발생한다.
console.log(myObject);
/*
{
	firstKey: "first Value",
	secondKey: "second Value",
	thirdKey: "third Value",
	fourthKey: "fourth Value"
}
*/

myObject['firstKey']; // first Value
myObject['second' + 'Key']; // second Value
myObject.thirdKey; // third Value
myObject[fourthKey] // error, fourthKey라는 키는 존재하지만 따옴표가 없기 때문에 문자열이 아닌 변수로 평가해서 참조 에러가 발생한다.
```

<br />

**프로퍼티 삭제**

`delete` 연산자를 사용하여 프로퍼티를 삭제할 수 있다. 단, 객체 자체를 삭제하는 것은 불가능하다.

```javascript
const myObject = {
    v1: 1,
    v2: 2,
    v3: 3
}
console.log(myObject);
/*
{
    v1: 1,
    v2: 2,
    v3: 3
}
*/

delete myObject.v1;
console.log(myObject);
/*
{
    v2: 2,
    v3: 3
}
*/
delete myObject // 아무 일도 일어나지 않는다.
```

