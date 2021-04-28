---
title: 자바스크립트 제어문
categories:
  - language
  - javascript
tags:
  - javascript
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg
date: 2021-04-26 10:56:42
---

---

<!--more-->

## **제어문**

자바스크립트 코드는 일반적으로 위에서 아래로 차례대로 실행되는데 이를 제어할 수 있다.

크게 조건에 따라 코드 블록을 실행하는 조건문과 특정 횟수만큼 반복하는 반복문이 있다.

조건문과 반복문을 사용하면 코드의 흐름을 제어하여 특정 코드를 실행할 수 있다.

하지만 과도하게 사용한다면 위에서 아래로 흐르는 직관적인 흐름을 벗어나게 되고 가독성을 저하해 좋지 못한 코드를 만들 가능성이 있다.

---

## **조건문**

조건문은 조건식에 따라 코드 블록을 실행하는 표현식이다.

조건식은 Boolean으로 평가될 수 있는 표현식으로 조건식이 참일 때만 실행한다.

자바스크립트의 조건문은 `if...else`문과 `switch`문이 있다.

<br />

**if...else**

if문은 지정한 조건이 참일 때 명령문을 실행한다. 조건이 거짓인 경우 다른 명령문을 실행하거나 다음 코드 블록을 실행한다.

```javascript
if (조건식) {
	명령문
} else if (조건식) {
	명령문
} else {
	명령문
}
```

if문은 항상 if가 먼저 나와야 하며 else if 또는 else가 처음부터 올 수 없다.

if문 뒤에 나오는 else if는 이전의 if문 또는 else if문이 거짓인 경우 실행하고 if문 뒤에 여러 번 등장할 수 있다.

else문은 이전의 조건문이 전부 거짓인 경우 실행하는 구문으로 마지막에 한 번만 사용할 수 있다.

<br />

```javascript
if(true) {
    // 실행
} else if(true) {
    // if문이 실행되기 때문에 실행되지 않음
}

if(false) {
    // 조건이 false이기 때문에 실행되지 않음
} else if(true) {
    // if문이 실행되지 않고 조건이 true이기 때문에 실행
}

if(false) {
    // 조건이 false이기 때문에 실행되지 않음
} else if(false) {
    // 조건이 false이기 때문에 실행되지 않음
} else {
    // 위 조건이 모두 false이기 때문에 실행
}
```

조건문은 항상 true, false와 같은 boolean형일 필요는 없다. 만약 조건에 어떠한 식이 들어간다면 그 결과를 형 변환하여 평가한다.

<br />

```javascript
const str = '';

if(str) {
    console.log('문자열이 있습니다.');
} else {
    console.log('문자열이 없습니다.')
}
```

str에는 빈 문자열 ''가 들어있지만, 조건문은 이를 형 변환하여 평가한다. ''는 false로 형 변환되고 if문을 건너뛰고 else문을 실행한다.

<br />

if...else의 블록 내의 실행문이 하나뿐이라면 중괄호를 생략할 수 있다.

```javascript
const num = -8;

if(num > 0) console.log('양수');
else if(num < 0) console.log('음수');
else console.log('0');
```

<br />

**switch**

if...else와 같은 조건문으로 표현식이 일치하는 case문을 실행한다. 표현식과 일치하는 case만 실행하고 싶다면 break를 명시적으로 작성해야 한다.

if...else의 else처럼 모든 case와 일치하지 않는 경우는 default문을 사용해서 처리할 수 있다.

```javascript
switch(표현식) {
	case 표현식1: {
		// 실행문
		break;
	}
    case 표현식2: {
		// 실행문
		break;
	}
	default: {
		// 실행문
		break;
	}
}
```

<br />

break는 switch문이나 후에 등장하는 반복문의 실행을 중지하고 다음 코드에 제어권을 넘긴다.

만약 switch문에 break를 사용하지 않으면 표현식과 일치하는 case문부터 마지막 case 또는 default문까지 모두 실행된다.

```javascript
const num = 8;

switch(num) {
	case 0: {
		console.log('0');
	}
    case 5: {
    	console.log('5');
	}
    case 8: {
    	console.log('8');
	}
    case 10: {
	    console.log('10');
	}
    case 100: {
    	console.log('100');
	}
    default: {
    	console.log('default')
	}
}

// 8
// 10
// 100
// default
```

num은 8과 일치하기 때문에 case 8의 구문을 실행한다. 하지만 그 후 switch문을 빠져나가는 break문이 없기 때문에 아래에 있는 모든 실행문을 실행한다.

<br />

switch문의 case는 묶어서 사용할 수도 있다. 두 가지 이상의 case에 하나의 실행문만 실행해야 한다면 case를 중첩하여 적용하면 된다.

