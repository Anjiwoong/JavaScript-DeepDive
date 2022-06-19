# 프로토타입

자바스크립트는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어다.

- 클래스 기반 객체지향 프로그래밍 언어보다 효율적이며 더 강력한 객체지향 프로그래밍 능력을 지니고 있는 프로토타입 기반의 객체지향 프로그래밍 언어다.

## 객체지향 프로그래밍

프로그램을 명령어 또는 함수의 목록으로 보는 전통적인 명령형 프로그래밍의 절차지향적 관점에서 벗어나 여러 개의 독립적 단위, 즉 객체의 집합으로 츠로그램을 표현하려는 프로그래밍 패러다임을 말한다.

실세계의 실체를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작한다.
<br/>실체는 특징이나 성질을 나타내는 **속성**을 가지고 있고, 이를 통해 실체를 인식하거나 구별할 수 있다.

다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것을 **추상화**라 한다.
<br/>송성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 객체라 하며, 객체지향 프로그래밍은 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임이다.

객체는 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조라고 할수 있다. 이때 객체의 상태 데이터를 **프로퍼티**, 동작을 **메서드**라 부른다.

## 상속과 프로토타입

상속은 객체지향 프로그래밍의 핵심 개념으로, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.

자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다.

```javascript
// 생성자 함수
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

console.log(circle1.getArea === circle2.getArea); // false
```

생성자 함수는 동일한 프로퍼티 구조를 갖는 객체를 여러 개 생성할 때 유용하다.
하지만 위 예제의 생성자 함수에는 문제가 있다.

생성자 함수에 의해 생성된 모든 인스턴스가 동일한 메서드를 중복 소유하는 것은 메모리를 불필효하게 낭비한다. 또한 인스턴스를 생성할 때마다 메서드를 생성하므로 퍼포먼스에도 악영향을 준다.

상속을 통해 불필요한 중복을 제거해 보자.

**자바스크립트는 프로토타입을 기반으로 상속을 구현한다.**

```javascript
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // true
```

Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속받는다.

생성자 함수가 생성할 때 모든 인스턴스가 공통적으로 사용할 프로퍼티나 메서드를 프로토타입에 미리 구현해 두면 생성자 함수가 생성할 모든 인스턴스는 별도의 구현없이 상위 객체인 프로토타입의 자산을 공유하여 사용할 수 있다.

## 프로토타입 객체

객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용된다. 프로토타입을 상속받은 하위 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.

모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며 이 내부 슬롯의 값을 프로토타입의 참조다.
[[Prototype]]에 저장되는 프로토타입은 객체 생성 방식에 의해 결정된다.

- 객체의 프로토타입은 Object.prototype
- 생성자 함수에 의해 생성된 개게의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.
- 모든 객체는 하나의 프로토타입을 갖는다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있다.

\_ _ proto _ \_ 접근자 프로퍼티를 통해 자신의 프로토타입에 간접적으로 접근할 수 있고 프로토타입은 자신의 constructor 프로퍼티를 통해 프로토타입에 접근할 수 있다.

- ### \_ _ proto _ \_ 접근자 프로퍼티

모든 객체는 \_ _ proto _ \_ 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.

##### \_ _ proto _ \_는 접근자 프로퍼티다.

