# ☑️ 24\_클로저

p.388~416

✍️ 2023.02.20(Mon) / 2024.09.12(Thurs)~09.13(Fri)

> `A closure is the combination of a function and the lexical environment within which that function was declared.` <br/>
> ⇒ 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합

- `렉시컬 환경`: 함수가 정의된 위치의 스코프 ⇒ 상위 스코프를 의미하는 실행 컨텍스트의 렉시컬 환경

```jsx
const x = 1;

function outerFunc() {
  const x = 10;

  // 중첩 함수
  // 상위 스코프 = 외부 함수 outerFunc 스코프 -> outerFunc의 x 변수에 접근 가능
  function innerFunc() {
    console.log(x); // 10
  }

  // 만약 innerFunc 함수가 outerFunc 함수 내부에 정의된 중첩 함수가 아니라면 outerFunc 함수 변수에 접근 불가
  // ⌙ Because JS is 렉시컬 스코프를 따르는 프로그래밍 언어이기 때문!

  innerFunc();
}

OuterFunc();
```

<br/>

## ✅ 24.1\_렉시컬 스코프

- `렉시컬 스코프`(= 정적 스코프): JS엔진은 함수를 어디에 정의했는지에 따라 상위 스코프 결정 (호출은 영향 ❌)

- 스코프 실체는 실행 컨텍스트의 렉시컬 환경!

  - 이는 외부 렉시컬 환경에 대한 참조를 통해 상위 렉시컬 환경과 연결 ⇒ `스코프 체인`

- 함수의 상위 스코프를 결정한다 = 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값 = 상위 렉시컬 환경의 참조 = `상위 스코프`

💡 즉, 렉시컬 스코프란 상위 스코프의 참조가 함수 정의 평가 시점에 함수가 정의된 환경(위치)에 의해 결정되는 것을 의미한다.

<br/>

## ✅ 24.2\_함수 객체의 내부 슬롯 `[[Environment]]`

- 렉시컬 스코프가 가능하려면 함수는 자신이 정의된 환경 즉, 상위 스코프를 기억해야 한다 <br/>
  ⌙ 이를 위해 함수는 자신의 내부 슬롯 `[[Environment]]`에 자신이 정의된 환경인 상위 스코프 참조 저장 <br/>
  ⇒ 함수 정의가 평가되어 함수 객체를 생성할 때 상위 스코프 참조를 함수 객체 자신의 내부 슬롯 `[[Environment]]`에 저장 <br/>
  (이는 현재 실행 중인 실행 컨텍스트의 렉시컬 환경! → 이를 참조하는 것은 상위 스코프)

👀 함수 객체는 내부 슬롯 `[[Environment]]`에 저장한 렉시컬 환경의 참조 즉, 상위 스코프를 자신이 존재하는 한 기억함

```jsx
const x = 1;

function foo() {
  const x = 10;

  // 상위 스코프는 함수 정의 환경에 따라 결정됨 (함수 호출 위치와 상위 스코프는 관계 X)
  bar();
}

// 함수 bar는 자신의 상위 스코프 = 전역 렉시컬 환경을 [[Environment]]에 저장해서 기억함
function bar() {
  console.log(x);
}

// foo, bar 함수 모두 전역에서 함수 선언문으로 정의
// -> 전역 코드 평가 시점에 평가 -> 함수 객체 생성 & 전역 객체 window 메서드가 됨
// 함수 객체 내부 슬롯 [[Environment]]엔 함수 정의가 평가된 시점
// 즉, 전역 코드 평가 시점에 실행 중인 실행 컨텍스트의 렉시컬 환경인 전역 렉시컬 환경의 참조가 저장됨
```

<br/>

## ✅ 24.3\_클로저와 렉시컬 환경

