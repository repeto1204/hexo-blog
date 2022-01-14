---
title: 자바스크립트 데이터 타입
categories:
  - language
  - javascript
tags:
  - javascript
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg
date: 2021-04-11 17:18:47
---

---

<!--more-->

## **데이터 타입**

자바스크립트의 데이터 타입은 원시 타입(Primitive)과 객체 타입(Object)으로 구분할 수 있다.

- 원시 타입
  - Null
  - Undefined
  - Boolean
  - Number
  - Bigint
  - String
  - Symbol
- 객체 타입
  - Object

---

## **null**

`null`은 어떤 값이 존재하지 않음을 의도적으로 명시하기 위한 표현이다.

`null`은 변수가 어떠한 객체나 값을 가리키고 있지 않음을 표시하기 위해 사용한다.

```javascript
var foo = 10;

// foo는 이제 10을 참조하지 않는다.
foo = null;
```

변수에 `null`을 할당하면 참조를 명시적으로 제거하는 것을 의미한다.

위의 예시에서 10을 저장하고 있는 메모리는 참조되지 않고 자바스크립트 엔진에 의해 제거된다.

또 함수나 API의 결과가 기대한 값이 없거나 유효한 값이 없는 경우 `null`을 반환하기도 한다.

```javascript
var root = document.getElementById('root');

// HTML 문서에 id가 root인 element가 없으면 null을 반환한다.
console.log(root);
```

---

## **undefined**

`undefined`는 선언 된 변수나 파라미터에 자동으로 할당되는 값이다.

다른 언어의 경우 쓰레깃값이 들어 있는 경우가 있는데 자바스크립트의 경우는 엔진이 `undefined`로 초기화한다.

```javascript
var foo;

// foo에 값이 할당되지 않아 자동으로 undefined가 할당된다.
console.log(foo); // undefined

function bar(param) {
	console.log(param);
}
// 인수에 아무 값도 전달하지 않으면 인수에는 undefined가 할당된다.
bar(); // undefined
```

또한 함수에 명시적인 반환 값이 없는 경우 함수는 `undefined`를 반환한다.

```javascript
function foo() {};

var bar = foo();
console.log(bar); // undefined
```

---

## **boolean**

논리적으로 참과 거짓을 나타내는 값, `true`와 `false` 두 가지 값을 가질 수 있다.

명시적으로 값을 선언하거나 표현식이 평가된 값을 할당할 수도 있다.

```javascript
var foo = true;
console.log(foo); // true

var bar = 3 == 4;
console.log(bar); // false
```

---

## **number**

다른 언어의 경우 `int, long, float, double` 등 다양한 숫자 타입이 있는 경우가 있는데 자바스크립트의 숫자 자료형은 정수형, 실수형을 통틀어 `number` 타입을 사용한다.

`number` 타입은 숫자 이외에 `+Infinity, -Infinity, NaN`의 값을 표현할 수 있다.

- Infinity : 양의 무한대
- -Infinity : 음의 무한대
- NaN : Not a Number (숫자가 아님)

자바스크립트의 숫자 표현은 IEEE 754 표준의 일부인 배정밀도 64비트 부동소수점 형식을 따른다.

따라서 안전하게 표현할 수 있는 수의 범위는 -(<code>2<sup>53</sup>-1</code>) ~ <code>2<sup>53</sup>-1</code> 이다.

안전하게 표현한다는 의미는 비교의 정확성을 의미한다.

예를 들어 `Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2`의 결과는 거짓이 되어야 하지만 참으로 평가된다.

안전한 범위를 벗어났기 때문에 올바른 평가가 이루어지지 않은 것이다.

자바스크립트의 모든 수는 사실 실수이다. 정수로 표현되는 수도 내부적으로 실수로 표현되고 있다.

이 때문에 정수끼리의 연산 결과도 실수가 나올 수 있다.

```javascript
console.log(1 === 1.0); // true

console.log(3 / 2); // 1.5
```

<br />

자바스크립트는 2진수, 8진수, 16진수 데이터 타입을 따로 제공하지 않고 이들은 전부 64비트 부동소수점 형식 2진수로 저장된다.

```javascript
const bin = 0b10000; // 2진수
const oct = 0o20; // 8진수
const hex = 0x10; // 16진수

console.log(bin === oct); // true
console.log(oct === hex); // true
```

---

## **bigint**

길이의 제약 없이 정수를 다룰 수 있게 해주는 숫자 형으로 새롭게 추가된 타입이다.

정수 리터럴 끝에 `n`을 붙이거나 `BigInt()`를 사용하여 만들 수 있다.

```javascript
const bigInt = 123456789123456789123456789n;
const newBigInt = BigInt("123456789123456789123456789");
```

<br />

수학 연산이나 비교, 논리 연산도 가능하다.

수학 연산의 경우 일반 숫자와 혼합하여 연산하는 경우 `BigInt()` 또는 `Number()`를 사용해 형 변환을 해주어야한다.

```javascript
console.log(1n + 3n); // 4n
console.log(5n - 2n); // 3n
console.log(10n * 3n); // 30n
console.log(10n / 3n); // 3n, 정수형 값을 반환한다.
console.log(3n + BigInt(3)); // 6n;
console.log(8 + Number(2n)); // 10;
console.log(10n + 20); // TypeError
```

