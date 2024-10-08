스코프(scope, 유효범위)는 자바스크립트를 포함한 모든 프로그래밍 언어의 기본적이며 중요한 개념이다.
스코프는 함수와 변수에 연관이 깊어서 매우 중요한 개념이니 잘 이해해야한다.
타 언어와 다른 자바스크립트 스코프만의 특징이 존재함으로 이 부분도 주의 깊게 살펴봐야한다.

## 스코프

스코프는 식별자가 유효한 범위를 말한다. 모든 식별자는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효범위(스코프)가 정해진다.

대표적인 예시로, 함수의 매개변수가 있다. 매개변수는 함수 몸체 내부로 범위가 한정되며 외부에서는 사용할 수 없다.

```javascript
var x = "global";

function foo() {
  var x = "local";
  console.log(x); // 1. local
}

foo();

console.log(x); //2. global
```

그런데, 위 예시처럼 x가 여러 개라면 어떤 x가 출력될까? (어떤 x를 참조해야할까?) 이를 자바스크립트 엔진이 결정하는 과정을 식별자 결정이라 한다. 그리고 자바스크립트 엔진이 식별자 결정 시 사용하는 규칙이 스코프라 할 수 있다.

위 예제의 변수 x는 동일한 식별자를 사용하지만, 스코프(유효범위)가 다르다. 최상단 x는 전역변수이고 함수 내부 x는 함수 변수이다. 변수는 식별할 수 있어야하므로, 동일 스코프 내에서 식별자는 유일(unique)하다. 따라서, 스코프는 네임스페이스의 역할을 한다.

※ 코드가 어디서 실행되고, 주변에 어떤 코드가 있는 지를 렉시컬 환경이라 부른다.

※ 실행 컨텍스트는 렉시컬 환경을 구현한 것으로 모든 코드는 실행 컨텍스트에서 평가되고 실행된다.

## 스코프의 종류

코드는 전역(global)과 지역(local)로 구분할 수 있다.
이때, 변수는 자신이 선언된 위치에 의해서 스코프가 결정된다.

### 전역과 전역 스코프

전역이란 코드의 가장 바깥 영역을 말한다. 전역은 전역 스코프를 만든다.
전역 변수는 어디서든지 참조할 수 있다.

### 지역과 지역 스코프

지역이란 함수 몸체 내부를 말한다. 지역은 지역 스코프를 만든다.
지역 변수는 자신의 지역 스코프와 하위 지역 스코프에서 유효하다.

```javascript
var x = "global x"; // global

var y = "global y";

function outer() {
  // local_1
  var z = "outer local z";
  console.log(x); // global x
  console.log(y); // global y
  console.log(z); // outer local z

  function inner() {
    // local_2
    var x = "inner local x";
    console.log(x); // global x
    console.log(y); // global y
    console.log(z); // outer local z
  }

  inner();
}
outer();
```

위 예시처럼 전역 변수는 어디서나 참조할 수 있다.

하지만, 지역 변수는 자신의 지역과 자신의 하위 지역 스코프에서만 유효하다.

전역은 어디서나 참조할 수 있다면, 같은 네미잉의 로컬 변수를 참조할 지 전역 변수를 참조할 지 어떻게 알 수 있을까?

이는 자바스크립트 엔진의 스코프 체인을 통해서 참조할 변수를 검색(identifier resolustion)했기 때문이다.

## 스코프 체인

함수 몸체 내부에 함수가 정의되면 이를 중첩 함수라 부른다. 함수가 중첩되듯이, 스코프 또한 중첩에 의해서 계층 구조를 갖는다. 외부함수일수록, 상위 스코프라 한다.

바로 위 예시를 다시 사용하면 local_2 < local_1 < global 순으로 상위 스코프가 형성된다. 이렇게 계층적으로 연결된 것을 스코프 체인이라 한다.

자바스크립트 엔진은 스코프 체인을 통해서 참조하는 코드의 스코프에서 시작하여 상위 스코프로 이동하며 선언된 변수를 검색한다.

스코프 체인은 물리적 실체로 존재한다. 자바스크립트 엔진은 코드를 실행하기에 앞서서 이와 유사한 자료구조인 렉시컬 환경을 실제로 생성한다. 변수 선언이 실행되면 변수 식별자가 이 자료구조에 키로 등록되고, 변수 할당이 일어나면 이 자료구조의 변수 식별자에 해당하는 값을 변경한다. 변수의 검색도 이 자료구조 상에서 이뤄진다.

## 함수 레벨 스코프

지역은 함수 몸체 내부를 말하고 지역은 지역 스코프를 만든다. 이는 코드 블록이 아닌 함수에 의해서 지역스코프가 생성된다는 의미다.

c나 자바 등을 비롯한 대부분의 프로그래밍 언어는 함수 몸체만이 아니라 모든 코드 블록(if, for, while)이 지역 스코프를 만든다. 이런 특성을 블록 레벨 스코프(block level scope)라 한다. 하지만 var 키워드로 선언된 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다. 이러한 특성을 함수 레벨 스코프(function level scope)라 한다.

```javascript
var i = 10;

for (var i = 0; i < 5; i++) {
  console.log(i);
}

console.log(i); // 5
```

함수만을 스코프로 하기 때문에 for문 내부에 i가 의도치 않게 재선언 및 재할당 되었다.

블록 레벨 스코프를 지원하는 프로그래밍 언어에서는 for문이 별도의 스코프기 때문에 이런 중복이 일어나지 않는다.

## 렉시컬 스코프

```javascript
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); //1
bar(); //1
```

위 예제의 결과를 생각해보자.

상위 스코프에 따라서 실행결과가 달라지면 크게 두 가지 패턴이 있다.

1. 함수를 어디서 호출했는지에 따라 함수의 상위 스코프를 결정한다.

2. 함수를 어디서 정의했는지에 따라 함수의 상위 스코프를 결정한다.

프로그래밍 언어는 흔히 둘 중 하나의 패턴을 선택한다.

1번은 동적 스코프라 하며, 호출되는 시점에 상위 스코프가 결정되기에 동적스코프라 부른다.

2번은 렉시컬 스코프 또는 정적 스코프라 한다. 동적 스코프 방식처럼 상위 스코프가 동적으로 변하지 않고 함수 정의가 평가되는 시점에 상위 스코프가 정적으로 결정되기 때문이다.

자바스크립트는 렉시컬 스코프이다. 그래서 함수 정의가 실행되어 생성된 함수 객체는 이렇게 결정된 상위 스코프를 기억한다. 함수가 호출될 때마다 함수의 상위 스코프를 참조할 필요가 있기 때문이다.

이 개념은 클로저와 관계가 깊으니 추후 다시 클로저를 다루도록 하자.