```jsx
const x = 1;

function outer() {
  const x = 10;
  const inner = function () {
    console.log(x);
  };
  return inner;
}

// outer 함수 호출 -> 중첩 합수 inner 반환 & 생명주기 마감
// => outer 함수 실행 종료 -> outer 함수 실행 컨텍스트는 실행 컨텍스트 스택에서 제거(pop)
const innerFunc = outer();

innerFunc(); // 10 (outer 함수의 지역 변수 x 값)

// ** 외부함수보다 중첩함수가 더 오래 유지되는 경우
// 중첩함수는 이미 생명 주기가 종료한 외부함수의 변수 참조 가능 => 클로저(closure)
```

- 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 함수의 렉시컬 환경까지 소멸하는 것은 아님

  - outer 함수 렉시컬 환경은 inner 함수 내부 슬롯에 의해 참조
  - inner 함수는 전역 변수 innerFunc에 의해 참조

  👉 가비지 컬렉션 대상이 되지 않음 (가비지 컬렉션은 누군가가 참조하고 있는 메모리 공간을 함부로 해제 ❌)

- 외부 함수보다 더 오래 생존한 중첩 함수는 외부 함수 생존 여부와 상관없이 자신이 정의된 위치에 의해 결정된 상위 스코프 기억 → 상위 스코프의 식별자 참조 및 식별자 값 변경 가능 ⭕️

<br/>

🙅‍♀️ <b>클로저가 아닌 경우</b>

- 상위 스코프의 어떤 식별자도 참조하지 않는 함수
- 외부함수의 외부로 중첩함수가 반환되지 않음 → 외부함수보다 중첩함수의 생명주기가 짧음

```jsx
function foo() {
  const x = 1;
  const y = 2;

  // closure
  // 중첩함수 bar는 외부함수보다 더 오래 유지되며 상위 스코프 식별자 참조
  function bar() {
    debugger;
    console.log(x);
  }

  return bar;
}

const bar = foo();
bar();
```

> 💡 클로저는 아래와 같은 케이스로 한정 !
>
> 1.  중첩함수가 상위 스코프 식별자를 참조하며,
> 2.  중첩함수가 외부함수보다 더 오래 유지되는 경우

✋ 클로저가 참조하고 있지 않는 식별자는 기억하지 않아 불필요한 메모리 점유는 걱정하지 않아도 된다!

> 🔥 클로저란? <br/>
> : 함수가 자유 변수에 대해 닫혀있음(close)을 의미 ⇒ 자유 변수에 묶여 있는 함수
>
> \*\* 자유 변수란? 클로저에 의해 참조되는 상위 스코프 변수

<br/>

## ✅ 24.4\_클로저의 활용

- 클로저는 상태(state)를 안전하게 변경하고 유지하기 위해 사용함 <br/>
  (상태가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위햬)

```jsx
// Bad Case 👎

let num = 0; // 상태 변수

const increase = function () {
  return ++num;
};

// 1. num 값은 increase 함수가 호출되기 전까지 변경되지 않고 유지되어야 함 ⭕️
// => 전역 변수로 관리되어 누구나 접근/변경 가능 (암묵적 결합) -> 상태 변경될 수 있음 ❌
// 2. num 값은 increase 함수만이 변경할 수 있어야 함
```

```jsx
// Better Case 👍

// 상태 변경 함수
const increase = function () {
  let num = 0; // 상태 변수 -> 지역 변수로 변경하여 의도치 않은 상태 변경 방지!
  return ++num;
};

// 호출될 때마다 num 재선언 및 0으로 초기화됨 -> 이전 상태 유지 못함 🚨
console.log(increase()); // 1
console.log(increase()); // 1
console.log(increase()); // 1
```

```jsx
// Good Case 👍👍

const increase = (function () {
  let num = 0; // 상태 변수

  // Closure
  return function () {
    return ++num;
  };
})();

// 즉시 실행 함수 호출 -> 즉시 실행 함수가 반환한 함수가 increase 변수에 할당
// increase 변수에 할당된 함수는 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저!

// 즉시 실행 함수는 호출된 이후 소멸
// 즉시 실행 함수가 반환한 클로저 (즉시실행함수의 렉시컬 환경 기억하고 있음) - increase 변수에 할당되어 호출
// -> 카운트 상태 유지를 위한 자유 변수인 num을 언제 어디서 호출하든지 참조/변경 가능

// 즉시 실행 함수는 1번만 실행되므로 increase가 호출될 때마다 num 변수가 초기화될 일은 없을 것!
```