내부 슬롯은 프로퍼티가 아니기 때문에 [[Prototype]] 내부 슬롯에도 직접 접근할 수 없으며 \_ _ proto _ \_접근자 프로퍼티를 통해 간접적으로 [[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근할 수 있다.

- [[Get]], [[Set]] 프로퍼티 어트리뷰트로 구성된 프로퍼티다.

\_ _ proto _ \_접근자 프로퍼티를 통해 프로토타입에 접근하면 내부적으로 \_ _ proto _ \_접근자 프로퍼티의 getter함수인 [[Get]]이 호출된다.

\_ _ proto _ \_접근자 프로퍼티를 통해 새로운 프로토타입을 할당하면 \_ _ proto _ \_접근자 프로퍼티의 setter 함수인 [[Set]]이 호출된다.

##### \_ _ proto _ \_접근자 프로퍼티는 상속을 통해 사용된다.

\_ _ proto _ \_접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티다.
모든 객체는 상속을 통해 Object.prototype.\_ _ proto _ \_접근자 프로퍼티를 사용할 수 있다.

##### \_ _ proto _ \_접근자 프로퍼티를 통해 프로토타입에 접근하는 이유

상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서다.

```javascript
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent 프로토타입을 child로 설정
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

프로토타입 체인은 단반향 링크드 리스트로 구형되어야한다. 위 예제와 같이 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인(순환 참조)하는 프로토타입 체인이 만들어지면 프로토타입 체인 종점이 존재하지 않기 때문에 무한 루프에 빠진다.
<br/> 따라서 \_ _ proto _ \_접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현되어 있다.

##### \_ _ proto _ \_접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.

\_ _ proto _ \_접근자 프로퍼티는 ES5까지 ECMAScript 사양에 포함되지 않는 비표준이였지만 일부 브라우저에서 \_ _ proto _ \_를 지원하고 있었기 때문에 브라우저 호환성을 고려하여 ES6에서 \_ _ proto _ \_를 표준으로 채택했다.

하지만 코드 내에서 \_ _ proto _ \_접근자 프로퍼티를 직접 사용하는 것은 권장하지 않는다.

- 모든 객체가 \_ _ proto _ \_접근자 프로퍼티를 사용할 수 있는 것이 아니기 때문

직접 상속을 통해 Object.prototype을 상속받지 않는 객체를 생성할 수도 있기 때문에 \_ _ proto _ \_접근자 프로퍼티를 사용할 수 없는 경우가 있다.

- \_ _ proto _ \_접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶은 경우에는 Object.getPrototypeOf 메서드를 사용
- 프로토타입을 교체하고 싶은 경우에는 Object.setProtoTypeOf 메서드를 사용할 것을 권장

- ### 함수 객체의 prototype 프로퍼티
  함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

생성자 함수로서 호출할 수 없는 함수, 즉 non-constructor인 화살표 함수와 ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.

모든 객체가 가지고 있는 \_ _ proto _ \_접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.
하지만 이들 프로퍼티를 사용하는 주체가 다르다

|              구분               |    소유     |        값         |  사용 주체  |                             사용 목적                              |
| :-----------------------------: | :---------: | :---------------: | :---------: | :----------------------------------------------------------------: |
| \_ _ proto _ \_ 접근자 프로퍼티 |  모든 객체  | 프로토타입의 참조 |  모든 객체  |      객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용       |
|       prototype 프로퍼티        | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체의 프로토타입을 할당하기 위해 사용 |

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// 결국 Person.prototype과 me.__proto__는 결국 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__); // true
```

- ### 프로토타입의 constructor 프로퍼티와 생성자 함수
  모든 프로토타입은 constructor 프로퍼티를 갖는다. 이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person); // true
```

## 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결된다. 이때 constructor 프로퍼티가 가리키는 생성자 함수는 인스턴스를 생성한 생성자 함수다.

리터럴 표기법에 의해 객체 생성 방식과 같이 명시적으로 new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 있다.

리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없다.

```javascript
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다
const obj = {};

// 하지만 obj객체의 생성자 함수는 Object 생성자 함수디.
console.log(obj.constructor === Object); // true
```

Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null을 인수로 전달하면서 호출하면 내부적으로 추상 연산 OrdinaryObjectCreate를 호출하여 Object.prototype을 프로토타입으로 갖는 빈 객체를 생성한다.

- 객체 리터럴이 평가될 때는 추상 연산 OrdinaryObjectCreate를 호출하여 빈 객체를 생성하고 프로퍼티를 추가하도록 정의되어 있다.
- 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다.

함수 선언문과 함수 표현식을 평가하여 함수 객체를 생성한 것은 Function 생성자 함수가 아니다. 하지만 constructor 프로퍼티를 통해 확인해 보면 foo 함수의 생성자 함수는 Function 생성자 함수다.

```javascript
// foo 함수는 Function 생성자 함수로 생성한 함수 객체가 아니라 함수 선언문으로 생성했다.
function foo() {}

// 하지만 constructor 프로퍼티를 통해 확인해보면 함수 foo의 생성자 함수는 Function 생성자 함수다.
console.log(foo.constructor === Function); // true
```

**프로토타입과 생성자 함수는 단독으로 존재할 수 없고 쌍으로 존재한다.**

프로토타입의 constructor 프로퍼티를 통해 연결되어 있는 생성자 함수를 리터럴 표기법으로 생성한 객체를 생성한 생성자 함수로 생각해도 크게 무리가 없다.

|   리터럴 표기법    | 생성자 함수 |     프로토타입     |
| :----------------: | :---------: | :----------------: |
|    객체 리터럴     |   Object    |  Object.prototype  |
|    함수 리터럴     |  Function   | Function.prototype |
|    배열 리터럴     |    Array    |  Array.prototype   |
| 정규 표현식 리터럴 |   RegExp    |  RegExp.prototype  |

## 프로토타입의 생성 시점

객체는 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 결국 모든 객체는 생성자 함수와 연결되어 있다.

프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다. 생성자 함수는 사용자가 직접 정의한 사용자 정의 생성자 함수와 자바스크립트가 기본 제공하는 빌트인 생성자 함수로 구분할 수 있다.

- ### 사용자 정의 생성자 함수와 프로토타입 생성 시점

생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

- 즉 non-constructor는 프로토타입이 생성되지 않는다.

```javascript
// 화살표 함수는 non-constructor다.
const Person = (name) => {
  this.name = name;
};

// non-constructor는 프로토타입이 생성되지 않는다.
console.log(Person.prototype); // undefined
```

함수 선언문으로 정의된 Person 생성자 함수는 어떤 코드보다 먼저 평가되어 함수 객체가 된다. 이때 프로토타입도 더불어 생성된다. 생성된 프로토타입은 Person 생성자 함수의 prototype에 바인딩 된다.

생성된 프로토타입은 오직 constructor 프로퍼티만 갖는 객체다. 프로토타입도 객체이고 모든 객체는 프로토타입을 가지므로 프로토타입도 자신의 프로토타입을 갖는다.
생성된 프로토타입의 프로토타입은 object.prototype이다.

- 생성된 프로토타입의 프로토타입은 언제나 Object.prototype이다.

- ### 빌트인 생성자 함수와 프로토타입 생성시점
  빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다. 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다.
  생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩 된다.

객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재한다. 이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당된다.

- 생성된 객체는 프로토타입을 상속받는다.

## 객체 생성 방식과 프로토타입의 결정

다양한 방식으로 생성된 모든 객체는 각 방식마다 세부적인 객체 생성 방식의 차이는 있으나 생성 방식의 차이는 있으나 추상 연산 OrdinaryObjectCreate에 의해 생성된다는 공통점이 있다.

- OrdinaryObjectCreate는 필수적으로 자신이 생성할 객체의 프로토타입을 인수로 전달 받는다.

- 자신이 생성할 객체에 추가할 프로퍼티 목록을 옵션으로 전달할 수 있다.

- 프로토타입은 추상 연산 OrdinaryObjectCreate에 전달되는 인수에 의해 결정된다.

- 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정된다.

- ### 객체 리터럴에 의해 생성된 객체의 프로토타입

자바스크립트는 객체 리터럴을 평가하여 객체를 생성할 때 추상 연산 OrdinaryObjectCreate를 호출한다. 이때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이다.

- 즉 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.

```javascript
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x')); // true
```

- obj 객체는 Object.prototype을 상속 받는다
- 자신의 프로토타입인 Object.prototype의 constructor 프로퍼티와 hasOwnProperty 메서드를 자신의 자산인 것처럼 자유롭게 사용할 수 있다.

- ### Object 생성자 함수에 의해 생성된 객체의 프로토타입

Object 생성자 함수를 인수없이 호출하면 빈 객체가 생성된다. Object 생성자 함수를 호출하면 객체 리터럴과 마찬가지로 추상 연산 OrdinaryObjectCreate가 호출된다.

- Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.
- 객체 리터럴에 의해 생성된 객체와 동일한 구조를 갖는다.

객체 리터럴과 Object 생성자 함수에 의한 객체 생성 방식의 차이는 프로퍼티를 추가하는 방식에 있다.

객체 리터럴 방식은 객체 리터럴 내부에 프로퍼티를 추가하지만 Object 생성자 함수 방식은 일단 빈 객체를 생성한 이후 프로퍼티를 추가해야 한다.

- ### 생성자 함수에 의해 생성된 객체의 프로토타입
  new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하면 다른 객체 생성 방식과 마찬가지로 추상 연산자 OrdinaryObjectCreate가 호출된다.

이때 OrdinaryObjectCreate에 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

- 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

```javascript
function Person(name) {
  this.name = name;
}

const me = new Person('An');
```

표준 빌트인 객체인 Object 생성자 함수와 더불어 생성된 프로토타입 Object.prototype은 다양한 빌트인 메서드를 갖고있다. 하지만 사용자 정의 생성자 함수 Person과 생성된 프로토타입 Person.prototype의 프로퍼티는 constructor뿐이다.

```javascript
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi My name is ${this.name}`);
};

const me = new Person('An');
const you = new Person('Kim');

me.sayHello(); // Hi My name is An
you.sayHello(); // Hi My name is Kim
```

Person 생성자 함수를 통해 생성된 모든 객체는 프로토타입에 추가된 sayHello 메서드를 상속받아 자신의 메서드처럼 사용할 수 있다.

## 프로토타입 체인

자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]]내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다.

