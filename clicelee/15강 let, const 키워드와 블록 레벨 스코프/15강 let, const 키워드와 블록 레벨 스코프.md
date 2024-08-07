
![](https://i.imgur.com/8Ccqy2i.png)
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

# 15.4 var vs let vs const

`var` 은 되도록 사용하지 않는다
`let` 을 사용하게 되는 경우에는, 변수 스코프를 최대한 좁게 만든다
객체는 의외로 재할당 하는 경우가 드물다, 따라서 변수를 선언할때는 일단 `const` 키워드를 사용해라.

>정리
>변수 선언에는 기본적으로` const`를 사용하고, 재할당이 필요한 경우에만 `let`을 한정하여 사용하는 것이 좋다

