
# 15.1 `var` 키워드로 선언한 변수의 문제점

ES5 까지는 `var` 키워드로만 변수를 선언할 수 있었다
그래서 이전 코드를 살펴보면 `var` 이 자주 보이는데, `var` 은 다른 언어들과 다른 독특한 특징이 있기 때문에 잘 알고 넘어가야 한다

## 15.1.1 변수 중복 선언 허용
`var` 로 선언된 키워드는 중복 선언될 수 있다
```js
var x=1;
var x=2;
var x;
```
이래도 에러가 발생하지 않는다
에러가 발생하지 않는다는 것은 경고를 하지 않는다는 것이고
따라서 코드를 작성하다가 중복 선언하여 문제를 일으킬 수 있다




## 15.1.2 함수 레벨 스코프
`var` 은 함수 블록 스코프를 가진다
따라서 `var` 은 그것이 속한 함수 내에서 전역변수 처럼 사용된 수 있다

아래 예제를 보자 
```js
function 함수(){
	console.log(x); //x가 위에 선언되지 않았지만, 전역변수 취급을 받기에 x값을 출력할 수 있다
	if(true){
		var x=10;
	}	
	console.log(x); 
}
```
다른 언어들이라면 if 문을 벗어난 순간 지역 스코프를 벗어나여 호출할 수 없을것이다
하지만 `var` 은 해당 함수 내라면 어디서든 호출할 수 있다!



## 15.1.3 변수 호이스팅

호이스팅은 `var` 이 가지는 독특한 특징이다
4장에서도 깊게 다룬적이 있는데, 우선 자바스크립트 엔진이 함수를 쭉 읽고 사전처럼 변수들을 정리해둔다
그러나 값 할당 이전에 호출하게 되면 `undefined` 가 뜬다
이건 호이스팅 할 때 변수에 할당되는 기본 값이다