이를 프로토타입 체인이라 한다.

- 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.

- 스코프 체인은 식별자 검색을 위한 메커니즘.

- 스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용된다.

```javascript
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi My name is ${this.name}`);
};

const me = new Person('An');

// hasOwnProperty는 Object.prototype의 메서드다.
console.log(me.hasOwnProperty('name')); // true
```

Person 생성자 함수에 의해 생성된 me 객체는 Object.prototype의 메서드인 hasOwnProperty를 호출할 수 있다.

- me 객체가 Person.prototype뿐만 아니라 Object.prototype도 상속받았다는 것을 의미한다.

- me 객체의 프로토타입은 Person.prototype이다.

- Person.prototype의 프로토타입은 Object.prototype이다.

- 프로토타입의 프로토타입은 언제나 Object.prototype이다.

- 프로토타입 체인의 최상위에 위치하는 객체는 언제나 Object.prototype이다. 따라서 모든 객체는 Object.prototype을 상속받는다.

- Object.prototype

  - 프로토타입 체인의 종점

- Object.prototype에서도 프로퍼티를 검색할 수 없는 경우 undefined를 반환한다.

## 오버라이딩과 프로퍼티 섀도잉

```javascript
const Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
})();

const me = new Person('An');

//인스턴스 메서드
me.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};

