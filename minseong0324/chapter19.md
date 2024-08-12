# 1. JavaScript는 프로토타입 기반

JavaScript는 **명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍**을 지원하는 멀티 패러다임 프로그래밍 언어.

클래스 기반 언어 특징인 클래스와 public, private, protected와 같은 키워드가 없어서 객체지향 언어가 아니라는 오해를 받긴 하지만, 클래스 기반 객체지향 프로그래밍 언어보다 더 강력한 객체지향 프로그래밍 능력을 갖고 있는 프로토타입 기반 객체지향 프로그래밍 언어이다.

JavaScript를 이루고 있는 **거의 ‘모든 것'이 객체**이다. 원시 타입(string, number, boolean, undefined 등)을 제외한 나머지 값들(**함수, 배열, 정규 표현식 등**)은 모두 **객체**이다.

\*_ 클래스(class) : ES6에서 클래스가 도입되었다. ES6가 기존 프로토타입 기반 객체지향 모델을 폐지하고 새로운 객체지향 모델을 제공하는 것은 아니다. 사실 클래스도 함수이며, 기존 프로토타입 기반 패턴의 문법적 설탕이라 볼 수 있다._

_클래스, 생성자 함수 모두 프로토타입 기반 인스턴스를 생성하나, 동일하게 작동하진 않는다. 클래스는 생성자 함수보다 엄격하며 클래스는 생성자 함수에서는 제공하지 않는 기능도 제공한다._

# 2. 상속과 프로토타입

**상속(inheritance)** : 객체지향 프로그래밍의 핵심 개념으로, 어떤 **객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것**.

JavaScript는 **Prototype을 기반으로 상속을 구현**해 기존의 코드를 적극적으로 재사용함으로써 불필요한 중복을 제거한다.

```jsx
//생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    return Math.PI * this.radius ** 2;
  };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// 이는 메모리 낭비이다.
// getArea 메서드는 모든 인스턴스가 공통으로 사용할 수 있는 메서드이므로
// 중복 생성하지 않고 프로토타입으로 이동하자.
console.log(circle1.getArea === circle2.getArea); // false

console.log(circle1.getArea()); //3.141592653589793
console.log(circle2.getArea()); //12.566370614359172
```

![image 3.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/89b10043-17a3-43fe-8f9f-f3b47bd5beaa/c2b5588e-5ca8-4c91-bde9-ab4a9d9a599e/image_3.png)

```jsx
//생성자 함수
function Circle(radius) {
  this.radius = radius;
}

//Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
//공유해서 사용할 수 있도록 프로토타입에 추가한다.
//프로토타입은 Circle 생성자 함수로 생성한 모든 인스턴스의 부모 역할을 하는 객체이다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체 Circle.prototype의
// getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); //3.141592653589793
console.log(circle2.getArea()); //12.566370614359172
```

![image 4.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/89b10043-17a3-43fe-8f9f-f3b47bd5beaa/0e7cbd17-8678-4da1-81d2-f42c96e94eb8/image_4.png)

기본적으로 radius의 값은 각각의 Circle마다 다를 것이다. 하지만, getArea 메서드는 모두 동일하다.

이러한 경우에는 getArea 메서드는 하나만 생성해서 모든 인스턴스가 공유해서 사용하는 것이 불필요한 중복을 통한 메모리 낭비를 막을 수 있게 된다.

# 3. 프로토타입 객체(===프로토타입)란?

> 자바스크립트는 프로토타입(prototype) 을 기반으로 상속을 구현한다.

- 위에 예제에서 `getArea 메서드` 는 **Circle 객체의 생성자 함수에 메서드로 등록한 것이 아닌, Circle 생성자 함수의 prototype 객체에 메서드로 등록한 것**
  - `생성자 함수에 메서드로 등록할 경우` → 각각의 객체에 `독립적인 동일한 기능을 하는 메서드가 할당` → `메모리 소모`
  - `prototype 에 메서드로 등록할 경우` → 동일한 기능을 하는 `하나의 메서드를 여러 객체가 공유` , 각각의 객체는 `독립된 상태(state)만 관리할 수 있다.`

> 모든 객체(인스턴스)는 자신의 프로토타입 , 즉 상위(부모) 객체 역할을 하는 prototype 의 모든 프로퍼티와 메서드를 상속받는다.

- 모든 객체는 `[[Prototype]]` 이라는 내부 슬롯을 가진다.
  - 이는 `프로토타입의 참조값`(null인 경우도 있음)
  - `[[Prototype]]` 에 저장되는 프로토타입은 `객체 생성 방식` 에 의해 결정
    - 예를 들어, 객체 리터럴( `{ }` ) 로 생성된 객체의 프로토타입 → `Object.prototype`
    - 생성자 함수에 의해 생성된 객체의 프로토타입 → `생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체`
  - 모든 객체는 `하나의 프로토타입을 갖는다.`
    - 모든 프로토타입은 생성자 함수와 `연결` 되어 있다.
    - 즉, `객체 - 프로토타입 - 생성자 함수`는 `연결`되어 있다.
      ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/89b10043-17a3-43fe-8f9f-f3b47bd5beaa/b54b94ea-d917-49be-9b20-750c4aba1e16/Untitled.png)

