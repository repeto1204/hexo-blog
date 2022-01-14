---
title: 자바스크립트 프로퍼티 어트리뷰트
categories:
  - language
  - javascript
tags:
  - javascript
toc: true
cover: images/javascript_cover.jpg
thumbnail: images/javascript_cover.jpg
date: 2021-09-27 21:18:29
---

---

<!--more-->

## **내부 슬롯과 내부 메서드**

자바스크립트의 내부 슬롯(internal slot)과 내부 메서드(internal method)는 자바스크립트의 엔진의 알고리즘을 설명하기 위해 ECMAScript에서 사용하는 의사 프로퍼티(pseudo property)와 의사 메서드(pseudo method)이다.

이는 ECMAScript에 이중 대괄호([[...]])로 감싸서 표현되며 엔진에서 실제로 동작하지만 개발자가 접근할 수 있도록 외부에 공개된 것은 아니다.

원칙적으로 접근하거나 호출할 방법이 제공되지 않으나 일부 내부 슬롯, 내부 메서드의 한하여 간접적으로 접근할 수 있는 수단을 제공한다.

```javascript
const o = {};
o.[[prototype]] // Uncaught SyntaxError: Unexpected token '['
o.__proto__ // Object.prototype
```

---

## **디스크립터 객체**

자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 자동으로 정의한다.

이를 디스크립터 객체라고 하는데 데이터(data), 접근자(accessor) 두 가지 형식을 취할 수 있다.

프로퍼티 어트리뷰트는 내부 상태 값인 내부 슬롯이기 때문에 직접 접근할 수 없고 Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인할 수 있다.

<br />

**공유 디스크립터**

데이터 디스크립터와 접근자 디스크립터는 다음의 두 가지 키를 공유한다.

| 프로퍼티 어트리뷰트 | 디스크립터 객체 프로퍼티 | 설명                                  |
| ------------------- | ------------------------ | ------------------------------------- |
| [[Configurable]]    | configurable             | 프로퍼티 재정의 가능 여부를 나타낸다. |
| [[Enumerable]]      | enumerable               | 프로퍼티 열거 가능 여부를 나타낸다.   |

<br />

**데이터 디스크립터**

| 프로퍼티 어트리뷰트 | 디스크립터 객체 프로퍼티 | 설명                                       |
| ------------------- | ------------------------ | ------------------------------------------ |
| [[Value]]           | value                    | 프로퍼티 키를 통해 값에 접근하면 반환된 값 |
| [[Writable]]        | writable                 | 프로퍼티 값의 변경 가능 여부를 나타낸다.   |

```javascript
const user = {
    id: 'userId'
}

console.log(Object.getOwnPropertyDescriptor(user, 'id'));
// {value: 'userId', writable: true, enumerable: true, configurable: true}
```

<br />

**접근자 디스크립터**

| 프로퍼티 어트리뷰트 | 디스크립터 객체 프로퍼티 | 설명                                                         |
| ------------------- | ------------------------ | ------------------------------------------------------------ |
| [[Get]]             | get                      | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수 |
| [[Set]]             | set                      | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수 |

```javascript
const user = {
    id: 'userId',
    nickName: 'useNickName',
    get userId() {
        return this.id;
    },
    set userId(id) {
        this.id = id;
    }
}

console.log(Object.getOwnPropertyDescriptor(user, 'userId'));
// {enumerable: true, configurable: true, get: ƒ, set: ƒ}
```

---

## **프로퍼티 정의**

프로퍼티 어트리뷰트는 새로운 프로퍼티를 정의할 때 명시적으로 정의하거나, 기존 프로퍼티의 값을 재정의할 수 있다.

`Object.defineProperty`, `Object.defineProperties` 메서드를 사용하여 정의한다.

만약 생략한다면 다음과 같은 기본값을 가진다.

| 프로퍼티 어트리뷰트 | 디스크립터 객체 프로퍼티 | 기본값    |
| ------------------- | ------------------------ | --------- |
| [[Configurable]]    | configurable             | false     |
| [[Enumerable]]      | enumerable               | false     |
| [[Value]]           | value                    | undefined |
| [[Writable]]        | writable                 | false     |
| [[Get]]             | get                      | undefined |
| [[Set]]             | set                      | undefined |

```javascript
const user = {};

// 데이터 디스크립터
Object.defineProperty(user, 'id', {
    value: 'userId',
    writable: true,
    enumerable: true,
    configurable: true
})
Object.getOwnPropertyDescriptor(user, 'id');
// {value: 'userId', writable: false, enumerable: false, configurable: false}

Object.defineProperty(user, 'nickName', { value: 'useNickName' })
Object.getOwnPropertyDescriptor(user, 'nickName');
// {value: 'useNickName', writable: false, enumerable: false, configurable: false}

// [[Writable]]의 값이 false라면 [[Value]]값을 변경할 수 없다.
user.nickName = 'newNickName'; // 값을 변경하면 무시된다.

// [[Enumerable]]의 값이 false라면 열거할 수 없다.
Object.keys(user); // ['id']

// [[Configurable]]의 값이 false라면 삭제하거나 재정의 할 수 없다.
delete user.nickName; // 프로퍼티를 삭제하면 무시된다.
Object.defineProperty(user, 'nickName', { configurable: true }); // Uncaught TypeError: Cannot redefine property: nickName

// 접근자 디스크립터
Object.defineProperty(user, 'userId', {
    get() {
        return this.id;
    },
    set(id) {
        this.id = id;
    },
    enumerable: true,
    configurable: true
})
Object.getOwnPropertyDescriptor(user, 'userId');
// {enumerable: true, configurable: true, get: ƒ, set: ƒ}
```