// 인스턴스 메서드가 호출된다. 프로토타입 메서드는 인스턴스 메서드에 의해 가려진다.
me.sayHello(); // Hey! My name is An
```

프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰는 것이 아니라 인스턴스 프로퍼티로 추가한다.

- 인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello를 오버라이딩했고 프로토타입 메서드 sayHello는 가려진다

- 상속 관계에 의해 프로퍼티가 가려지는 현상을 **프로퍼티 섀도잉**이라 한다.

```javascript
// 인스턴스 메서드를 삭제한다.
delete me.sayHello;

// 인스턴스에는 sayHello 메서드가 없으므로 프로토타입 메서드가 호출된다.
me.sayHello(); // Hi! My name is An
```

프로토타입 메서드가 아닌 인스턴스 메서드 sayHello가 삭제된다.

```javascript
// 프로토타입 체인을 통해 프로토타입 메서드가 삭제되지 않는다.
delete me.sayHello;
// 프로토타입 메서드가 호출된다.
me.sayHello(); // Hi! My name is An
```

하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제하는 것은 불가능하다.
하위 객체를 통해 프로토타입 get 액세스는 허용되나 set 액세스는 허용되지 않는다.

```javascript
// 프로토타입 메서드 변경
Person.prototype.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};
me.sayHello(); // Hey! My name is An

