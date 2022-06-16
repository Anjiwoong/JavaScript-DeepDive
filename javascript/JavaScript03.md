# 스코프

- ### 스코프는 자바스크립트를 포함한 모든 프로드래밍 언어의 기본적이며 중요한 개념이다.

var 키워드로 선언한 변수와 let 또는 const 키워드로 선언한 변수의 스코프도 다르게 동작한다.<br/>

- 함수의 매개변수는 함수 몸체 내부에서만 참조할 수 있고 함수 몸체 외부에서는 참조할 수 없다고 했다. 이것은 매개변수를 참조할 수 있는 유효범위, 즉 매개변수의 스코프가 함수몸체 내부로 한정되기 때문이다.

```javascript
function add0(x, y) {
  // 매개변수는 함수 몸체 내부에서만 참조할 수 있다.
  // 즉, 매개변수의 스코프(유효범위)는 함수 몸체 내부다.
  console.log(x, y);
  return x + y;
}

add(2, 5);

// 매개변수는 함수 몸체 내부에서만 참조할 수 있다.
console.log(x, y); // ReferenceError: x is not defined
```

- 변수는 코드의 가장 바깥 영역뿐 아니라 코드 블록이나 함수 몸체 내에서도 선언할 수 있다. 이때 코드 블록이나 함수는 중첩될 수 있다.

- 변수는 자신이 선언된 위치에 의해 자신이 유효한 범위, 즉 다른 코드가 변수 자신을 참조할 수 있는 범위가 결정된다.

- #### 모든 식별자는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정된다. 이를 스코프라 한다. 즉, 스코프는 식별자가 유효한 범위를 말한다.

```javascript
var x = "global";

function foo() {
  var x = "local";
  console.log(x); // 1️⃣
}

foo();

console.log(x); // 2️⃣
```

이름이 같은 두 개의 변수 중에서 어떤 변수를 참조해야 할 것인지를 결정하는 것을 **식별자 결정**이라 한다.<br/>
스코프를 통해 어떤 변수를 참조해야 할 것인지 결정한다.<br/>
**스코프**란 자바스크립트 엔진이 식별자를 검색할 때 사용하는 규칙이라고 할 수 있다.
만약 스코프라는 개념이 없다면 같은 이름을 갖는 변수는 충돌을 일으키므로 프로그램 전체에서 하나밖에 사용할 수 없다.

- 스코프 내에서 식별자는 유일해야 하지만 다른 스코프에는 같은 이름의 식별자를 사용할 수 있다.

## 스코프의 종류

코드는 전역과 지역으로 구분할 수 있다.

| 구분 | 설명                 | 스코프      | 변수     |
| ---- | -------------------- | ----------- | -------- |
| 전역 | 코드의 가장 바깥영역 | 전역 스코프 | 지역변수 |
| 지역 | 함수 몸체 내부       | 지역 스코프 | 지역변수 |

- 변수는 자신이 선언된 위치(전역 또는 지역)에 의해 자신이 유효한 범위인 스코프가 결정된다.

- ### 전역과 전역 스코프

전역이란 코드의 가장 바깥 영역을 말한다. 전역에 변수를 선언하면 전역스코프를 갖는 전역 변수가 된다.<br/>
**전역 변수는 어디서든지 참조할 수 있다.**

- ### 지역과 지역 스코프

지역이란 함수 몸체내부를 말한다. 지역변수는 자신의 지역 스코프와 하위 지역 스코프에서 유효하다.

## 스코프 체인

함수는 전역에서 정의할 수도 있고 함수 몸체 내부에서 정의할 수도 있다. <br />

- 함수의 중첩 : 함수 몸체 내부에서 함수가 정의된 것
- 중첩 함수 : 함수 몸체 내부에서 정의한 함수
- 외부 함수 : 중첩 함수를 포함하는 함수

함수는 중첩될수 있으므로 함수의 지역 스코프도 중첩될 수 있다. 즉, **스코프가 함수의 중첩에 의해 계층적 구조를 갖는다**

- ### 스코프 체인

![스코프체인](/javascript/images/ScopeChain.png);

- 스코프 체인은 최상위 스코프인 전역 스코프, 전역에서 선언된 outer 함수의 지역 스코프, outer 함수 내부에서 선언된 inner 함수의 지역 스코프로 이뤄진다.

- 변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 선언된 변수를 검색한다.

- ### 스코프 체인에 의한 변수 검색

자바스크립트 엔진은 스코프 체인을 따라 변수를 참조하는 코드의 스코프에서 시작해서 상위 방향으로 이동하며 선언된 변수를 검색한다.<br />
절대 하위 스코프로 내려가면서 식별자를 검색하는 일은 없다.

- 상위 스코프에서 유요한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만 하위 스코프에서 유혀한 변수를 상위 스코프에서 참조할 수 없다는 것.