## 3-1. **`__proto__` 접근자 프로퍼티**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/89b10043-17a3-43fe-8f9f-f3b47bd5beaa/74d62a95-6bf5-40aa-b4a0-f8bdacae408a/Untitled.png)

name: “Lee” → person 객체의 프로퍼티

### `__proto__` 는 접근자 프로퍼티다.

**원래는 내부슬롯과 내부 메서드는 직접적으로 접근이 불가능하다. 모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 `[[Prototype]]`  내부 슬롯에 간접적으로 접근할 수 있다.**

```jsx
const obj = {};
const parent = { x: 1 };

// __proto__ 접근자 프로퍼티의 getter 접근자 함수로 obj 객체의 프로토타입 취득
console.log(obj.__proto__); // [Object: null prototype] {}

// __proto__ 접근자 프로퍼티의 setter 접근자 함수로 obj 객체의 프로토타입에 값 할당
obj.__proto__ = parent;
console.log(obj.__proto__); // { x: 1 }
```

### `__proto__` 접근자 프로퍼티 는 **상속**을 통해 사용된다.

- `__proto__ 접근자 프로퍼티` 는 객체가 직접 소유하고 있는 프로퍼티가 아닌, `Object.prototype 의 프로퍼티다.`
- 즉, 모든 객체는 상속을 통해 `Object.prototype.__proto__ 접근자 프로퍼티` 를 사용할 수 있다.

```jsx
const obj = {};

// obj 객체는 __proto__ 접근자 프로퍼티를 직접 소유하지는 않는다.
// Object.prototype 에 __proto__ 프로퍼티를 상속받아 사용하는 것이다.
console.log(obj.hasOwnProperty("__proto__")); // false

// __proto__ 접근자 프로퍼티는 모든 객체의 프로토타입 객체인 Object.protoype의 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "__proto__"));
/*
{
  get: [Function: get __proto__],
  set: [Function: set __proto__],
  enumerable: false,
  configurable: true
}
*/

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용하는 것임을 증명
console.log(obj.__proto__ === Object.prototype); // true
```

`Object.prototype` : 프로토타입 체인의 종점, 즉 프로토타입 체인의 최상위 객체. 이 객체의 프로퍼티와 메서드는 모든 객체에 상속된다.

### `__proto__ 접근자 프로퍼티` 를 통해 `프로토타입에 접근하는 이유`

- `상호 참조` 에 의해 프로토타입 체인(참조 루프)이 생성되는 것을 방지하기 위해서이다.

  - 프로토타입 체인은 `단방향 연결 리스트로 구현` 되어야 한다.
  - 즉, 프로퍼티 식별자 검색 방향이 `한쪽 방향` 으로만 흘러야 한다.

  ```jsx
  const parent = {};
  const child = {};

  child.__proto__ = parent;
  parent.__proto__ = child; // TypeError: Cyclic __proto__ value
  ```

  ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/89b10043-17a3-43fe-8f9f-f3b47bd5beaa/f13bcd73-8620-4da7-91ed-b8cb8feb7aba/Untitled.png)
  위와 같은 코드가 에러 없이 정상적으로 처리되면 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이 만들어지기 때문에 `__proto__` 접근자 프로퍼티는 에러를 발생시킨다.
  결국, 순환 참조하는 프로토타입 체인이 만들어지면, 프로토타입 체인 종점이 존재하지 않게 되어 프로토타입 체인에서 프로퍼티를 검색할 때 무한 루프에 빠지게 된다.

  ### `__proto__ 접근자 프로퍼티` 를 `코드 내에서 직접 사용하는 것은 권장하는 않는다.`

  - 모든 객체가 **proto** 접근자 프로퍼티를 사용할 수 있는 것이 아니기 때문

  ```jsx
  //obj는 프로토타입 체인의 종점이다. 따라서 Object.__proto__를 상속받을 수 없다.
  const obj = Object.create(null);

  //obj는 Object.__proto__를 상속받을 수 없다.
  console.log(obj.__proto__); //undefined

  //따라서 __proto__보다 Object.getPrototypeOf 메서드를 사용하는 편이 좋다.
  console.log(Object.getPrototypeOf(obj)); //null
  ```

  - 따라서, **proto** 접근자 프로퍼티를 사용하는 대신

    - 프로토타입 취득에 경우 → `Object.getPrototypeOf` 메서드 를
    - 프로토타입 교체에 경우 → `Object.getPrototypeOf` 메서드 를 사용하는 것을 권장

    ```jsx
    const obj = {};
    const parent = { x: 1 };

    // obj 객체의 프로토타입 취득
    console.log(Object.getPrototypeOf(obj)); // [Object: null prototype] {}

    // obj 객체의 프로토타입을 교체
    Object.setPrototypeOf(obj, parent);

    // obj 객체의 __proto__ 접근자 프로퍼티로 obj 객체의 프로토타입 변경사항을 조회
    console.log(obj.__proto__); // { x: 1 }
    ```

