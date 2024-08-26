# 21장 빌트인 객체

### **21.1 Javascript 객체의 분류**

- 표준 빌트인 객체 : ECMAScript에 정의된 객체

- 호스트 객체 : ECMAScript에는 정의되어 있지 않지만, Javascript 실행환경에서 추가로 제공하는 객체

- 사용자 정의 객체 : 사용자가 직접 정의한 객체

### **21.2 표준 빌트인 객체**

Javascript는 Object, Function, Symbol 등 40여개의 표준 빌트인 객체를 제공한다. 대부분 생성자 함수 객체다.

생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 제공한다.

생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.

```jsx
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String("Lee"); // String {"Lee"}

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype이다.
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5); // Number {1.5}

// toFixed는 Number.prototype의 프로토타입 메서드다.
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환한다.
console.log(numObj.toFixed()); // 2

// isInteger는 Number의 정적 메서드다.
// Number.isInteger는 인수가 정수(integer)인지 검사하여 그 결과를 Boolean으로 반환한다.
console.log(Number.isInteger(0.5)); // false
```

### **21.3 원시값과 래퍼 객체**

String, Number, Boolean, Symbol 값에 대해 객체처럼 접근하면 생성되는 임시 객체(JS 엔진이 암묵적으로 생성)를 래퍼 객체(wrapper object)라고 한다.

```jsx
const str = "hi";

// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length); // 2
console.log(str.toUpperCase()); // HI

// 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
console.log(typeof str); // string
```

자세한 과정

```jsx
// ① 식별자 str은 문자열을 값으로 가지고 있다.const str = 'hello';

// ② 식별자 str은 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 식별자 str의 값 'hello'는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.
// 래퍼 객체에 name 프로퍼티가 동적 추가된다.
str.name = "Lee";

// ③ 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 이때 ②에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.

// ④ 식별자 str은 새롭게 암묵적으로 생성된(②에서 생성된 래퍼 객체와는 다른) 래퍼 객체를 가리킨다.
// 새롭게 생성된 래퍼 객체에는 name 프로퍼티가 존재하지 않는다.
console.log(str.name); // undefined

// ⑤ 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 이때 ④에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.
console.log(typeof str, str);
```

### **21.4 전역 객체**

전역 객체는 코드가 실행되기 이전 단계에 Javascript 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않는 최상위 객체다. 브라우저 환경에서는 window, node.js환경에서는 global이다.

전역 객체의 특징

- 의도적으로 생성할 수 없다.
- 전역 객체의 프로퍼티를 참조할 때 window(또는 global)를 생략할 수 있다.
- 모든 표준 빌트인 객체를 프로퍼티로 가지고있다.
- 실행환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다.
- var키워드로 선언한 변수, 암묵적 전역, 전역함수는 전역객체의 프로퍼티가 된다.
- 여러 개의 script 태그를 통해 javascript 코드를 분리해도 하나의 전역 객체를 공유한다.

### **21.4.1 빌트인 전역 프로퍼티**

Infinity, NaN, undefined

```jsx
// 전역 프로퍼티는 window를 생략하고 참조할 수 있다.
console.log(window.Infinity === Infinity); // true

console.log(window.NaN); // NaN

console.log(window.undefined); // undefined
```

### **21.4.2 빌트인 전역 함수**

빌트인 전역 함수는 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드다.

**eval**

```jsx
// 표현식인 문
eval("1 + 2;"); // -> 3
// 표현식이 아닌 문
eval("var x = 5;"); // -> undefined

// eval 함수에 의해 런타임에 변수 선언문이 실행되어 x 변수가 선언되었다.
console.log(x); // 5

// 객체 리터럴은 반드시 괄호로 둘러싼다.
const o = eval("({ a: 1 })");
console.log(o); // {a: 1}

// 함수 리터럴은 반드시 괄호로 둘러싼다.
const f = eval("(function() { return 1; })");
console.log(f()); // 1
```

eval 함수를 통해 사용자로부터 입력받은 콘텐츠를 실행하는 것은 보안에 매우 취약하다. 그리고 일반적인 코드실행에 비해 처리속도가 느리다. **따라서 eval 함수의 사용은 금지해야 한다.**

**isFinite**

```jsx
// 인수가 유한수이면 true를 반환한다.
isFinite(0); // -> true
isFinite(2e64); // -> true
isFinite("10"); // -> true: '10' → 10
isFinite(null); // -> true: null → 0

// 인수가 무한수 또는 NaN으로 평가되는 값이라면 false를 반환한다.
isFinite(Infinity); // -> false
isFinite(-Infinity); // -> false

// 인수가 NaN으로 평가되는 값이라면 false를 반환한다.
isFinite(NaN); // -> false
isFinite("Hello"); // -> false
isFinite("2005/12/12"); // -> false
```

**isNaN**

```jsx
// 숫자
isNaN(NaN); // -> true
isNaN(10); // -> false

// 문자열
isNaN("blabla"); // -> true: 'blabla' => NaN
isNaN("10"); // -> false: '10' => 10
isNaN("10.12"); // -> false: '10.12' => 10.12
isNaN(""); // -> false: '' => 0
isNaN(" "); // -> false: ' ' => 0

// 불리언
isNaN(true); // -> false: true → 1
isNaN(null); // -> false: null → 0

// undefined
isNaN(undefined); // -> true: undefined => NaN

// 객체
isNaN({}); // -> true: {} => NaN

// date
isNaN(new Date()); // -> false: new Date() => Number
isNaN(new Date().toString()); // -> true:  String => NaN
```

