---
title: 자바스크립트 타입 변환
categories:
  - language
  - javascript
tags:
  - javascript
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg
date: 2021-05-17 18:49:18
---

---

<!--more-->

## **타입 변환**

자바스크립트의 모든 값에는 타입이 있다. 값의 타입은 함수와 연산자에 전달될 때 대부분 적절한 자료형으로 자동 변환되거나 개발자가 의도적으로 다른 타입으로 변환할 수 있다.

이러한 행위를 통틀어 타입 변환이라고 하며 명시적 타입변환, 타입 캐스팅(type casting) 또는 암묵적 타입 변환, 형 변환(type conversion)으로 불린다.

---

## **명시적 타입 변환**

개발자가 의도를 가지고 어떠한 값을 특정 타입으로 변경하는 것을 의미한다.

<br />

**문자열 타입으로 변환**

문자열 타입이 아닌 값을 문자열 타입으로 명시적 변환하는 방법은 다음과 같다.

1. String 생성자 함수를 new 연산자 없이 사용
2. Object.prototype.toString 메서드 사용

```javascript
// String 생성자 함수 사용
String(1); // '1'
String(Infinity); // 'infinity'
String(true); // 'true'

// Object.prototype.toString 메서드 사용
(1).toString(); // '1'
(Infinity).toString(); // 'infinity'
(true).toString(); // 'true'
```

<br />

**숫자 타입으로 변환**

숫자 타입이 아닌 값을 숫자 타입으로 명시적 변환하는 방법은 다음과 같다.

1. Number 생성자 함수를 new 연산자 없이 사용
2. parseInt, parseFloat 함수 사용(문자열만 변환 가능)

```javascript
// Number 생성자 함수 사용
Number('0'); // 0
Number('30.4'); // 30.4
Number('-432'); // -432
Number(true); // 1
Number(false); // 0

// parseInt, parseFloat 메서드 사용
parseInt('0'); // 0
parseFloat('30.4') // 30.4
```

<br />

**불린 타입으로 변환**

불린 타입이 아닌 값을 불린 타입으로 명시적 변환하는 방법은 다음과 같다.

1. Boolean 생성자 함수를 new 연산자 없이 사용

```javascript
// Boolean 생성자 함수 사용
Boolean('x'); // true
Boolean(''); // false
Boolean('false'); // true
Boolean(0); // false
Boolean(10); // true
Boolean(NaN); // false
Boolean(undefined); // false
Boolean(null); // false
Boolean({}); // true
Boolean([]); // true
```

---

## **암묵적 타입 변환**

자바스크립트 엔진이 개발자의 의도와 상관없이 코드 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환하는 것을 의미한다.

**문자열 타입으로 변환**

문자 타입으로 암묵적 타입 변환이 발생하는 경우는 다음과 같다.

```javascript
0 + ''; // '0'
-1 + ''; // '-1'
null + ''; // 'null'
undefined + ''; // 'undefined'
```

<br />

위 예제와 같이 + 연산자의 피연산자 중 하나 이상이 문자열인 경우 문자열 연결 연산자로 동작한다.

이때 피연산자 중 문자열이 아닌 피연산자는 암묵적 타입 변환이 발생하여 문자열로 변환된다.

<br />

**숫자 타입으로 변환**

숫자 타입으로 암묵적 타입 변환이 발생하는 경우는 다음과 같다.

```javascript
// 산술 연산자를 사용하는 경우
3 - '1'; // 2
7 * '10'; // 70
100 / 'zero' // NaN

// 단항 연산자를 사용하는 경우
+''; // 0
-'1'; // -1
+'one'; // NaN
+true; // 1
+false; // 0
+null; // 0
+undefined // NaN
```

<br />

+를 제외한 산술 연산자를 사용하면 피연산자 중 숫자 타입이 아닌 피연산자는 숫자 타입으로 암묵적 타입 변환이 발생한다.

또 +, - 단항 연산자를 사용하게 되면 마찬가지로 암묵적 타입 변환이 발생한다.

위의 예제에는 없지만, 비교 연산 시에도 암묵적 타입 변환이 발생한다.

<br />

**불린 타입으로 변환**

불린 타입으로 암묵적 타입 변환이 발생하는 경우는 다음과 같다.

```javascript
// 조건식에 사용됐을 때
if('') {} // false
if('1') {} // true
if(null) {} // false
if({}) {console.log('a')} // true

// 논리 부정 연산자가 사용됐을 때
if(!false) {} // true
if(!'1') {} // false
if(!undefined) {console.log('a')} // true
if(![]) {console.log('a')} // false
```

<br />

자바스크립트에는 Truthy와 Falsy라는 개념이 있는데 이는 각각 참으로 평가되는 값, 거짓으로 평가되는 값을 나타낸다.

아래의 값들은 Falsy 값이다.

- false
- undefined
- null
- 0, -0
- NaN
- '' (빈 문자열)

Falsy 값을 제외한 모든 값을 Truthy 값이다.

자바스크립트에서 불린으로 평가되어야 하는 경우 Truthy는 true로, Falsy는 false로 암묵적 타입 변환이 일어난다.