## **함수 객체의 prototype 프로퍼티**

함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스(객체)의 프로토타입을 가리킨다.

```jsx
// 함수 객체는 protoype 프로퍼티를 가진다.
console.log(function () {}.hasOwnProperty("prototype")); // true

// 일반 객체는 prototype 프로퍼티를 가지지 않는다.
console.log({}.hasOwnProperty("prototype")); // false
```

• 생성자 함수로서 호출할 수 없는 `non-constructor` 인 `화살표 함수, ES6 메서드 축약 표현으로 정의한 메서드` 는 prototype 프로퍼티를 소유하지 않고, 프로토타입도 생성하지 않는다.

```jsx
// 화살표 함수는 non-constructor이다.
const Person = (name) => {
  this.name = name;
};

//non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(Person.hasOwnProperty("prototype")); //false

//non-constructor는 프로토타입을 생성하지 않는다.
console.log(Person.prototype); //undefined

//ES6의 메서드 축약 표현으로 정의한 메서드는 non-constructor다.
const obj = {
  foo() {},
};

//non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(obj.foo.hasOwnProperty("prototype")); //false

//non-constructor는 프로토타입을 생성하지 않는다.
console.log(obj.foo.prototype); // undefined
```

모든 객체가 가지고 있는 `__proto__ 접근자 프로퍼티` 와 함수 객체만이 가지고 있는 `prototype 프로퍼티`는 동일한 프로토타입을 가리킨다.

```jsx
//생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("KIM");

// Person 생성자 함수의 prototype 프로퍼티와 me 객체(인스턴스)의 __proto__ 접근자 프로퍼티가 가리키는 것은 동일한 프로토타입이다.
console.log(me.__proto__ === Person.prototype); // true
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/89b10043-17a3-43fe-8f9f-f3b47bd5beaa/e25cb704-3cc2-4b10-9c9d-d7d0a5c90811/Untitled.png)

- 다만, 사용하는 주체가 다르다.

| 구분                  | 소유        | 값                  | 사용 주체   | 사용 목적                                                                    |
| --------------------- | ----------- | ------------------- | ----------- | ---------------------------------------------------------------------------- |
| proto 접근자 프로퍼티 | 모든 객체   | 프로토타입의 참조값 | 모든 객체   | 객체가 자신의 프로토타입에 접근 또는 교체하기위해 사용                       |
| prototype 프로퍼티    | constructor | 프로토타입의 참조값 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용 |

## **프로토타입의 constructor 프로퍼티와 생성자 함수**

- 모든 프로토타입은 `constructor 프로퍼티` 를 갖는다.
- 이는, 자신을 참조하고 있는 생성자 함수를 가리킨다.
- 이 연결은 `생성자 함수가 생성될 때`, 즉 `함수 객체가 생성될 때` 이뤄진다.

```jsx
function Person(name) {
  this.name = name;
}

const me = new Person("Kim");

// me 객체의 생성자 함수는 Person
// me 객체의 constructor 프로퍼티(정확히는 me 객체의 프로토타입인 Person.prototype 에 constructor 프로퍼티)
console.log(me.constructor === Person); // true
```

# **리터럴 표기법으로 생성된 객체의 생성자 함수와 프로토타입**

리터럴 표기법( { } ) 으로 생성된 객체의 프로토타입의 경우, constructor 프로퍼티 가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수가 아닐 수 있다.

```jsx
// Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 obj 객체 생성
const obj = {};

// obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); // true
```

obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴에 의해 생성된 객체다. 하지만 obj 객체는 Object 생성자 함수와 constructor 프로퍼티로 연결되어 있다.

그러면 사실 Object 생성자 함수로 생성되는 것은 아닌가?

ECMAScript에 따르면, Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null을 인수로 전달하면서 호출하면 내부적으로는 추상 연산 `OrdinaryObjectCreate`를 호출하여 `Object.prototype`을 프로토타입으로 갖는 빈 객체를 생성한다.

`추상연산` : ECMAScript 내부 동작의 구현 알고리즘

- Object 생성자 함수 호출과 객체 리터럴의 평가는 `OrdinaryObjectCreate`를 호출 해서 `빈 객체를 생성`하는다는 점은 동일
  - 다만, new.target 의 확인, 프로퍼티 추가하는 처리 등 세부 처리에서 차이가 있다.
  - 따라서, 객체 리터럴에 의해 생성된 객체 != Object 생성자 함수가 생성한 객체

# 프로토타입의 생성 시점

프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다. → 단독으로 존재하지 않고 쌍으로 존재.

- 생성자 함수는 `사용자 정의 생성자 함수` 와 자바스크립트가 기본 제공하는 `빌트인 생성자 함수` 로 구분된다.

## **사용자 정의 생성자 함수와 프로토타입 생성 시점**

함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행된다.

함수 정의가 평가 되어 `함수 객체를 생성하는 시점` 에 프로토타입도 더불어 생성

```jsx
// 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입 더불어 생성
// 함수 호이스팅 적용
console.log(Person.prototype); // { constructor: f }

