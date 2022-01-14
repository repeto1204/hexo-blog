---
title: 자바스크립트 원시값과 객체
categories:
  - language
  - javascript
tags:
  - javascript
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg
date: 2021-07-20 21:49:06
---

---

<!--more-->

## **원시 값과 객체**

자바스크립트의 8가지 데이터 타입(Number, String, Boolean, Null, Undefined, Symbol, Bigint, Object)은 원시 타입(primitive type)과 객체 타입(object type)으로 구분된다.

이처럼 데이터 타입을 원시 타입과 객체 타입으로 분류하는 이유는 다음과 같다.

- 원시 타입은 변경 불가능한 값(immutable value)이고 객체 타입은 변경 가능한 값(mutable value)이다.
- 원시 값을 변수에 저장하면 변수에는 실제 값이 저장되고 객체를 변수에 저장하면 객체 참조 값이 저장된다.
- 원시 값을 가진 변수를 다른 변수에 할당하면 원본의 값이 복사되어 전달(pass by value)되고 객체를 참조하고 있는 변수를 다른 변수에 할당하면 참조 값이 복사되어 전달(pass by reference)된다.

---

## **원시 값**

**변경 불가능한 값**

원시 타입의 값은 변경 불가능한 값이다. 한 번 생성 된 원시 값은 읽기 전용 값으로 변경할 수 없고 불변성을 보장한다.

하지만 이는 변수가 변경될 수 없음을 의미하지 않는다.

변수는 메모리 공간을 식별하기 위해 붙인 이름이고 원시 값은 메모리에 저장된 값을 말하는 것이다.

```javascript
let temp = 10; // temp 값: 10, 10의 메모리 주소: 100
temp = 20; // temp 값: 20, 20의 메모리 주소: 200
```

변수에 원시 값을 할당하면 원시 값이 메모리 공간에 저장되고 변수는 원시 값이 할당된 메모리 공간을 참조한다.

변수에 다른 원시 값을 할당하면 참조하던 메모리 공간의 값을 변경하는 것이 아니라 다른 메모리 공간에 원시 값을 할당하고 변수가 가리키는 주소를 변경한다.

변수에 담긴 원시 값을 변경하는 방법은 재할당밖에 없다. 이러한 특성을 불변성이라고 하는데 이는 변수의 상태 변경 추적을 쉽게 만든다.

이러한 특성이 없다면 메모리에 접근하여 원시 값을 자유롭게 변경할 수 있을 것이고 이는 예기치 못한 값 변경이 발생하거나 변수의 상태 추적을 어렵게 만들 것이다.

<br />

**값에 의한 전달**

```javascript
let temp1 = 100;
let temp2 = temp1;

console.log(temp1); // 100
console.log(temp2); // 100

temp1 = 10;

console.log(temp1); // 10
console.log(temp2); // 100
```

temp1에 100을 할당하고 temp2에 temp1을 할당하면 temp1이 100으로 평가되므로 temp2에도 100이 할당된다.

그 후 temp1에 10을 할당하고 temp1과 temp2를 확인하면 temp1만 변경되어있다.

원시값이 복사될 때는 할당되는 변수에 원시 값이 복사되어 전달되고 이를 값에 의한 전달이라고 한다.

최초 할당 시 temp1과 temp2는 같은 값을 갖고 다른 메모리 주소를 갖는다. 이후 어느 한 변수의 값을 변경해도 서로의 값에 영향을 주지 않는다.

---

## **객체**

객체는 프로퍼티의 개수가 정해져 있지 않고 동적으로 추가, 삭제가 가능하다.

프로퍼티의 타입과 값에도 제한이 없기 때문에 선언 시 메모리 공간의 크기를 사전에 정의할 수가 없다.

객체는 원시 값보다 상대적으로 많은 메모리를 소비하고 때에 따라 크기가 매우 큰 경우도 있을 수 있다.

이러한 이유로 객체는 원시 값과 다른 방식으로 동작하도록 설계되어있다.

<br />

**변경 가능한 값**

객체는 변경 가능한 값이다.

```javascript
const note = {
	price: 2000
}
```

객체를 생성하면 객체를 참조하는 메모리 주소가  변수에 저장된다.

원시 값은 값이 할당된 메모리 주소가 저장되는 반면에 객체는 참조 메모리 주소가 저장된다.

이는 객체의 동적 생성, 삭제의 특성 때문인데 객체에 프로퍼티를 추가, 삭제할 때마다 객체를 재생성할 수 없기 때문에 이러한 방식을 사용한다.

```javascript
const note = {
	price: 2000
}

note.spring = true;
delete note.price;
```

위 코드처럼 객체에 프로퍼티를 추가, 삭제 할 수 있다.

note 변수에는 객체를 참조하는 주소 값이 할당되어있기 때문에 객체를 변경하더라도 해당 주소는 변경되지 않기 때문에 const 선언을 해도 객체의 변경이 가능하다.

<br />

**참조에 의한 전달**

객체를 저장하는 변수는 객체의 메모리 주소를 저장한다. 이러한 특성은 객체 복사 시 참조 복사가 가능하게 한다.

이는 여러 개의 변수가 하나의 객체를 공유할 수 있다는 것이고, 이로 인해 예기치 못한 상황이 발생할 수도 있다.

```javascript
const schoolA = {
    teacher: 30,
    student: 400
}

const schoolB = schoolA;

console.log(schoolA === schoolB) // true

schoolA.teacher = 10;
schoolB.student = 300;

console.log(schoolA); // {teacher: 10, student: 300}
console.log(schoolB); // {teacher: 10, student: 300}
```

schoolA에 객체를 생성하고 schoolB에 schoolA를 복사했다. 이때 객체는 값이 아닌 참조를 복사하기 때문에 두 변수 모두 최초 할당된 객체의 주솟값이 할당된다.

같음 연산자로 비교해보면 동일한 객체로 판명된다.

또 schoolA에 teacher 값을 변경하고, schoolB에 student 값을 변경한 후 두 변수를 확인해보면 모두 변경된 값이 반영되어있다.

객체 복사 시 참조 복사가 되기 때문에 이러한 결과가 발생한다.

<br />

```javascript
const schoolA = {
    teacher: 30,
    student: 400
}

const schoolB = {
    teacher: 30,
    student: 400
}

console.log(schoolA === schoolB) // false

schoolA.teacher = 10;
schoolB.student = 300;

console.log(schoolA); // {teacher: 10, student: 400}
console.log(schoolB); // {teacher: 30, student: 300}
```

객체 리터럴로 선언되는 순간 새로운 객체가 생성된다. 위의 schoolA와 schoolB가 같은 내용이더라도 다른 메모리에 저장되기 때문에 다른 객체로 평가된다.

따라서 변경을 하여도 서로에게 영향을 주지 않는다.