```javascript
const num = 8;

switch(num) {
	case 0:
	case 5: {
        console.log('0 또는 5');
        break;
	}
	case 8:
	case 10: {
        console.log('8 또는 10');
        break;
	}
    case 100: {
        console.log('100');
        break;
	}
    default: {
    	console.log('default')
	}
}
// 8 또는 10
```

---

## **반복문**

반복문은 조건식이 참이면 실행문을 실행한다는 점에서 조건문과 같지만, 거짓이 될 때까지 지속해서 반복한다는 차이가 있다.

같은 작업을 반복해야 하거나 특정 자료구조의 개수와 동일한 양의 자료구조를 생성하는 작업에서 코드를 반복 횟수만큼 쓸 필요 없이 반복문을 사용하면 효과적으로 코드의 양을 줄일 수 있다.

기본적인 반복문은 `while, do...while, for`문이 있다.

<br />

**for**

```javascript
for (초기화식; 조건식; 증감식;) {
    실행문
}
```

for문은 초기화식을 기반으로 조건식이 참인 경우 반복문을 수행한다.

1회 반복 후 증감식을 실행하고 그 후 조건식이 참이면 다시 반복하여 조건식이 거짓일 때까지 반복한다.

초기화식에서는 새로운 변수를 선언할 수도 있고 기존의 변수를 초기화하여 사용할 수도 있다.

<br />

```javascript
for (let i = 0; i < 10; i++) {
    console.log(i);
}
```

위 예제에서는 지역변수 키워드로 i를 선언하였고 i가 10보다 작을 때까지 증가시키면서 console.log(i)를 수행하는 코드이다.

i가 9일 때까지 실행되고 i++로 증가하여 10이 됐을 때 i < 10은 거짓이기 때문에 실행문을 실행하지 않고 반복을 종료한다.

<br />

for문의 초기화식, 조건식, 증감식은 생략도 가능하다.

단, 생략 시 무한루프에 빠질 우려가 있는 반복문이라면 반드시 실행문에 조건을 걸어 break로 빠져나오도록 해야 한다.

```javascript
// 초기화식 생략
let i =0;
for(; i < 10; i++) {
    // 실행문
}

// 조건식 생략
for(let i = 0;; i++) {
    // 실행문
    // 조건식이 생략되었기 때문에 조건을 걸어 break를 해주어야 한다.
    if(i > 100) break;
}

// 모두 생략
// 무한 루프
for(;;) {
    // 실행문
    // 무한루프를 탈출할 수 있는 break문을 작성해야 한다.
}
```

<br />

**while, do...while**

while문은 반복 전에 조건을 먼저 검사하고 반복을 시작한다. 보통 반복의 횟수가 명확하지 않을 때 사용한다.

do...while문은 먼저 실행문을 한 번 실행 후 조건을 검사한다.

```javascript
// while
while(조건식) {
	실행문
}

// do...while
do {
	실행문
} while(조건식)
```

<br />

```javascript
let count = 0;
while (count < 10) {
	console.log(count);
	count++;
}

count = 0;
do {
    console.log(count);
    count++;
} while (count < 10)
```

while문과 do...while문은 증감식이 따로 없기 때문에 실행문에서 조건에 대한 증감 처리를 해주어야 한다.

<br />

```javascript
while (true) {
	// 실행문
    // break
}
```

while, do...while문도 for문과 마찬가지로 무한루프를 만들 수 있는데 무한루프 생성 시 실행문 내부에서 조건과 함께 break문을 써주어야 한다.

<br />

## **break, continue, label**

- break: 해당 블록의 반복문 또는 레이블이 지정된 반복문을 종료
- continue: 해당 블록 또는 레이블이 지정된 반복문의 반복 회차를 종료하고 다음 회차의 반복을 수행
- label: 식별자 앞에 붙는 접두어로 break, continue에서 쓰인다.

<br />

```javascript
loop1: // label
for ( let i = 0; i < 10; i++) {
    loop2:
    for (let j = 0; j < 10; j++) {
        if(i === 3 && j === 3) break loop1;
        console.log(i, j);
    }
}
```

break와 label을 조합하면 특정 조건에서 특정 루프를 종료할 수 있다.

중첩이 많은 for문을 실행할 때 모든 루프에 조건을 걸어 break할 필요없이 break 뒤에 종료할 루프의 label을 입력하면 해당 루프가 종료된다.

<br />

```javascript
loop1: // label
for ( let i = 0; i < 10; i++) {
    loop2:
    for (let j = 0; j < 10; j++) {
        if(i === j) continue loop1;
        console.log(i, j);
    }
}
```

label은 continue와 조합할 수도 있다.