// 생성자 함수
function Person(name) {
  this.name = name;
}
```

생성자 함수로서 호출할 수 없는 함수, 즉 non-construcor는 프로토타입이 생성되지 않는다.

```jsx
const Person = (name) => {
  this.name = name;
};

// non-constructor는 프로토타입이 생성되지 않는다.
console.log(Person.prototype); // undefined
```

## **빌트인 생성자 함수와 프로토타입 생성 시점**

빌트인 생성자 함수는 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성

- 모든 빌트인 생성자 함수는 `전역 객체가 생성되는 시점에 생성`
- 생성된 프로토타입은 빌트인 생성자 함수의 `prototype 프로퍼티에 바인딩`

# **객체 생성 방식과 프로토타입 결정**

1. **객체 리터럴**
2. **Object 생성자 함수**
3. **생성자 함수**
4. **Object.create 메서드**
5. **클래스(ES6)**

다양한 방식의 차이가 있는 객체 생성방식이지만, 추상 연산 OrdinaryObjectCreate 호출에 의해 객체가 생성된다는 것은 동일

## **객체 리터럴에 의해 생성된 객체의 프로토타입**

- `OrdinaryObjectCreate` 호출로 `빈 객체를 생성` 하고
- 객체에 추가할 프로퍼티 목록이 인수로 전달될 경우 `프로퍼티를 객체에 추가`
- 인수로 전달받은 프로토타입을 자신이 생성한 객체의 `[[Prototype]] 내부 슬롯` 에 할당한 다음, `생성한 객체를 반환`

객체 리터럴에 의해 생성되는 객체의 프로토타입은 `Object.prototype`

```jsx
// 객체 리터럴로 생성된 obj 객체
const obj = { x: 1 };

// obj 객체에는 constructor 프로퍼티와 hasOwnProperty 메서드를 소유하지 않는다.
// 하지만 자유롭게 사용할 수 있다.
// 이유는, Object.prototype 에 있는 프로퍼티를 상속받았기 때문이다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty("x")); // true
```

## **Object 생성자 함수에 의해 생성된 객체의 프로토타입**

Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 `Object.prototype`

`new`연산자와 함께 생성자 함수를 호출하여 객체를 생성하는 방식도 마찬가지로 추상연산 `OrdinaryObjectCreate`가 호출된다.

```jsx
function Person(name) {
  this.name = name;
}

const me = new Person("Kim");
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/89b10043-17a3-43fe-8f9f-f3b47bd5beaa/6bb9e552-b12b-44b1-8052-d334c653eb36/Untitled.png)

객체의 프로토타입은 생성자 함수와 생성자 함수에 의해 생성된 객체 사이에 생성되여 둘 사이를 연결한다.

# **프로토타입 체인**

- 프로토타입 체인
  - 자바스크립트 엔진은 프로토타입 체인을 따라 프로퍼티 & 메서드를 검색
  - 객체 간의 상속 관계로 이뤄진 프로토타입의 계층적인 구조에서 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색
  - 따라서, 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘
  - 프로토타입 체인의 최상위 객체는 항상 Object.prototype 이며 이를 프로토타입 체인의 종점(end of prototype chain)
- 스코프 체인
  - 자바스크립트 엔진은 함수의 중첩 관계로 이뤄진 스코프의 계층적 구조에서 식별자를 검색
  - 따라서, 스코프 체인은 `식별자 검색을 위한 메커니즘`

> 스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아닌, 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용

- `스코프 체인` → `프로토타입 체인` 순

# 오버라이딩과 프로퍼티 섀도잉

- 프로토타입이 소유한 프로퍼티 → `프로토타입 프로퍼티`
- 인스턴스가 소유한 프로퍼티 → `인스턴스 프로퍼티`

```jsx
const Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHello = function () {
    console.log(`Hi, My name is ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
})();

const me = new Person("Kim");
// Person.prototype 에 정의한 sayHello 메서드 호출
console.log(me.sayHello()); // Hi, My name is Kim