**parseFloat**

```jsx
// 문자열을 실수로 해석하여 반환한다.
parseFloat("3.14"); // -> 3.14
parseFloat("10.00"); // -> 10

// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseFloat("34 45 66"); // -> 34
parseFloat("40 years"); // -> 40

// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseFloat("He was 40"); // -> NaN

// 앞뒤 공백은 무시된다.
parseFloat(" 60 "); // -> 60
```

**parseInt**

```jsx
// 문자열을 정수로 해석하여 반환한다.
parseInt("10"); // -> 10
parseInt("10.123"); // -> 10

parseInt(10); // -> 10
parseInt(10.123); // -> 10

// 10'을 10진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt("10"); // -> 10
// '10'을 2진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt("10", 2); // -> 2
// '10'을 8진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt("10", 8); // -> 8
// '10'을 16진수로 해석하고 그 결과를 10진수 정수로 반환한다
parseInt("10", 16); // -> 16

// 16진수 리터럴 '0xf'를 16진수로 해석하고 10진수 정수로 그 결과를 반환한다.
parseInt("0xf"); // -> 15
// 위 코드와 같다.
parseInt("f", 16); // -> 15
// 2진수 리터럴(0b로 시작)은 제대로 해석하지 못한다. 0 이후가 무시된다.
parseInt("0b10"); // -> 0
// 8진수 리터럴(ES6에서 도입. 0o로 시작)은 제대로 해석하지 못한다. 0 이후가 무시된다.
parseInt("0o10"); // -> 0

// 'A'는 10진수로 해석할 수 없다.
parseInt("A0"); // -> NaN
// '2'는 2진수로 해석할 수 없다.
parseInt("20", 2); // -> NaN

// 10진수로 해석할 수 없는 'A' 이후의 문자는 모두 무시된다.
parseInt("1A0"); // -> 1
// 2진수로 해석할 수 없는 '2' 이후의 문자는 모두 무시된다.
parseInt("102", 2); // -> 2
// 8진수로 해석할 수 없는 '8' 이후의 문자는 모두 무시된다.
parseInt("58", 8); // -> 5
// 16진수로 해석할 수 없는 'G' 이후의 문자는 모두 무시된다.
parseInt("FG", 16); // -> 15

// 공백으로 구분된 문자열은 첫 번째 문자열만 변환한다.
parseInt("34 45 66"); // -> 34
parseInt("40 years"); // -> 40
// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환한다.
parseInt("He was 40"); // -> NaN
// 앞뒤 공백은 무시된다.
parseInt(" 60 "); // -> 60
```

### **encodeURI / decodeURI**

encodeURI 함수는 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩 한다.

- 이스케이프 처리 - 네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환하는 것

decodeURI 함수는 인코딩 된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩한다.

```jsx
const uri = "http://example.com?name=이웅모&job=programmer&teacher";

// encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩한다.
const enc = encodeURI(uri);
console.log(enc);
// http://example.com?name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher

// decodeURI 함수는 인코딩된 완전한 URI를 전달받아 이스케이프 처리 이전으로 디코딩한다.
const dec = decodeURI(enc);
console.log(dec);
// http://example.com?name=이웅모&job=programmer&teacher
```

### **encodeURIComponent / decodeURIComponent**

encodeURIComponent 함수는 URI 구성 요소를 인수로 전달받아 인코딩 한다.

encodeURIComponent 함수는 인수로 전달된 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주한다. 따라서 쿼리 스트링 구분자로 사용되는 =, ?, & 까지 인코딩한다.

```jsx
// URI의 쿼리 스트링
const uriComp = "name=이웅모&job=programmer&teacher";

// encodeURIComponent 함수는 인수로 전달받은 문자열을 URI의 구성요소인 쿼리 스트링의 일부로 간주한다.
// 따라서 쿼리 스트링 구분자로 사용되는 =, ?, &까지 인코딩한다.
let enc = encodeURIComponent(uriComp);
console.log(enc);
// name%3D%EC%9D%B4%EC%9B%85%EB%AA%A8%26job%3Dprogrammer%26teacher

let dec = decodeURIComponent(enc);
console.log(dec);
// 이웅모&job=programmer&teacher

// encodeURI 함수는 인수로 전달받은 문자열을 완전한 URI로 간주한다.
// 따라서 쿼리 스트링 구분자로 사용되는 =, ?, &를 인코딩하지 않는다.
enc = encodeURI(uriComp);
console.log(enc);
// name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher

dec = decodeURI(enc);
console.log(dec);
// name=이웅모&job=programmer&teacher
```

### **21.4.3 암묵적 전역**

선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 된다.

```jsx
var x = 10; // 전역 변수

function foo() {
  // 선언하지 않은 식별자에 값을 할당
  y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x + y); // 30
```

JS엔진이 y=20을 window.y = 20으로 해석하고 동적 생성한다. 이러한 현상을 암묵적 전역 이라고 한다. 이 때 y는 변수가 아니므로 호이스팅이 발생하지 않는다. 또한 delete 연산자로 삭제할 수 있다. ( 변수는 delete 연산자로 삭제가 불가능하다.)
