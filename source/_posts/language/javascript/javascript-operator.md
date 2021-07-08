---
title: 자바스크립트 연산자
categories:
  - language
  - javascript
tags:
  - javascript
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg
date: 2021-04-24 13:41:15
---


---

<!--more-->

## **연산자**

프로그래밍 언어에서는 수학 연산과 유사한 연산자 집합을 지원한다.

일반적인 수학 연산자뿐만 아니라 프로그래밍에 특화된 연산을 지원하고 일부 언어에서는 프로그래머가 정의한 연산자 생성을 허용하는 경우도 있다.

자바스크립트에는 할당, 비교, 산술, 증감, 비트, 논리, 문자열, 삼항 등 여러 가지 연산자 집합을 지원한다.

연산자는 피연산자의 개수에 따라 단항, 이항, 삼항으로 나뉘는데 차례대로 피연산자가 한 개, 두 개, 세 개가 필요한 연산자이다.

---

## **산술 연산자**

산술 연산자는 수학적 계산을 수행해서 새로운 숫자를 만들어내는 연산자이다.

연산자는 피연산자를 대상으로 연산자에 맞는 연산을 수행하고 결과를 반환한다.

만약 수행 결과가 숫자가 아니거나 숫자 형으로 변환할 수 없는 경우 `NaN`을 반환한다.

**이항 산술 연산자**

- 덧셈 연산자 `+`
- 뺄셈 연산자 `-`
- 곱셈 연산자 `*`
- 나눗셈 연산자 `/`
- 나머지 연산자 `%`
- 거듭제곱 연산자 `**`

일반적으로 알고 있는 연산자 이외에 `%, **`는 특별한 기능을 제공한다.

**% 연산자**

나머지 연산자 `%`는 `a % b`라는 식이 존재할 때 a를 b로 나눈 나머지를 정수로 반환한다.

```javascript
console.log(10 % 3) // 10을 3으로 나눈 나머지 1 출력
console.log(12 % 3) // 12를 3으로 나눈 나머지 0 출력
console.log(11 % 12) // 11을 12로 나눈 나머지 11 출력
```

<br />

**&ast;&ast; 연산자**

거듭제곱 연산자 `**`는 `a ** b`라는 식이 존재할 때 a를 b번 곱한 값(<code>a<sup>b</sup></code>)을 반환한다.

```javascript
console.log(2 ** 2) // 4
console.log(3 ** 3) // 9
console.log(7 ** 3) // 343
```

<br />

**단항 산술 연산자**

덧셈 연산자 `+`와 뺄셈 연산자 `-`는 단항 연산자로 사용 가능하다.

**+ 연산자**

`+` 연산자는 단항 연산자로 쓰일 때 피연산자의 형을 숫자형으로 변경할 수 있다.

```javascript
console.log(+false); // 0
console.log(+true); // 1
console.log(+'10'); // 숫자 타입 10
console.log(+'test'); // NaN
```

<br />

**- 연산자**

`-` 연산자도 `+` 연산자와 마찬가지로 피연산자의 형변환이 가능하다.

다른점은 `-` 연산자는 숫자형에 쓰일 경우 피연산자의 부호를 변경한다는 것이다.

```javascript
console.log(-(10)); // -10
console.log(-true); // -1
console.log(-(-8)); // 8
console.log(-'test'); // NaN
```

<br />

**문자열 연결**

`+` 연산자는 피연산자 중 하나가 문자열인 경우 숫자형을 문자열로 변경한 후 연결하여 반환한다.

그 외의 경우는 숫자형으로 형변환하여 연산 결과를 반환한다.

```javascript
console.log('9' + 3); // '93'
console.log(10 + '7'); // '17'
console.log(3 + true); // 4
console.log(12 - null); // 12
console.log(10 + undefined); // NaN, undefined는 숫자로 변환되지 않는다.
```

---

## **증가, 감소 연산자**

숫자를 1 증가하거나 1 감소할 때 사용하는 단항 연산자이다.

- 증가 연산자 `++`
- 감소 연산자 `--`

증가, 감소 연산자는 피연산자의 값을 변경하는 특징이 있다. 즉, 암묵적인 할당이 이루어진다.

증가, 감소 연산자는 연산자의 위치에 따라 의미가 달라지는데 피연산자의 앞에 있으면 전위, 뒤에 있으면 후위라고 한다.

전위의 경우 먼저 피연산자를 증가, 감소 후 다른 연산을 수행하는 것이고

후위의 경우 다른 연산을 먼저 수행하고 증가, 감소 연산을 수행하는 것이다.

```javascript
let count = 0;
let temp;
count++;
console.log(count); // 1
count--;
console.log(count); // 0

temp = ++count;
console.log(temp); // 1
console.log(count); // 1
temp = count--;
console.log(temp); // 1
console.log(count); // 0
```

---

## **비교 연산자**

비교 연산자는 좌변과 우변에 있는 피연산자를 비교하여 그 결과를 Boolean 값으로 반환한다.

- 동등 비교 연산자 `==`
- 일치 비교 연산자 `===`
- 부등 비교 연산자 `!=`
- 불일치 비교 연산자 `!==`
- 작음 연산자 `<`
- 작거나 같음 연산자 `<=`
- 큼 연산자 `>`
- 크거나 같음 연산자 `>=`

자바스크립트는 연산 시 암묵적 형 변환을 통해 타입이 자동으로 변환되는 경우가 있는데, 동등 비교, 부등 비교의 경우 형 변환을 통해 타입을 일치시킨 후 값이 같은지 비교한다.

```javascript
console.log(3 == 3) // true
console.log(3 == '3') // true
console.log(3 != 3) // false
console.log(3 != '3') // false
```