// me 객체(인스턴스)에 메서드 정의
me.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};
// 인스턴스 메서드 정의 이후에는 Person.prototype 에 sayHello가 아닌, 인스턴스에 정의한 sayHello를 호출
console.log(me.sayHello()); // Hey! My name is Kim
```

- 동일한 이름의 프로퍼티에 대해

  - 인스턴스 메서드가 프로토타입의 메서드를 → `오버라이딩(overriding)`
  - 프로토타입 메서드는 인스턴스 메서드로 인해 프로퍼티가 가려짐 → `프로퍼티 섀도잉(property shadowing)`
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/89b10043-17a3-43fe-8f9f-f3b47bd5beaa/c456c0bb-3860-4605-9aa3-f7117249a11a/Untitled.png)

  ```jsx
  //인스턴스 메서드 사제
  delete me.sayHello;

  //인스턴스에는 SayHello 메서드가 없으므로 프로토타입 메서드 호출
  me.sayHello(); //Hi! My name is Lee
  ```

  근데 여기서 한번더 삭제를 시도하면 프로토타입 체인을 통해 프로토타입 메서드가 삭제되지 않고, 다시 한번 프로토타입 메서드가 호출되어 Hi! My name is Lee가 출력되게 된다.
  결론은, 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색해 프로토타입 프로퍼티를 덮어쓰는게 아니라 추가되는 것임. 이것이 프로퍼티 섀도잉이다.

  # **프로토타입 교체**

  프로토타입은 다른 객체로 변경할 수 있다. 즉, 부모 객체인 프로토타입을 동적으로 변경할 수 있다.

  - 근데, 결론 먼저 말하면 프로토타입 교체를 통해 객체 간의 상속 관계를 동적으로 변경하는 것은 번거로우며, 직접 프로토타입을 교체하는 것은 바람직하지 않다.
  - 따라서, `직접 상속` 이나 `ES6+ 의 클래스` 를 사용하면 간편하고 직관적으로 상속 관계를 구현할 수 있다.

  ## **생성자 함수에 의한 프로토타입 교체**

  ```jsx
  const Person = (function () {
    function Person(name) {
      this.name = name;
    }

    // prototype 프로퍼티를 통해 프로토타입 교체
    Person.prototype = {
      sayHello() {
        console.log(`Hi, My name is ${this.name}`);
      },
    };

    return Person;
  })();

  const me = new Person("Kim");
  ```

  프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없다.
  constructor 프로퍼티는 자바스크립트 엔진이 프로토타입을 생성할때
  암묵적으로 추가한 프로퍼티이다.
  따라서 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다.

  ```jsx
  // 생성자 함수에 프로퍼티로 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴
  console.log(me.constructor === Person); // false
  // 프로토타입 체인을 따라 Object.prototype 의 constructor 프로퍼티가 검색
  console.log(me.constructor === Object); // true
  ```

  이 파괴를 되살리기 위해서는 객체 리터럴에 constructor 프로퍼티를 추가하면 된다.

  ```jsx
  const Person = (function () {
     function Person(name) {
         this.name = name;
     }

     // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입 교체
     Person.prototype = {
  		   //constructor 프로퍼티와 생성자 함수 간의 연결을 설정
         constructor: Person,
         sayHello() {
             console.log(`Hi! My name is ${this.name`);
         }
     }

  return Person;
  }());

  const me = new Person('Lee');

  //이제 Object를 가리키지 않고 Person을 가리킨다.
  console.log(me.constructor === Person); // true
  console.log(me.constructor === Object); // false
  ```

  ## **인스턴스에 의한 프로토타입 교체**

  ```jsx
  function Person(name) {
    this.name = name;
  }

  const me = new Person("Kim");

  // 프로토타입으로 교체할 객체
  const parent = {
    sayHello() {
      console.log(`Hi, My name is ${this.name}`);
    },
  };

  // 1. me 객체의 프로토타입을 parent 객체로 교체한다.
  Object.setPrototypeOf(me, parent);
  // 위 코드는 아래의 코드와 동일하게 동작한다.
  // me.__proto__ = parent;

  me.sayHello(); // Hi! My name is Kim
  ```

  생성자 함수에 의한 프로토타입의 교체와 마찬가지로 프로토타입으로 교체한 객체에는 constructor 프로퍼티가 없으므로 constructor 프로퍼티와 생성자 함수와의 연결이 파괴된다.
  따라서 프로토타입의 constructor 프로퍼티로 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다.

  ```jsx
  console.log(me.constructor === Person); // false
  console.log(me.constructor === Object); // true
  ```

  ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/89b10043-17a3-43fe-8f9f-f3b47bd5beaa/2bb11173-000f-4d54-b8a7-e028849e99f6/Untitled.png)
  프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가하고 생성자 함수의 prototype 프로퍼티를 재설정하여 파괴된 생성자 함수와 프로토타입 간의 연결을 살리는 방법은 다음과 같다.

  ```jsx
  fucntion Person(name) {
    this.name = name;
  }

  const me = new Person("Lee");

  // 프로토타입으로 교체할 객체
  const parent = {
    // constructor 프로퍼티와 생성자 함수간의 연결을 설정
    constructor: Person,
    sayHello(){
      console.log(`Hi! My name is ${this.name}`);
     }
  };

  // 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결을 설정
  Person.prototype = parent;

  // me 객체의 프로토타입을 parent 객체로 교체한다.
  Object.setPrototypeOf(me, parent);
  // 위 코드는 아래의 코드와 동일하게 동작한다.
  // me.__proto__ = parent;

  me.sayHello(); // Hi! My name is Lee

  // constructor 프로퍼티가 생성자 함수를 가리킨다.
  console.log(me.constructor === Person); // true;
  console.log(me.constructor === Object); // false;

  // 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리킨다.
  console.log(Person.prototype === Object.getPrototypeOf(me)) // true
  ```

  # **instanceof 연산자**

  좌변에는 객체를 가리키는 식별자, 우변에는 생성자 함수를 가리키는 식별자를 피연산자로 받는다. 만약 우변의 피연산자가 함수가 아닌 경우 TypeError 를 발생시킨다.

  ```jsx
  객체 instanceof 생성자 함수
  ```

  우변에 생성자 함수에 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고 그렇지 않은 경우에는 false로 평가된다.

  ```jsx
  //생성자 함수
  function Person(name) {
    this.name = name;
  }

  const me = new Person("Kim");

  //Person.prototype이 me 객체의 프로토타입 체인 상에 존재.
  console.log(me instanceof Person); // true;

  //Object.prototype이 me 객체의 프로토타입 체인 상에 존재.
  console.log(me instanceof Object); // true;
  ```

  ```jsx
  function Person(name) {
    this.name = name;
  }

  const me = new Person("Lee");

  // 프로토타입으로 교체할 객체
  const parent = {};

  // 프로토타입으로 교체
  Object.setPrototypeOf(me, parent);

  // Person 생성자함수와 parent 객체는 연결되어있지 않다.
  console.log(Person.prototype === parent); // false
  console.log(parent.constructor === Person); // false

  // Person.prototype이 me 객체의 프로토타입 상에 존재하지 않기 때문에 false
  console.log(me instanceof Person); // false
  // Object.prototype이 me 객체의 프로토타입 상에 존재하기 때문에 true
  console.log(me instanceof Object); // true;
  ```

  프로토타입으로 교체한 parent 객체를 Person 생성자 함수에 prototype 프로퍼티에 바인딩하면 me instanceof Person은 true로 평가된다.

  ```jsx
  function Person(name) {
     this.name = name;
  }

  const me = new Person('Lee');

  // 프로토타입으로 교체할 객체
  const parent = {};

  // 프로토타입으로 교체
  Object.setPrototypeOf(me, parent);

  // Person 생성자함수와 parent 객체는 연결되어있지 않다.
  console.log(Person.prototype === parent); // false
  console.log(parent.constructor === Person); // false

  // parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩한다.
  **Person.prototype = parent;**

  // Person.prototype이 me 객체의 프로토타입 상에 존재하기 때문에 true
  console.log(me instanceof Person); // true
  // Object.prototype이 me 객체의 프로토타입 상에 존재하기 때문에 true
  console.log(me instanceof Object); // true;
  ```

  instanceof 연산자를 함수로 표현하면 다음과 같다.

  ```jsx
  function isInstanceof(instance, constructor) {
  	//프로토타입 취득
    const prototype = Object.getPrototypeof(instance);

  	//재귀 탈출 조건
  	//prototype이 null이면 프로토타입 체인의 종점에 다다른 것.
    if(prototype === null) return false;

  	// 프로토타입이 생성자 함수의 prototype 프로퍼티에 바인딩된 객체라면 true를 반환.
  	// 그렇지 않다면 재귀 호출로 프로토타입 체인 상의 상위 프로토타입으로 이동하여 확인.
    return prototype === constructor.prototype || isInstanceof(prototype, constructor_;
  }

  console.log(isInstanceof(me, Person)); // true
  console.log(isInstanceof(me, Object)); // true
  console.log(isInstanceof(me, Array));; // false
  ```

  즉, instanceof 연산자는 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인하다.

  # **직접 상속**

  ## Object.create에 의한 직접 상속

  Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다.
  다른 객체 생성 방식과 마찬가지로 추상연산 OrdinaryObjectCreate를 호출한다.
  다른 점은 `객체를 생성하면서 직접적으로 상속을 구현한다는 점`

  - **new 연산자 없이도 객체를 생성 가능**
  - **프로토타입을 지정하면서 객체를 생성할 수 있다.**
  - **객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.**

  ```jsx
  /**
  /* 지정된 프로토타입 및 프로퍼티를 같은 새로운 객체를 생성하여 반환한다.
  /* @params {Object} prototype - 생성할 객체의 프로토타입으로 지정할 개체
  /* @params {Object} [properiesObject] - 생성할 객체의 프로퍼티를 갖는 객체
  /* @return {Object} 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체
  /*

  Object.create(prototype, [, properiesObject];
  ```

  ```jsx
  // 프로토타입이 null 인 객체를 생성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
  let obj = Object.create(null);
  console.log(Object.getPrototypeOf(obj) === null); // true
  // Object.prototype를 상속받지 못한다.
  console.log(obj.toString()); // TypeError: obj.toString is not a function

  // obj -> Object.prototype -> null
  // obj = {}; 와 동일하다.
  obj = Object.create(Object.prototype);
  console.log(Object.getPrototypeOf(obj) === Object.prototype); // true;

  // obj -> Object.prototype -> null
  // obj = {x:1}; 과 동일하다.
  obj = Object.create(Object.prototype, {
    x: { value: 1, writable: true, enumerable: true, configurable: true },
  });
  // 위 코드는 아래와 동일하다.
  // obj = Object.create(Object.prototype);
  // obj.x = 1;
  console.log(obj.x); // 1
  console.log(Object.getPrototypeOf(Obj) === Object.prototype); // true

  const myProto = { x: 10 };
  console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

  const myProto = { x: 10 };
  // 임의의 객체를 직접 상속받는다.
  // obj -> myProto -> Object.prototype -> null
  obj = Object.create(myProto);
  console.log(obj.x); // 10
  console.log(Object.getPrototypeOf(obj) === myProto); // true

  // 생성자 함수
  function Person(name) {
    this.name = name;
  }
  // obj -> Person.prototype -> Object.prototype -> null
  // obj -> new Person('Lee') 와 동일하다.
  obj = Object.create(Person.prototype);
  obj.name = "Lee";
  console.log(obj.name); // Lee
  console.log(Object.getPrototypeOf(obj) === Person.prototype); // true
  ```

  - **ESLint 에서는 Object.create의 빌트인 메서드를 객체가 직접 호출하는 것을 권장하지 않는다.**

    - `Object.create 메서드`를 통해 **프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있기 때문**
    - 모든 객체에 종점에는 언제나 `Object.prototype` 이 존재한다는 점을 고려했을 때, 직접 프로토타입 체인의 종점에 위치하는 객체를 생성하는 것은 바람직하지 않다.
    - 그러므로, `Object.prototype 의 빌트인 메서드`는 **간접적으로 호출**하는 것이 바람직하다.

    ```jsx
    // 프로토타입이 null 인 객체를 생성
    const obj = Object.create(null);
    obj.a = 1;

    // console.log(obj.hasOwnProperty("a"));
    // TypeError: obj.hasOwnProperty is not a function

    // Object.prototype의 빌트인 메서드는 객체로 직접 호출하지 않는다.
    console.log(Object.prototype.hasOwnProperty.call(obj, "a")); // true
    ```

    ## **객체 리터럴 내부에서 proto 에 의한 직접 상속**

    **Object.create 로 객체를 생성할 때**, 두 번째 파라미터로 프로퍼티를 정의하는 것에 **번거로움을 해소**하는 방법

    ```jsx
    onst myProto = { x: 10 };

    // 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접상속 받을 수 있다.
    const obj = {
      y: 20,
      //객체를 직접 상속받는다
      // obj -> myProto -> Object.prototype -> null
      __proto__ : myProto
    };

    console.log(obj.x, obj.y); // 10 20
    console.log(Object.getPrototypeOf(obj) === myProto); // true
    ```

    # **정적 프로퍼티/메서드**

    생성자 함수로 인스턴스를 생성하지 않아도 참조/호출 가능한 프로퍼티/메서드

    - 생성자 함수도 `객체` 다.
    - 그러므로, 생성자 함수도 `프로퍼티나 메서드를 소유할 수 있다.`
    - 즉, `생성자 함수가 소유한 프로퍼티나 메서드를 정적 프로퍼티/메서드` 라고 한다.
      - 생성자 함수가 소유하고 있는 정적 프로퍼티/메서드는 `인스턴스에서 직접 참조/호출할 수 없다.`

    ```jsx
    // 생성자 함수
    function Person(name) {
      this.name = name;
    }

    // 프로토타입 메서드
    Person.prototype.sayHello = function () {
      console.log(`HI, My name is ${this.name}`);
    };

    // 정적 프로퍼티
    Person.staticProp = "인간 생성자 함수의 정적 프로퍼티 !";

    // 정적 메서드
    Person.staticMethod = function () {
      console.log("인간 생성자 함수의 정적 메서드 호출 !");
    };

    const me = new Person("WI");

    Person.staticMethod(); // 인간 생성자 함수의 정적 메서드 호출 !
    me.staticMethod(); // TypeError: me.staticMethod is not a function
    ```

    # **프로퍼티 존재 확인**

    ## **in 연산자**

    > 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인 → boolean

    ```
    key in object;
    ```

    - `in 연산자` 는 확인 대상 객체의 프로퍼티뿐만 아니라, `확인 대상 객체가 상속받은 모든 프로토타입의 프로토타입의 프로퍼티를 확인하는 것에 주의할 것`

    ```jsx
    const person = {
      name: "Kim",
      age: 100,
    };

    console.log("name" in person); // true
    console.log("age" in person); // true
    console.log("address" in person); // false << 🔍 address 프로퍼티는 person 객체에 정의되어 있지 않으므로 false

    // toString 메서드는 person 객체의 프로퍼티에는 존재하지 않는다.
    // 하지만, person 객체의 프로토타입인 Object.prototype 에 toString 메서드가 존재하기 때문에 true
    // 이처럼, in 연산자는 조사할 객체의 상속받은 프로토타입의 프로퍼티까지 조사를 한다.
    console.log("toString" in person); // true 🔍
    ```

    ## **Object.prototype.hasOwnProperty 메서드**

    > in 연산자와 동일하게 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인

    - 단, `객체 고유의 프로퍼티 키 인 경우에만 true 를 반환`
    - `상속받은 프로토타입의 프로퍼티 키인 경우는 false 를 반환`

    ```jsx
    const person = {
      name: "Kim",
      age: 100,
    };

    console.log(person.hasOwnProperty("name")); // true
    console.log(person.hasOwnProperty("age")); // true

    // toString 메서드는 person 객체의 프로퍼티에 존재하지 않는다.
    // 객체의 고유 프로퍼티일 때만 true 를 반환하는 Object.prototype.hasOwnProperty 메서드는 false 를 반환
    // toString 은 person 객체에 존재하는 것이 아닌, 그에 상속된 Object.prototype 에 메서드이기 때문이다.
    console.log(person.hasOwnProperty("toString")); // false 🔍
    ```

    # **프로퍼티 열거**

    ## **for - in 문**

    > 객체의 모든 프로퍼티를 순회하며 열거(enumeration)할 필요가 있을 때 사용

    ```
    for (변수선언문 in 객체) { ... }
    ```

    - 정확히는, 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서
      - 프로퍼티 어트리뷰트 `[[Enumerable]]` 의 값이 `true` 인 프로퍼티를 순회하며 열거한다.
    - 프로퍼티 키가 `심벌(Symbol)` 인 프로퍼티는 열거하지 않는다.
    - `for - in` 문은 `순서를 보장하지 않는다.`

    ```jsx
    const person = {
      name: "WI",
      age: 100,

      __proto__: {
        address: "Incheon",
      },
    };

    // in 연산자로 person 객체에 toString 프로퍼티(메서드)가 존재하는지 확인 -> 존재(true)
    console.log("toString" in person); // true

    // 그럼에도 불구하고, toString 메서드는 for - in 문에서 key 에 할당되지 않았다.
    // 이는 toString 의 프로퍼티 어트리뷰트에서 [[Enumerable]] 값이 false 로 설정되어 있기 때문이다.
    // 추가적으로, __proto__ 로 person 객체의 프로토타입에 프로퍼티로 address 프로퍼티를 추가했다. 이 또한 for - in문에 key로 할당
    for (const key in person) {
      console.log(`${key} : ${person[key]}`);
    }
    /*
    name : WI
    age : 100
    address : Incheon
    */
    ```

    - 오로지 해당 객체의 프로퍼티 키들로만 for - in 문을 순회하고 싶을 때는 `Object.prototype.hasOwnProperty` 메서드를 호출하며 검사한다.

    ```jsx
    const person = {
      name: "WI",
      age: 100,

      __proto__: {
        address: "Incheon",
      },
    };

    for (const key in person) {
      // person 객체의 고유 프로퍼티일 경우에만 정보를 출력
      if (person.hasOwnProperty(key)) {
        console.log(`${key} : ${person[key]}`);
      }
    }
    /*
    name : WI
    age : 100
    */
    ```

    ## **객체 고유 프로퍼티만으로 구성된 열거하고 싶을 경우**

    - `Object.keys()` : 객체 자신의 열거 가능한 `프로퍼티 키를 배열로 반환` ( ES6+ )
    - `Object.values()` : 객체 자신의 열거 가능한 `프로퍼티 값을 배열로 반환` ( ES8 )
    - `Object.entries()` : 객체 자신의 열거 가능한 `프로퍼티 키,값을 배열로 반환` ( ES8 )

    ```jsx
    const person = {
      name: "WI",
      age: 100,

      __proto__: {
        address: "Incheon",
      },
    };

    console.log(Object.keys(person)); // [ 'name', 'age' ]
    console.log(Object.values(person)); // [ 'WI', 100 ]
    console.log(Object.entries(person)); // [ [ 'name', 'WI' ], [ 'age', 100 ] ]
    ```