위의 예시처럼 형이 다른 두 숫자 타입끼리의 연산은 **TypeError**를 반환한다.

또 `BigInt`는 언제나 정수이고 소수점 이하를 버림 한다.

<br />

`BigInt`는 아래와 같은 상황에서 `Number`의 결과와 같은 결과를 반환한다.

- `Boolean`을 사용해 객체로 반환될 때
- 논리연산자와 함께 사용될 때
- `if`문 등 조건으로 사용될 때

```javascript
console.log(Boolean(2n)) // true
console.log(2n > 1n); // true
console.log(2n > 1); // true
console.log(1 == 1n); // true
console.log(1 === 1n); // false

if(0n) {
    // 실행 불가
}
```

---

## **string**

자바스크립트에서 문자열을 선언하는 방법은 다음과 같다.

1. 큰따옴표 (")
2. 작은따옴표 (')
3. 역 따옴표/백틱 (`)

```javascript
var string;
string = "STRING";
string = 'STRING';
string = `STRING`;
```

<br />

문자열 내부에서 특수문자를 사용하기 위해 다음과 같은 표현을 사용해야한다.

| **코드** | **출력**    |
| -------- | ----------- |
| \\'      | 작은따옴표  |
| \\"      | 큰따옴표    |
| \\\\     | 역슬래시    |
| \\n      | 개행        |
| \\b      | 백스페이스  |
| \\r      | 캐리지 리턴 |
| \\t      | 탭          |
| \\v      | 세로 탭     |
| \\uXXXX  | 유니코드    |

만약 문자열 내부에서 개행을 하고 싶다면 `\n` 코드를 추가해야 한다.

개행을 원하지 않지만 가독성을 위해 문자열 할당 시 개행을 하고 싶다면 끝에 `\`를 붙여 남은 문자열이 있음을 알린다.

```javascript
var str;
str = '개행이 되는 문자열\n입니다.';
str = '개행을 원하지는 않지만 \
읽는 사람의 편의를 위해 \
엔터를 삽입하고있습니다.';
```

만약 개행을 확인하고 싶으면 `alert` 을 통해 확인해야한다. HTML은 기본적으로 white space 문자열은 하나의 공백으로 변경하여 출력하기 때문에 여러 번의 개행이 작동하지 않거나 `console`에서는 개행 문자열 그대로 출력될 수도 있다.

<br />

문자열의 연결이 필요하다면 `+` 연산을 통해 연결할 수 있다.

```javascript
const str1 = '첫 번째 문자열';
const str2 = '두 번째 문자열';
const str3 = '세 번째 문자열';
console.log(str1 + ' ' + str2 + ' ' + str3); // 첫 번째 문자열 두 번째 문자열 세 번째 문자열
```

<br />

백틱을 사용하면 개행을 편하게 사용할 수 있고 문자열 내부에서 `${}` 으로 감싼 후 자바스크립트 표현식을 입력하면 표현식의 반환 값이 문자열로 형 변환 되어 삽입된다.

```javascript
const template = `글을 쓰다 개행을 하고 싶으면
이렇게 개행을 하면 되고 연산 결과를 출력하고 싶으면 ${1 + 3} 이렇게 하면 된다.`
```

---

## **symbol**

심볼은 유일한 식별자를 만들 때 사용한다. 객체 자료형의 키는 중복되면 값이 덮어 쓰이고 이전 값이 사라지는 문제가 발생하는데 이를 방지하기 위해 유일한 키 값인 심볼을 생성해 사용하는 경우도 있다.

심볼 이외의 원시 자료형은 리터럴을 통해 생성할 수 있지만, 심볼은 `Symbol` 함수를 호출해서 생성한다.

```javascript
// id는 새로운 심볼을 참조한다.
const id = Symbol();
// 심볼에는 설명을 붙일 수 있고 디버깅 시 이를 확인할 수 있다.
const userHandsomeJ = Symbol('HandomeJ Object Key');
// 심볼의 설명을 출력한다.
console.log(userHandsomeJ.description);
```

<br />

심볼은 유일한 값이기 때문에 중복될 수 없고 설명이 같아도 설명만 같을 뿐 다른 심볼이 된다.

```javascript
const sym1 = Symbol('mySymbol');
const sym2 = Symbol('mySymbol');
console.log(sym1 == sym2); // false
console.log(sym1 === sym2); // false
```

---

## **object**

지금까지 알아본 자료형은 전부 원시형(primitive type)이다. 객체형은 원시형과 다르게 키(key)와 값(value)으로 구분된 다양한 자료를 저장할 수 있는 자료구조이다.

키에는 문자형, 심볼형이 들어갈 수 있고, 값에는 모든 자료형이 허용된다.

객체의 값은 키를 이용해 호출할 수 있고 . 또는 []를 통해 호출할 수 있다.

```javascript
const privateNum = Symbol('privateNum');
const obj = {
    name: 'HandsomeJ',
    [privateNum]: 13721987
}

console.log(obj.name); // HandsomeJ
console.log(obj[privateNum]); // 13721987
```