// 프로토타입 메서드 삭제
delete Person.prototype.sayHello;
me.sayHello(); // TypeError: me.sayHello is not a function
```

하위 객체를 통해 프로토타입 체인으로 접근하는 것이 아니라 프로토타입에 직접 접근해야 한다.

## 프로토타입의 교체

프로토타입은 임의의 다른 객체로 변경할 수 있다.

- 부모 객체인 프로토타입을 동적으로 변경할 수 있다.
- 프로토타입은 생성자 함수 또는 인스턴스에 의해 교체할 수 있다.

- ### 생성자 함수에 의한 프로토타입의 교체

```javascript
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    sayHello() {
      console.log(`Hi My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person('An');
```

- Person.prototype에 객체 리터럴로 할당했다.

프로토타입으로 교체한 객레 리터럴에는 constructor 프로퍼티가 없다. 따라서 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다.

이처럼 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.

프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가하여 프로토타입의 constructor 프로퍼티를 되살린다.

```javascript
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
    constructor: Person,
    sayHello() {
      console.log(`Hi My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person('An');

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false
```

- ### 인스턴스에 의한 프로토타입의 교체
  프로토타입은 생성자 함수의 prototype 프로퍼티뿐만 아니라 인스턴스의 \_ _ proto _ \_접근자 프로퍼티를 통해 접근할 수 있다.
- \_ _ proto _ \_접근자 프로퍼티를 통해 프로토타입을 교체할 수 있다.

```javascript
function Person(name) {
  this.name = name;
}
const me = new Person('An');

// 프로토타입으로 교체할 객체
const parent = {
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  },
};

// me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// me.__proto__ = parent

me.sayHello(); // Hi! My name is An
```

프로토타입으로 교체한 객체에는 constructor 프로퍼티가 없으므로 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다.

생성자 함수에 의한 프로토타입 교체와 인스턴스에 의한 프로토타입 교체는 별다른 차이가 없어보이지만 미묘한 차이가 있다.

- 생성자 함수에 의한 프로토타입 교체

  - Person 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리킨다.

- 인스턴스에 의한 프로토타입 교체
  - Person 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리키지 않는다.

프로토타입 교체를 통해 객체 간의 상속 관계를 동적으로 변경하는 것은 꽤나 번거롭다.
따라서 프로토타입은 직접 교체하지 않는 것이 좋다.

- 직접 상속이 더 편리하고 안전하다.

## instanceof 연산자

instanceof 연산자는 이항 연산자로서 좌변에 객체를 가리키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받는다. 만약 우변의 피연산자가 함수가 아닌 경우 TypeError가 발생한다.

```javascript
객체 instanceof 생성자 함수
```

우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우에는 false로 평가된다.

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('An');

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Person); // true

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true
```

instanceof 연산자는 프로토타입의 constructor 프로퍼티가 가리키는 생서자 함수를 찾는 것이 아니라 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.

- 생성자 함수에 의해 프로토타입이 교체되어 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴되어도 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결은 파괴되지 않으므로 instanceof는 아무런 영향을 받지 않는다.

## 직접 상속

- ### Object.create에 의한 직접 상속

  Object.create 메서드의 첫 번째 매개변수에는 생성할 객체의 프로토타입으로 지정할 객체를 전달한다. 두 번째 매개변수에는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달한다.

- 이 객체의 형식은 Object.defineProperties 메서드의 두 번째 인수와 동일하다.
- 두 번째 인수는 옵션이므로 생략 가능하다.

Object.create 메서드의 장점

- new 연산자가 없어도 객체를 생성할 수 있다.
- 프로토타입을 지정하면서 객체를 생성할 수 있다.
- 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.

Object.prototype의 빌트인 메서드인 Object.prototype.hasOwnProperty, Object.prototype.isPrototypeOf, Object.prototype.propertyIsEnumerable 등은 모든 객체의 프로토타입 체인의 종점, 즉 Object.prototype의 메서드이므로 모든 객체가 상속받아 호출할 수 있다.

Object.create메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할수 있기 때문에 ESLint에서는 Object.prototype의 빌트인 메서드를 객체가 직접 호출하는 것을 권장하지 않는다.

Object.prototype의 빌트인 메서드는 다음과 같이 간접적으로 호출하는 것이 좋다.

```javascript
// 프로토타입이 null인 객체를 생성한다.
const obj = Object.create(null);
obj.a = 1;

// console.log(obj.hasOwnProperty('a'));
// TypeError: obj.hasOwnProperty is not a function

// Object.prototype의 빌트인 메서드는 객체로 직접 호출하지 않는다.
console.log(Object.prototype.hasOwnProperty.call(obj, 'a'));
```

- ### 객체 리터럴 내부에서 \_ _ proto _ \_에 의한 직접 상속
  ES6에서는 객체 리터럴 내부에서 \_ _ proto _ \_접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있다.

```javascript
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
  y: 20,
  // 객체를 직접 상속받는다.
  // obj -> myProto -> Object.prototype -> null
  __proto__: myProto,
};

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

## 정적 프로퍼티 / 메서드

정적 프로퍼티 / 메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조 / 호출할 수 있는 프로퍼티 / 메서드를 말한다.

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = 'static prop';

// 정적 메서드
Person.staticMethod = function () {
  console.log('staticMethod');
};

const me = new Person('An');

// 생성자 함수에 추가한 정적 프로퍼티 / 메서드는 생성자 함수로 참조 / 호출한다.
Person.staticMethod(); // staticMethod

// 정적 프로퍼티 / 메서드는 생성자 함수가 생성한 인스턴스로 참조 / 호출할 수 없다.
// 인스턴스로 참조 / 호출할 수 있는 프로퍼티 / 메서드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

생성자 함수가 생성한 인스턴스는 자신의 프로토타입 체인에 속한 객체의 프로퍼티 / 메서드에 접근할 수 있다. 하지만 정적 프로퍼티 / 메서드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티 / 메서드가 아니므로 인스턴스로 접근할 수 없다.

만약 인스턴스 / 프로토타입 메서드 내에서 this를 사용하지 않는다면 그 메서드는 정적 메서드로 변경할 수 있다. 인스턴스가 호출한 인스턴스 / 프로토타입 메서드 내에서 this는 인스턴스를 가리킨다.

메서드 내에서 인스턴스를 참조할 필요가 없다면 정적 메서드로 변경하여도 동작한다. 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 하지만 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있다.

- MDN같은 문서를 보면 표기법만으로도 정적 프로퍼티 / 메서드와 프로토타입 프로퍼티 / 메서드를 구별할 수 있어야 한다.
- 프로토타입 프로퍼티 / 메서드를 표기할 때 prototype을 #으로 표기하는 경우도 있으니 알아두도록 하자.

## 프로퍼티 존재 확인

- ### in 연산자

in 연산자는 확인 대상 객체의 프로퍼티뿐만 아니라 확인 대상 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인하므로 주의가 필요하다.

```javascript
const person = {
  name: 'An',
  address: 'Seoul',
};

console.log('toString' in person); // true
```

in 연산자가 person 객체가 속한 프로토타입 체인 상에 존재하는 모든 프로토타입에서 toString 프로퍼티를 검색했기 때문이다.

- toString은 Object.prototype의 메서드다.

- ES6에서 도입된 Reflect.has 메서드를 사용할 수 있다. in 연산자와 동일하게 동작한다.

- ### Object.prototype.hasOwnProperty 메서드
  Object.prototype.hasOwnProperty 메서드를 사용해도 객체의 특정 프로퍼티가 존재하는지 확인할 수 있다.

```javascript
const person = { name: 'An' };

console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('age')); // false
```

## 프로퍼티 열거

- ### for ... in 문

객체의 모든 프로퍼티를 순회하며 열거하려면 for ... in 문을 사용한다.

```javascript
for (변수선언문 in 객체) {...}
```

for ... in 문은 in 연산자처럼 순회 대상 객체의 프로퍼티뿐만 아니라 상속받은 프로토타입의 프로퍼티까지 열거한다.
하지만 toString과 같은 Object.prototype의 프로퍼티가 열거되지 않는다.

toString 메서드가 열거할 수 없도록 정의되어 있는 프로퍼티이기 때문이다.

- Object.prototype.string 프로퍼티의 프로퍼티 어트이뷰트 [[Enumerable]]의 값이 false이기 때문.

for ... in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거한다.

- 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.
- 상속받은 프로퍼티를 제외하고 객체 자신의 프로퍼티만 열거하려면 Object.prototype.hasOwnProperty 메서드를 사용하여 객체 자신의 프로퍼티인지 확인해야 한다.
- for ... in 문은 프로퍼티를 열거할 때 순서를 보장하지 않는다.하지만 모던 브라우저는 순서를 보장하고 숫자인 프로퍼티 키에 대해서는 정렬을 실시한다.

배열에는 for ... in 문을 사용하지 말고 일반적인 for 문이나 for ... of 문 또는 Array.prototype.forEach메서드를 사용하기 권장.

- ### Object.keys / values / entries 메서드

객체 자신의 고유 프로퍼티만 열거하기 위해서는 for ... in 문을 사용하는 것보다 Object.keys / values / entries 메서드를 사용하는 것을 권장.

- Object.keys 메서드는 객체 자신의 열거 가능한 프로퍼티키를 배열로 반환한다.
- Object.values 메서드는 객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환한다.
- Object.entries 메서드는 객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환한다.