## 함수 레벨 스코프

코드 블럭이 아닌 함수에 의해서만 지역 스코프가 생성된다.

- 함수 몸체만이 아니라 모든 코드 블록이 지역 스코프를 만든다 이러한 특성을 **블록 레벨 스코프**라 한다.
- var키워드로 선언된 변수는 오로지 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정한다. 이러한 특성을 **함수 레벨 스코프** 라 한다.

```javascript
var x = 1;
if (true) {
  var x = 10;
}
console.log(x); // 10
```

var 키워드로 선언된 변수는 함수 레벨 스코프만 인정하기 때문에 함수 밖에서 var 키워드로 선언된 변수는
코드 블록 내에서 선언되었다 할지라도 모두 전역 변수다.
따라서 의도치 않은 전역 변수의 값이 재할당된다.

- var 키워드로 선언된 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정하지만 let, const 키워드는 블록 레벨 스코프를 지원한다.

## 렉시컬 스코프

- 동적 스코프 : 함수를 정의하는 시점에는 함수가 어디서 호출될지 알 수 없다. 따라서 함수가 호출되는 시점에 동적으로 상위 스코프를 결정해야 하기 때문에 동적 스코프라 부른다.

- 렉시컬 스코프, 정적 스코프 : 동적 스코프 방식처럼 상위 스코프가 동적으로 변하지 않고 함수 정의가 평가되는 시점에 상위 스코프가 정적으로 결정되기 때문에 정적 스코프라 부른다.<br />
  대부분의 프로그래밍 언어는 렉시컬 스코프를 따른다.

- #### 자바스크립트는 렉시컬 스코프를 따르므로 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다.

함수가 호출된 위치는 상위 스코프 결정에 어떠한 영향도 주지 않는다.
즉 함수의 상위 스코프는 언제나 자신이 정의된 스코프다.

---

# 전역 변수의 문제점

전역 변수의 무분별한 사용은 위험하다. 반드시 사용해야 할 이유를 찾지 못한다면 지역 변수를 사용해야 한다.

## 변수의 생명 주기

- ### 지역 변수의 생명 주기

변수는 생물과 유사하게 생성되고 소명되는 생명 주기가 있다. 변수에 생명 주기가 없다면 한번 선언된 변수는 프로그램을 종료하지 않는 한 영원히 메모리 공간을 점유하게 된다.
<br />

```javascript
function foo() {
  var x = "local";
  console.log(x); // local
  return x;
}

foo();
console.log(x); // ReferenceError: x is not defined
```

지역변수 x는 foo 함수가 호출되기 이전까지는 생성되지 않는다.<br />
foo 함수를 호출하지 않으면 함수 내부의 변수 선언문이 실행되지 않기 때문이다.

- 함수 내부에서 선언된 지역 변수 x는 foo 함수가 호출되어 실행되는 동안에는 유효하다. 즉, 지역 변수의 생명 주기는 함수의 생명 주기와 일치한다.

- 변수는 자신이 등록된 스코프가 소멸(메모리 해제)될 때 까지 유효하다. 즉, 누군가가 메모리 공간을 참조하고 있으면 해제되지 않고 확보된 상태로 남아있게 된다. 이는 스코프도 같다.

- 일반적으로 함수가 종료되면 함수가 생성한 스코프도 소멸한다. 하지만 누군가가 스코프를 참조하고 있다면 스코프는 해제되지 않고 생존하게 된다.

#### 호이스팅은 스코프를 단위로 동작한다.

- 전역 변수의 호이스팅은 전역 변수의 선언이 전역 스코프의 선두로 끌어 올려진 것처럼 동작한다. 전역 변수는 전역 전체에서 유효하다.
- 지역 변수의 호이스팅은 지역 변수의 선언이 지역 스코프의 선두로 끌어 올려진 것처럼 동작한다. 지역 변수는 함수 전체에서 유효하다.

즉, 호이스팅은 변수 선언이 스코프의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 말한다.

- ### 전역 변수의 생명 주기

전역 코드는 함수 호출과 같이 전역 코드를 실행하는 특별한 진입점이 없고 코드가 로드되자마자 곧바로 해석되고 실행된다.<br />
전역 코드에는 반환문을 사용할 수 없으므로 마지막 문이 실행되어 더 이상 실행할 문이 없을 때 종료한다.
<br />
var 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 된다. 이는 전역 변수의 생명 주기가 전역 객체의 생명 주기와 일치한다는 것을 말한다.

- 브라우저 환경에서 전역 객체는 window이므로 브라우저 환경에서 var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
- 브라우저 환경에서 var 키워드로 선언한 전역 변수는 웹페이지를 닫을 때까지 유효하다. 즉, var 키워드로 선언한 전역 변수의 생명 주기는 전역 객체의 생명 주기와 일치한다.