👀 즉시 실행 함수 내에서 선언된 변수는 인스턴스를 통해 접근 불가하며, 즉시 실행 함수 외부에서도 접근 불가한 은닉된 변수!

<br/>

<b>‣ 함수형 프로그래밍에서 클로저 활용 CASE</b>

```jsx
function makeCounter(aux) {
  let counter = 0; // 자유 변수

  // closure 반환
  return function () {
    // 인수로 전달받은 보조 함수에 상태 변경 위임
    counter = aux(counter);
    return counter;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

function decrease(n) {
  return --n;
}

// 함수로 함수 생성
// makeCounter 함수는 보조 함수를 인수로 전달받아 함수 반환
const increaser = makeCounter(increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

// 별개의 독립된 렉시컬 환경이므로 카운터 상태 연동 X
const decreaser = makeCounter(decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2

// => makeCounter 함수를 호출하여 함수 반환시 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖음
```

```jsx
// 증감 가능한 카운터를 만들려면 렉시컬 환경을 공유하는 클로저를 만들어야 함
// 함수 2번 호출 XX

const counter = (function () {
  let counter = 0; // 자유 변수

  // closure 반환
  return function (aux) {
    counter = aux(counter); // 인수로 전달받은 보조 함수에 상태 변경 위임
    return counter;
  };
})();

// 보조 함수
function increase(n) {
  return ++n;
}

function decrease(n) {
  return --n;
}

console.log(counter(increase)); // 1
console.log(counter(increase)); // 2

// 자유 변수 공유
console.log(counter(decrease)); // 1
console.log(counter(decrease)); // 0
```

<br/>

## ✅ 24.5\_캡슐화와 정보 은닉

- `캡슐화`(`encapsulation`): 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것
- `정보 은닉`(`information hiding`): 객체의 특정 프로퍼티/메서드를 감출 목적으로 사용 <br/>
  👉 적절치 못한 접근으로부터 객체 상태가 변경되는 것을 방지해 정보 보호하고, 객체 간의 상호 의존성, 즉 결합도(coupling) ⬇️

- 객체지향 프로그래밍 언어는 클래스를 정의하고 클래스 구성 멤버에 대해 접근 제한자 (e.g. `public`, `private`, `protected`) 선언하여 공개 범위 한정 가능
  - `public` : 클래스 외부에서 참조 가능 ⭕️
  - `private` : 클래스 외부에서 참조 불가능 ❌
- But, JS는 접근 제한자 제공 ❌ → JS 객체 모든 프로퍼티/메서드는 기본적으로 외부 공개 ⇒ `public`

```jsx
function Person(name, age) {
  this.name = name; // public
  let _age = age; // private (지역 변수)

  // 인스턴스 메서드 -> Person 객체 생성될 때 마다 중복 생성
  this.sayHi = function () {
    console.log(`My name is ${this.name}. I am ${_age}.`);
  };
}

const me = new Person("Choi", 20);
me.sayHi(); // My name is Choi. I am 20.
console.log(me.name); // Choi
chonsole.log(me._age); // undefined

// ----

// 인스턴스 메서드에서 프로토타입 메서드로 변경 -> sayHi 메서드 중복 생성 방지
Perosn.prototype.sayHi = function () {
  // Person 생성자 함수의 지역 변수 _age 참조 불가
  console.log(`My name is ${this.name}. I am ${_age}.`);
};

// ----

// 즉시 실행함수 사용
const Person = (function () {
  let _age = 0; // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name; // public
    _age = age;
  }

  // 프로토타입 메서드
  // 즉시 실행 함수가 종료된 이후 호출됨
  // But, Person 생성자 함수 & sayHi 메서드 - 이미 종료
  // -> 소멸한 즉시 실행 함수의 지역 변수 _age 참조 가능한 클로저
  Person.prototype.sayHi = function () {
    console.log(`My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수 반환
  return Person;
})();