![](https://i.imgur.com/HVwEskC.png)


앞선 예시를 다시 보자
```js
function 함수(){
	console.log(x); //x가 위에 선언되지 않았지만, 전역변수 취급을 받기에 x값을 출력할 수 있다
	if(true){
		var x=10;
	}	
	console.log(x); 
}
```

이 경우 `console.log(x)`에서` x`값은 `undefined`로 출력되고 
두번째 `console.log(x)`에서는 10이 출력될것이다



# 15.2 let 키워드

## 15.2.1 변수 중복 선언 금지
단점이 많은 `let` 을 보완하고자 ES6부터 `let` 키워드가 등장했다
`let` 으로 선언한 변수는 중복하여 선언할 수 없다
만약 중복 선언하게 되면 `Syntax Error` 가 발생한다

그래서 다른 언어를 공부하던 사람들이 `var` 을 보면 좀 당황하는데
`let `을 보면 친숙하게 느껴진다


## 15.2.2 블록 레벨 스코프

`var `은 함수 블록을 지역 스코프로 인정했다
하지만 `let` 은 선언된 코드 블록(함수, if문, for, while, try-catch문 등) 을 지역 스코프로 인정하는 **블록레벨 스코프**를 따른다
![](https://i.imgur.com/5EsE4Ej.png)


우리가 사용하는 다른 언어와 비슷해서 이해가 쉬울것이다



## 15.2.3 변수 호이스팅

`let` 키워드로 선언한 변수도 호이스팅이 된다

호이스팅에 있어서는 `let` 과 `const` 가 비슷한 특징을 가지기 때문에 같이 알아보자

 **`let`과 `const`**: 변수 선언이 끌어올려지지만, 선언 전에는 접근할 수 없다. 
 선언 전에 접근을 시도하면 오류가 난다
```js
console.log(b); // ReferenceError: b is not defined 
let b = 20; 
console.log(c); // ReferenceError: c is not defined 
const c = 30;
```
왜냐하면 `let`과 `const`는 선언이 호이스팅되지만 초기화는 호이스팅되지 않는다. 

let과 const는 블록 스코프를 가지며, 블록 내에서만 접근 가능하다

- **`var`**: 변수 선언이 끌어올려져서, 선언 전에 변수에 접근하면 `undefined`가 나온다

```js
console.log(a); // undefined
var a = 10; 
console.log(a); // 10
```
 **`let`과 `const`**: 변수 선언이 끌어올려지지만, 선언 전에는 접근할 수 없다. 
 선언 전에 접근을 시도하면 오류가 난다
```js
console.log(b); // ReferenceError: b is not defined 
let b = 20; 
console.log(c); // ReferenceError: c is not defined 
const c = 30;
```
왜냐하면 `let`과 `const`는 선언이 호이스팅되지만 초기화는 호이스팅되지 않는다. 


### TDZ
이렇게 변수가 선언된 되고 나서부터 초기화되기 전까지의 구간을 일시적 사각지대(TDZ)라고 한다
Temporal Dead Zone

| type | 변수명 | 값      |
| ----- | --- | ------ |
| let  | a   | (비어있음) |
| let   | b   | (비어있음) |
| const | c   | (비어있음) |


## 15.2.4 전역 객체와 let
*var 키워드로 선언한 전역변수와 전역함수, 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 window의 프로터피가 된다. 전역 객체의 프로퍼티를 참조할 때 window를 생략할 수 있다*

### 여기서 window 객체란?
`window` 객체는 브라우저 환경에서의 전역 객체이다 
즉, 웹 페이지를 로드하면 자동으로 생성되며, 자바스크립트 코드 어디에서든 접근할 수 있는 객체이다

window 객체의 3가지 특징에 대해 알아보자

#### 1.  전역 변수와 함수의 컨테이너
브라우저 환경에서는 모든 **전역 변수**와 함수가 `window` 객체의 프로퍼티가 된다
```js
var globalVar = "Hello, World!";
console.log(window.globalVar); //Hello, World!
```

단, 전역변수만 window 객체의 프로퍼티이다
지역 변수 var의 경우는 접근 불가하다
```js
var globalVar = "Hello, World!";
function foo(){
    var localVar="Hi";
}
console.log(window.globalVar); //Hello, World!
console.log(window.localVar); // undefined
```



#### 2. 브라우저 제어:
`window` 객체는 브라우저 창이나 프레임을 제어할 수 있는 다양한 메서드와 프로퍼티를 제공한다

```js
var result = window.confirm("탈퇴하시겠습니까?");
console.log(result); // true 또는 false
typeof(result) //'boolean'
```
![](https://i.imgur.com/noV0rbH.png)
_Q. var 키워드로 선언한 전역변수와 전역함수, 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 `window`의 프로터피가 된다. 전역 객체의 프로퍼티를 참조할 때 `window`를 생략할 수 있다 라고 하셨는데,_

_기본적으로 `window.~~`라고 접근할 수 있는 프로퍼티들이 있는데 예를들어 `window.alert`가 있잖아요_

`var alert = 1;` 이라고 선언 및 초기화를 했을 때 alert를 출력하면 뭐가 찍히나요?

_A. `var alert = 1;`으로 선언하면, `alert`는 숫자 1로 덮어쓰여진다. 이로 인해 `alert` 함수는 더 이상 사용할 수 없고, `alert`를 출력하면 1이 출력된다_

_alert는 사용하는건데 할당 가능한게 신기하다_

##### 열심히 찾아본 alert=1은 되는데 function=1은 안 되는 이유: 예약어와 메서드의 차이
자바스크립트에는 여러 예약어가 있고, 예약어는 *변수명*, 함수명, 객체의 속성 명으로 사용할 수 없다
예약어: `function` `var` `let` `const` `if` `else` `for` `while` `return` ...
대신, `alert`는 예약어가 아니다. `alert`는 ==전역 객체의 메서드==이다 그래서 덮어쓸 수 있는것이다
메세드 이름: `console` `Math` `Date` ... 은 변수명으로 사용 가능하다, 대신 변수에 할당하는 순간 고유의 메서드 기능은 사용하지 못 한다

#### 3.  DOM 접근
`window` 객체를 통해 `document` 객체에 접근할 수 있으며, 이를 통해 DOM(Document Object Model)을 조작할 수 있다.
개발할때 종종 쓰인다
`console.log(window.document);`


#### 정리
전역 변수와 함수는 `window` 객체의 프로퍼티로 취급되기 때문에, `window`를 생략하고 전역 변수와 함수에 접근할 수 있다. (너무 당연하니까 생략하는 느낌)
`console.log(window.document);` -> `console.log(document);`



![](https://i.imgur.com/NiF7VyP.png)

`let `키워드로 선언한 전역변수는 전역객체 프로퍼티가 아니다
즉, `window.foo`와 같이 접근할 수 없다
`let` 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드, 이에 대해서는 23장 실행 컨텍스트에서 자세히 살펴보게 된다) 내에서 존재하게 된다


# 15.3 const 키워드

`const `는 상수를 선언하기 위해 사용된다


## 15.3.1 선언과 초기화

`const `키워드로 선언한 변수는 반드시 *선언과 동시에 초기화를 해야 한다!*, 이것이 `let`과의 차이점이다
그렇지 않으면 아래와 같은 문법 에러가 발생한다
`SyntaxError: Missing initializer in const declaration`

`const` 키워드와 마찬가지로 블록 레벨 스코프를 가지며, 호이스팅이 일어나지만 값 할당 이전까지는 TDZ에 속하여 호이스팅이 일어나지 않는것 처럼 보인다



## 15.3.2 재할당 금지

`const `키워드로 선언된 변수는 재할당 할 수 없다. 
(그런데 뒤에 재할당 가능한 경우도 나온다!)


## 15.3.3 상수

`const `키워드로 선언한 변수에 값을 할당한 경우 값을 변경할 수 없다. 재할당이 불가능하기 때문이다
그래서 `const로` 상수를 선언한다

참고로, 상수는 변수의 상대 개념이다
변수는 재할당이 가능하지만 상수는 불가능하다


코드 최상단에 사용하면 코드를 이해하기도, 유지보수 하기도 쉽다

```js
const PI=3.14;

let radius=5;

let area = PI * radius * radius;

```

위 예제에서 조금 더 정확히 구하고 싶을때, 상수 `PI` 값을 `3.1415926545`로 바꾸기만 하면 된다


## 15.3.4 const 키워드와 객체
`const` 키워드로 원시값을 할당한 경우 값을 변경할 수 없다.
`const` 키워드로 선언된 변수에 객체를 할당한 경우 *값을 변경할 수 있다!*

```js
//원시값 할당
const part="frontend";
part="backend" //변경 불가

//객체 할당
const club={
	name: 'Devocean'
}
club.name='Devocean Young' //변경 가능
```
# 질문

Q. const 키워드로 선언된 변수에 객체 할당한 경우, 값 변경이 가능한 원리가 궁금합니다.

Q. `const` 키워드로 선언된 변수에 객체를 할당한 경우, 왜 객체의 프로퍼티는 변경할 수 있는지 궁금해요.

A. `const`로 선언된 변수는 참조(주소) 자체를 변경할 수 없지만, 객체의 속성은 변경할 수 있다. 이는 `const`가 객체의 불변성을 보장하는 것이 아니라, 변수의 참조를 고정하기 때문이다.

이는 `const`가 변수의 참조를 고정하기 때문이지, 객체 내부의 
![](https://i.imgur.com/N8A37g7.png)

![](https://i.imgur.com/MZzj6FH.png)
불변성을 보장하지 않기 때문이다.


let 이나 var로 선언된 변수는 값을 변경할때 참조하는 주소 자체가 바뀌게 된다

대신 const는 참조하는 주소 자체는 고정이고, 그 안의 값이 객체인 경우에만 바뀔 수 있는 것이다


```jsx
const club = {
  name: 'Devocean'
};
// club 변수가 가리키는 메모리 주소는 0x1234라고 가정

club.name = 'Devocean Young';
// club.name을 변경하는 것은 메모리 주소 0x1234 내의 데이터를 변경하는 것

// 만약 club을 다른 객체로 재할당하려고 하면:
club = { name: 'New Club' };
// 이는 club 변수가 새로운 메모리 주소를 가리키도록 하려는 시도이므로, 에러가 발생함
```


![](https://i.imgur.com/vdF1yB5.png)


```jsx
var club2 = {
  name: 'Devocean'
};
// club 변수가 가리키는 메모리 주소는 0x1234라고 가정

club2.name = 'Devocean Young';
// club.name을 변경하는 것은 메모리 주소 0x1234 내의 데이터를 변경하는 것
console.log(club2.name);

// 만약 club을 다른 객체로 재할당하려고 하면:
club = { name: 'New Club' };
// 이는 club 변수가 새로운 메모리 주소를 가리키도록 하려는 시도인데, var라서 재할당이 가능
console.log(club2.name);
```

![](https://i.imgur.com/FGQKCuP.png)



⇒ useState도 이 원리를 사용한다


# 15.4 var vs let vs const

`var` 은 되도록 사용하지 않는다
`let` 을 사용하게 되는 경우에는, 변수 스코프를 최대한 좁게 만든다
객체는 의외로 재할당 하는 경우가 드물다, 따라서 변수를 선언할때는 일단 `const` 키워드를 사용해라.

>정리
>변수 선언에는 기본적으로` const`를 사용하고, 재할당이 필요한 경우에만 `let`을 한정하여 사용하는 것이 좋다