## 전역 변수의 문제점

- 암묵적 결합

  - 전역 변수를 선언한 의도는 코드 어디서든 참조하고 할당할 수 있는 변수를 사용하겠다는 것이다.
    이는 모든 코드가 전역 변수를 참조하고 변경할수 있는 **암묵적 결합**을 호용하는 것이다.<br />
    변수의 유효 범위가 크면 클수록 코드의 가독성이 나빠지고 의도치 않게 상태가 변경될 수 있는 위험성도 높아진다.

- 긴 생명 주기

  - 전역 변수는 생명 주기가 길다. 따라서 메모리 리소스도 오랜 기간 소비한다.
  - var 키워드는 변수의 중복 선언을 허용하므로 생명 주기가 긴 전역 변수는 변수 이름이 중복될 가능성이 있다.
  - 지역변수는 전역 변수보다 생명 주기가 훨씬 짧다. 따라서 지역 변수의 상태를 변경할 수 있는 시간도 짧고 기회도 적다. 이는 전역 변수보다 상태 변경에 의한 오류가 발생할 확률이 작다는 것을 의미한다.

- 스코프 체인 상에서 종점에 존재

  - 전역 변수는 스코프 체인 상에서 종점에 존재한다.
  - 변수를 검색할 때 전역 변수가 가장 마지막에 검색된다. 즉, 전역 변수의 검색 속도가 가장 느리다.

- 네임스페이스 오염
  - 자바스크립트의 가장 큰 문제점 중 하나는 파일이 분리되어 있다 해도 하나의 전역 스코프를 공유한다는 것
  - 다른 파일 내에서 동일한 이름으로 명명된 전역 변수나 전역 함수가 같은 스코프 내에 존재할 경우 예상치 못한 결과를 가져올 수 있다.

## 전역 변수의 사용을 억제하는 방법

**변수의 스코프는 좁을수록 좋다**
<br />
전역 변수를 절대 사용하지 말라는 의미가 아니다. 전역변수 남발은 억제해야 한다는 것이다.

- ### 즉시 실행 함수

함수 정의와 동시에 호출되는 즉시 실행 함수는 단 한 번만 호출된다.<br />
모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.

```javascript
(function () {
  var foo = 10; // 즉시 실행 함수의 지역 변수
})();
console.log(foo);
```

- ### 네임스페이스 객체

전역에 네임스페이스 역할을 담당할 객체를 생성하고 전역 변수처럼 사용하고 싶은 변수를 프로퍼티로 추가하는 방법

```javascript
var = MYAPP = {}; // 전역 네임스페이스 객체

MYAPP.person = {
  name: 'An',
  address: 'Seoul'
};

console.log(MYAPP.person.name) // An
```

- 네임스페이스를 분리해서 식별자 충돌을 방지하는 효과는 있으나 네임스페이스 객체 자체가 전역 변수에 할당되므로 그다지 유용하지 않다.

- ### 모듈 패턴

클래스를 모방해서 관련이 있는 변수와 함수를 모아 즉시 실햄 함수로 감싸 하나의 모듈을 만든다.

- 클로저 기반으로 작동한다.
- 특징은 전역 변수의 억제는 물론 캡슐화까지 구현할 수 있다.

**캡슐화**는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.

- 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 **정보은닉** 이라 한다.
- 자바스크립트는 public, private, protected 등의 접근 제한자를 제공하지 않는다.
- 전역 네임스페이스의 오염을 막는 기능은 물론 한정적이기는 하지만 정보 은닉을 구현하기 위해 사용한다.

- ### ES6 모듈

es6 모듈을 사용하면 더는 전역 변수를 사용할 수 없다. <br />
es6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공한다.

- var 키워드로 선언한 변수는 더는 전역 변수가 아니며 window 객체의 프로퍼티도 아니다.

모던브라우저에서는 ES6 모듈을 사용할 수 있다. script 태그에 type="module" attribute 를 추가하면 로드된 자바스크립트 파일은 모듈로서 동작한다.

```html
<script type="module" src="lib.mjs"></script>
<script type="module" src="app.mjs"></script>
```

- IE를 포함한 구형 브라우저에서는 동작하지 않으며, 사용하더라도 트랜스파일링이나 번들링이 필요하기 때문에 아직까지는 Webpack 등의 모듈 번들러를 사용하는 것이 일반적이다.

---

# let, const 키워드와 블록 레벨 스코프

## var 키워드로 선언한 변수의 문제점

ES5까지 변수를 선언할 수 있는 유일한 방법은 var 키워드를 사용하는 것이었다.<br />
이는 다른 언어와는 구별되는 독특한 특징으로, 주의를 기울이지 않으면 심각한 문제를 발생시킬 수 있다.

- ### 변수 중복 선언 허용

var 키워드로 선언한 변수는 중복 선언이 가능하다.