이러한 방식은 편할 때도 있지만 예측하기 어려운 결과를 만들어 낼 때가 많다.

<br />

일치 비교, 불일치 비교 연산자를 사용하면 위의 문제를 해결할 수 있다.

일치 비교, 불일치 비교 연산자는 형 변환을 하지 않고 타입까지 비교하여 값과 타입이 모두 같을 때 `true`를 반환한다.

```javascript
console.log(3 === 3) // true
console.log(3 === '3') // false
console.log(3 !== 3) // false
console.log(3 !== '3') // true
```

<br />

크기 비교 연산자의 경우 값의 자료형이 다르면 형 변환 후 비교가 진행된다.

문자열 비교의 경우 유니코드 순으로 비교한다.

```javascript
console.log(3 > '2') // true
console.log(3 > 3) // false
console.log('A' > 'a') // false
console.log(2 <= 2) // true
```

---

## **조건 연산자**

유일한 삼항 연산자로 조건의 결과에 따라 두 개의 값 중 하나를 가질 수 있다. 문법은 아래와 같다.

> 조건 ? 값1 : 값2

조건의 결과가 참이면 값1을 거짓이면 값2를 반환한다.

```javascript
const x = 3;
console.log(x === 3 ? 'x는 3입니다.' : 'x는 3이 아닙니다.');
```

---

## **논리 연산자**

논리 연산자는 피연산자로 Boolean형뿐만 아니라 모든 타입의 값을 받을 수 있다. 연산 결과 역시 모든 타입이 될 수 있고 피연산자 중 하나를 반환한다.

- 논리 AND `&&`
- 논리 OR `||`
- 논리 NOT `!`

```javascript
console.log(true && true); // true
console.log(true || true); // true
console.log(false && true); // false
console.log(false || true); // true
console.log(true && false); // false
console.log(true || false); // true
console.log(!true); // false
console.log(!false); // true
console.log('temp' && 'dummy'); // dummy
```

---

## **기타 연산자**

**typeof 연산자**

`typeof`는 단항 연산자로 피연산자의 타입을 문자열로 반환한다.

```javascript
console.log(typeof ''); // string
console.log(typeof 100); // number
console.log(typeof NaN); // number
console.log(typeof true); // boolean
console.log(typeof undeclaredVariable); // undefined
console.log(typeof Symbol()); // symbol
console.log(typeof BigInt(3)); // bigint
console.log(typeof []); // object
console.log(typeof {}); // object
console.log(typeof function(){}); // function
console.log(typeof null); // object
```

<br />

`typeof`는 8가지 타입을 문자열로 반환한다.

- string
- number
- boolean
- undefined
- symbol
- bigint
- object
- function

`typeof`가 반환하는 타입은 실제 데이터 타입과 항상 일치하는 것은 아니다.

`null`의 경우 원시 자료형에 `null`이 있음에도 `object`를 반환한다.

이는 자바스크립트 초기 버전의 버그로 하위 호환성 때문에 아직 고쳐지지 않은 문제이다.

따라서 `null` 타입을 확인할 때에는 `typeof`가 아닌 일치 비교 연산자 `===`를 사용해야 한다.

<br />

**?. 연산자**

옵셔널 체이닝 연산자는 객체에 에러 없이 접근 가능하게 하는 연산자로 앞의 피연산자가 `null, undefined` 일 때 객체 참조 또는 함수 실행을 멈추고 `undefined`를 반환한다.

```javascript
const myData = {
	name: 'J',
};

const myName = myData.name;
console.log(myName); // 'J'
const myHouse = myData.house?.address;
console.log(myHouse); // undefined
```

위와 같이 객체 내에 없는 프로퍼티를 참조하려고 하면 undefined를 반환한다. 만약 옵셔널 체이닝 연산자를 사용하지 않는다면 아래와 같은 문법을 사용해야 할 것이다.

```javascript
const myData = {
	name: 'J',
};

const myHouse = myData && myData.house && myData.house.address;
console.log(myHouse); // undefined
```

문법이 장황해지지 않는다는 이점이 있다.

<br />

**?? 연산자**

null 병합 연산자는 앞의 좌변을 평가하여 `null, undefined`가 아니면 좌변을 그렇지 않으면 우변을 반환한다.

```javascript
const name = null;
const nickName = null;
const id = 'myId';

console.log(name ?? nickName ?? id); // 'myId'
```

<br />

**delete 연산자**

객체의 프로퍼티를 삭제할 때 사용한다.

```javascript
const myData = {
	name: 'J',
	house: 'Korea'
};

console.log(myData.house); // 'Korea'

delete myData.house;
console.log(myData.house); // undefined
```

<br />

**in 연산자**

객체에 프로퍼티가 존재하는지 확인할 때 사용한다.

```javascript
const myData = {
	name: 'J',
	house: 'Korea'
};

console.log('house' in myData); // true
```

<br />

**new 연산자**

유저가 정의한 객체 타입의 인스턴스를 생성할 때 사용한다.

내장 객체를 생성할 때도 사용할 수 있다.

```javascript
function MyData(name, house, car) {
    this.name = name;
    this.house = house;
    this.car = car;
}

const myData = new MyData('J', null, null);
console.log(myData.name); // 'J'
```

<br />

**instanceof 연산자**

좌변의 객체가 우변의 생성자 함수의 인스턴스인지 확인할 때 사용한다.

```javascript
function MyData(name, house, car) {
    this.name = name;
    this.house = house;
    this.car = car;
}

const myData = new MyData('J', null, null);
console.log(myData instanceof MyData); // true
console.log(myData instanceof String); // false
console.log(myData instanceof Object); // true
```