<br />

`Object.defineProperties` 메서드를 사용하면 한 번에 정의할 수 있다.

```javascript
const user = {};

// 데이터 디스크립터
Object.defineProperties(user, {
    id: {
        value: 'userId',
        writable: true,
        enumerable: true,
        configurable: true
    },
    nickName: {
        value: 'nickName'
    },
    userId: {
        get() {
            return this.id;
        },
        set(id) {
            this.id = id;
        },
        enumerable: true,
        configurable: true 
    }
})
console.log(user);
/*
{
	id: "userId",
    userId: 'userId',
    nickName: "nickName",
    get userId: ƒ get(),
    set userId: ƒ set(id),
    [[Prototype]]: Object
}
*/
```

---

## **객체 변경 방지**

객체는 변경 가능한 값이기 때문에 재할당 없이 직접 변경할 수 있다.

만약 중요한 값 또는 변경되지 않아야 하는 값이 객체에 저장되어있다면 객체 변경을 방지해야 한다.

자바스크립트에는 세 가지의 객체 변경 방지 메서드를 제공하는데 각각 변경 금지 강도가 다르다.

| 구분                     | 추가 | 삭제 | 읽기 | 쓰기 | 재정의 |
| ------------------------ | ---- | ---- | ---- | ---- | ------ |
| Object.preventExtensions | X    | O    | O    | O    | O      |
| Object.seal              | X    | X    | O    | O    | X      |
| Object.freeze            | X    | X    | O    | X    | X      |

<br />

**Object.preventExtentions**

새로운 프로퍼티가 객체에 추가되는 것을 방지한다. 즉, 장래의 확장을 방지한다.

프로퍼티는 동적 추가와 `ObjectdefineProperty` 메서드로 추가할 수 있는데 이 두 가지가 모두 금지된다.

객체가 확장 가능 객체인지 여부는 `Object.isExtensible` 메서드로 확인할 수 있다.

```javascript
const obj = { value: 'testValue' };

Object.isExtensible(obj); // true

// 객체의 확장을 방지한다.
Object.preventExtensions(obj); 

Object.isExtensible(obj); // false

obj.name = 'myName'; // 무시된다.
// TypeError: Cannot define property name, object is not extensible
Object.defineProperty(obj, 'name', {
    value: 'myName'
});
console.log(obj); // { value: 'testValue' }

delete obj.value; // 삭제는 가능
console.log(obj); // {}
```

<br />

**Object.seal**

객체를 밀봉하여 새로운 프로퍼티를 추가할 수 없게 하고 현재 존재하는 프로퍼티의 모든 속성을 설정 불가능 상태로 만든다.

하지만 쓰기 가능한 속성만 밀봉 후에도 변경할 수 있게 한다.

객체가 밀봉 상태인지 여부는 `Object.isSealed` 메서드로 확인할 수 있다.

```javascript
const obj = { value: 'testValue' };

Object.isSealed(obj); // false

// 객체를 밀봉한다.
Object.seal(obj); 

Object.isSealed(obj); // true

Object.getOwnPropertyDescriptors(obj);
// value: { value: 'testValue', writable: true, enumerable: true, configurable: false }

obj.name = 'myName'; // 무시된다.
delete obj.value; // 무시된다.

obj.value = 'newValue'; // 변경은 가능하다.
console.log(obj); // { value: 'newValue' }

// TypeError: Cannot redefine property: value
Object.defineProperty(obj, 'value', { configurable: true });
```

<br />

**Object.freeze**

객체를 동결한다. 동결된 객체는 모든 것이 금지되고 읽기만 가능하다.

객체의 동결 여부는 `Object.isFrozen` 메서드로 확인할 수 있다.

```javascript
const obj = { value: 'testValue' };

Object.isFrozen(obj); // false

// 객체를 동결한다.
Object.freeze(obj); 

Object.isFrozen(obj); // true

Object.getOwnPropertyDescriptors(obj);
// value: {value: 'testValue', writable: false, enumerable: true, configurable: false}

obj.name = 'myName'; // 무시된다.
delete obj.value; // 무시된다.
obj.value = 'newValue'; // 무시된다.


// TypeError: Cannot redefine property: value
Object.defineProperty(obj, 'value', { configurable: true });
```

<br />

**불변 객체**

객체 변경 방지 메서드는 중첩객체의 변경 방지를 보장하지 않는다.

프로퍼티가 객체라면 `Object.freeze` 메서드로 동결해도 중첩 객체까지 동결할 수 없다.

```javascript
const user = {
    id: 'userId',
    info: {
        name: 'J',
        age: '18'
    }
}

Object.freeze(user);
Object.isFrozen(user); // true
Object.isFrozen(user.info); // false

user.info.city = 'SEOUL';
console.log(user); // {id: 'userId', info: {name: 'J', age: '18', city: 'SEOUL'}}
```

<br />

중첩 객체까지 전부 동결하여 불변 객체를 만들고 싶다면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 `Object.freeze` 메서드를 호출해야한다.

```javascript
const deepFreeze = (object) => {
    // 객체이고 동결되지 않는 객체만 동결한다.
    if (object && typeof object === 'object' && !Object.isFrozen(object)) {
        Object.freeze(object);
        
        // 모든 프로퍼티를 순회하여 재귀적으로 동결한다.
        for(const key of Object.keys(object)) {
            deepFreeze(object[key]);
        }
    }
    return object;
}

const obj = {
  internal: {
    a: null
  }
};

deepFreeze(obj);

obj2.internal.a = 'anotherValue'; // 무시된다.
obj2.internal.a; // null
```