```javascript
var = x = 1;
var = y = 1;

var = x = 100;

var y;

console.log(x); // 100
console.log(y); // 1
```

var 키워드로 선언한 변수를 중복 선언하면 초기화문 유무에 따라 다르게 동작한다.

- 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작하고 초기화문이 없는 변수 선언문은 무시된다. 이때 에러는 발생하지 않는다.
- 위 예제처럼 변수를 중복 선언하면서 값까지 할당했다면 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용이 발생한다.

- ### 함수 레벨 스코프

var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다. <br />
따라서 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.

- ### 변수 호이스팅

var 키워드로 변수를 선언하면 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다.

- 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다.
- 단, 할당문 이전에 변수를 참조하면 언제나 undefined를 반환한다.

## let 키워드

var 키워드의 단점을 보완하기 위해 ES6에서는 새로운 변수 선언 키워드인 let 과 const 를 도입했다.

- ### 변수 중복 선언 금지

let 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러가 발생한다.

```javascript
var foo = 123;
var foo = 456;

let bar = 123;
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

- ### 블록 레벨 스코프

let 키워드로 선언한 변수는 모든 코드블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

- ### 변수 호이스팅

let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

- let 키워드로 선언한 변수를 변수 선언문 이전에 참조하면 참조 에러(ReferenceError)가 발생한다.

let 키워드로 선언한 변수는 "선언 단계"와 "초기화 단계"가 분리되어 진행된다. 즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다.

- 선언한 변수는 스코프의 시작 지점부터 초기화 단계 시작 지점까지 변수를 참조할 수 없다.
- 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 일시적 사각지대 Temporal Dead Zone: TDZ라고 부른다.

![TDZ](/javascript/images/TDZ.jpeg);

결국 let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 보인다. 하지만 그렇지 않다.

```javascript
let foo = 1; // 전역 변수
{
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2; // 지역 변수
}
```

let 키워드로 선언한 변수가 호이스팅이 발생하지 않는다면 위 예제는 전역 변수 foo 의 값을 출력해야한다.
하지만 여전히 호이스팅이 발생하기 때문에 참조에러가 발생한다.

모든 선언을 호이스팅한다. 하지만 let, const, class를 사용한 선언문은 호이스팅이 발생하지 않는 것처럼 동작한다.

- ### 전역 객체와 let

var 키워드로 선언한 전역 변수와 전역 함수, 그리고 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역 객체 window의 프로퍼티가 된다.
전역 객체의 프로퍼티를 참조할 때 window를 생략할 수 있다.

- let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
- let 전역 변수는 보이지 않는 개념적인 블록내에 존재하게 된다.

## const 키워드

const 키워드는 상수를 선언하기 위해 사용한다. 하지만 반드시 상수만을 위해 사용하지는 않는다.

- ### 선언과 초기화

**const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.**

- const 키워드로 선언한 변수는 let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

- ### 재할당 금지

var 또는 let 키워드로 선언한 변수는 재할당이 자유로우나 const 키워드로 선언한 변수는 재할당이 금지된다.

- ### 상수

const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다. 이러한 특징을 이용해 const 키워드를 상수를 표현하는데 사용하기도 한다.
<br/>
변수의 상대 개념인 상수는 재할당이 금지된 변수를 말한다.

- 상수는 상태 유지와 가독성, 유지보수의 편의를 위해 적극적으로 사용해야 한다.
- const 키워드로 선언된 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값이고 const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없다.
- 일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 나타낸다. 여러 단어로 이뤄진 경우에는 \_ 로 구분해서 스네이크 케이스로 표현하는것이 일반적이다.

- ### const 키워드와 객체
  const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.
- 변경 불가능한 값인 원시 값은 재할당 없이 변경할 수 있는 방법이 없지만 변경 가능한 값인 객체는 재할당 없이도 직접 변경이 가능하기 때문이다.

```javascript
const person = {
  name: "An",
};

// 객체는 변경 가능한 값이다. 따라서 재할당 없이 변경이 가능하다.
person.name = "Kim";
console.log(person); // {name: 'Kim'}
```

const 키워드는 재할당을 금지할 뿐 "불변"을 의미하지 않는다.

- 새로운 값을 재할당하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능하다.
- 객체가 변경되더라도 변수에 할당된 참조 값은 변경되지 않는다.

- ### var vs. let vs. const
  변수 선언에는 기본적으로 const를 사용하고 let은 재할당이 필요한 경우에 한정해 사용하는 것이 좋다.
  <br/>
  const 키워드를 사용하면 의도치 않은 재할당을 방지하기 때문에 좀 더 안전하다.
- ES6를 사용한다면 var 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 let 키워드를 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지않는 원시 값과 객체에는 const 키워드를 사용한다. 재할당을 금지하므로 var, let 키워드보다 안전하다.