const me = new Person("Choi", 20);
me.sayHi(); // My name is Choi. I am 20.
console.log(me.name); // Choi
console.log(me._age); // undefined

// Person.prototype.sayHi 메서드 - 즉시 실행 함수 호출시 생성
// 자신의 상위 스코프인 즉시 실행 함수 실행 컨텍스트의 렉시컬 환경 참조를 [[Environment]]에 저장 기억
// => Person.prototype.sayHi 메서드의 상위 스코프는 하나의 동일한 상위 스코프를 사용하게 됨
// So, Person 생성자 함수가 여러개 인스턴스 생성할 경우 -> _age 변수 상태 유지 X
```

<br/>

## ✅ 24.6\_자주 발생하는 실수

```jsx
// 👎
var funcs = [];

// var로 선언한 i 변수는 함수 레벨 스코프를 갖기 때문에 전역 변수
for (var i = 0; i < 3; i++) {
  funcs[i] = function () {
    return i;
  };
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // 3
}

// ---------------------

// 👍 클로저 사용

for (var i = 0; i < 3; i++) {
  // 즉시 실행 함수
  funcs[i] = (function (id) {
    // id - 중첩 함수의 상위 스코프에 존재 - 자유 변수 (값 유지)
    return function () {
      // 중첩 함수 반환
      return id;
    };
  })(id);
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());
}

// => JS 함수 레벨 스코프 특성상 var로 선언한 변수는 전역 변수가 되어버려 발생하는 현상

// ------------------------

// 👍👍

const funcs = [];

// let 키워드 사용하면 간단히 선언 가능
// for문 코드 블록 반복 실행될 때마다 for문 코드 블록의 새로운 렉시컬 환경 생성됨
for (let i = 0; i < 3; i++) {
  funcs[i] = function () {
    return i;
  };
}

for (let i = 0; i < funcs.length; i++) {
  console.log(funcs[i]()); // 0 1 2
}

// 함수의 상위 스코프는 for문 코드 블록 반복 실행될 때마다 식별자 값을 유지해야 함
// => for문 반복될 때마다 독립적인 렉시컬 환경 생성하여 식별자 값 유지

// ------------------------

// 👍👍👍 고차함수 사용

// 요소가 3개인 배열 생성 -> 배열 인덱스 반환 함수를 요소로 추가
// 배열 요소로 추가된 함수들은 모두 클로저
const funcs = Array.from(new Array(3), (_, i) => () => i); // (3) [f, f, f]

fucns.forEach((f) => console.log(f())); // 0 1 2
```

> **‣ `let`/`const` 사용 반복문 실행 원리**
>
> 1.  새로운 렉시컬 환경 생성 및 초기화 변수 식별자/값 등록
> 2.  새롭게 생성된 렉시컬 환경을 현재 실행 중인 실행 컨텍스트의 렉시컬 환경으로 교체
> 3.  for문 코드 블록 반복 실행 시작되면 → 새로운 렉시컬 환경 생성 및 for문 코드 블록 내 식별자/값 등록
> 4.  새롭게 생성된 렉시컬 환경을 현재 실행 중인 실행 컨텍스트의 렉시컬 환경으로 교체
> 5.  for문 코드 블록 반복 실행 모두 종료되면 for문 실행 이전의 렉시컬 환경을 실행 중인 실행 컨텍스트의 렉시컬 환경으로 되돌림
>
> ⇒ 코드 블록 반복 실행시 새로운 렉시컬 환경 생성하여 반복 당시 상태를 스냅샵처럼 저장

✋ 반복문 코드 블록 내부에서 함수 정의할 때 의미있음 <br/>
(함수 정의가 없는 반복문이라면 생성된 새로운 렉시컬 환경은 아무도 참조하지 않으므로 가비지 컬렉션 대상)
