# ☑️ 33_7번째 데이터 타입 Symbol

p.592~613

✍️ 2024.10.14(Mon)

## ✅ 33.1\_심벌이란?

- `Symbol`: ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값
  - 다른 값과 중복되지 않는 유일무이한 값 ⇒ 주로 이름 충돌 위험이 없는 유일한 프로퍼티 키 생성을 위해 사용

<br/>

## ✅ 33.2\_심벌 값의 생성

### 🔸 33.2.1_Symbol 함수

- Symbol 값은 Symbol 함수를 호출하여 생성함 (외부 노출되지 않아 확인 불가)
  - cf. 다른 원시값(문자열, 숫자, Boolean, undefined, null)은 리터럴 표기법을 통해 값 생성 가능

```jsx
// new 연산자와 함께 호출하지 않음
const mySymbol = Symbol();
new Symbol(); // TypeError
console.log(typeof mySymbol); // symbol

// 심벌 값 확인 불가
console.log(mySymbol); // Symbol();
```

- 선택적으로 문자열을 인수로 전달 가능

  - 해당 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용됨
  - 심벌 값 생성에 영향 주지 않음
  - ⇒ 심벌 값에 대한 설명이 같더라도 생성된 심벌 값은 유일무이한 값!

  ```jsx
  // 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성함
  const mySymbol1 = Symbol("mySymbol");
  const mySymbol2 = Symbol("mySymbol");

  console.log(mySymbol1 === mySymbol2); // false
  ```

- 객체처럼 접근하면 암묵적으로 래퍼 객체 생성

  ```jsx
  const mySymbol = Symbol("mySymbol");

  // Symbol도 래퍼 객체 생성함
  console.log(mySymbol.description); // mySymbol
  console.log(mySymbol.toString()); // Symbol(mySymbol)
  ```

- 암묵적으로 문자열이나 숫자 타입으로 변환 ❌

  ```jsx
  const mySymbol = Symbol("mySymbol");

  console.log(mySymbol + ""); // TypeError
  console.log(+mySymbol); // TypeError
  ```

- Boolean 타입으론 암묵적으로 타입 변환 가능

  ```jsx
  const mySymbol = Symbol();

  console.log(!!mySymbol); // true

  // if문 등에서 존재 확인 가능
  if (mySymbol) console.log("mySymbol is not empty.");
  ```

<br/>

### 🔸 33.2.2_Symbol.for / Symbol.keyFor 메서드

- `Symbol.for()`: 인수로 전달받은 문자열을 키로 사용하여 전역 심벌 레지스트리(키&심벌 값의 쌍들 저장)에서 해당 키와 일치하는 심벌 값 검색
  - 검색 Success: 새로운 심벌 값 생성하지 않고 검색된 심벌 값 반환
  - 검색 Fail: 새로운 심벌 값 생성하여 `Symbol.for` 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값 반환

```jsx
// 전역 심벌 레지스트리(registry)에 'mySymbol' key로 저장된 심벌 값이 없으면 새로운 심벌 값 생성
const s1 = Symbol.for("mySymbol");

// 'mySymbol' key로 저장된 심벌 값이 있으면 심벌 값 반환
const s2 = Symbol.for("mySymbol");

console.log(s1 === s2); // true
```

- Symbol 함수는 호출될 때마다 unique 심벌 값 생성

  - 전역 심벌 레지스트리(JS엔진이 관리하는 심벌 값 저장소)에서 심벌 값을 검색할 수 있는 키 지정 불가 <br/>
    → 전역 심벌 레지스트리에 등록되어 관리 ❌ <br/>
    But, `Symbol.for` 메서드 사용시 → 애플리케이션 전역에서 중복되지 않는 unique 상수인 심벌 값 only one 생성 → 전역 심벌 레지스트리를 통해 공유 가능

- `Symbol.keyFor()`: 전역 심벌 레지스트리에 저장된 심벌 값의 키 추출 가능

  ```jsx
  const s1 = Symbol.for("mySymbol");
  // 전역 심벌 레지스트리에 저장된 심벌 값의 키 추출
  Symbol.keyFor(s1); // mySymbol

  // Symbol 함수 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않음
  const s2 = Symbol("foo");
  Symbol.keyFor(s2); // undefined
  ```

<br/>

## ✅ 33.3\_심벌과 상수

```jsx
const Direction = { UP: 1, DOWN: 2, LEFT: 3, RIGHT: 4 };

// 값이 아닌 상수 이름에 의미가 있음 => 중복될 가능성이 없는 unique 심벌 값 사용할 수 있음
const Direction = {
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("let"),
  RIGHT: Symbol("right"),
};

// JS에서 enum 사용시 객체 변경 방지를 위해 Object.freeze() & 심벌 값 사용
const Direction = Object.freeze({
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("let"),
  RIGHT: Symbol("right"),
});

// 변수에 상수 할당
const myDirection = Direction.UP;

if (myDirection === Direction.UP) console.log("You are going UP.");
```

<br/>

## ✅ 33.4\_심벌과 프로퍼티 키

- 객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적 생성 가능

  - 대괄호 `[]` 사용해야 함

  ```jsx
  // 심벌 값으로 프로퍼티 키 생성
  const obj = { [Symbol.for("mySymbol")]: 1 };

  obj[Symbol.for("mySymbol")]; // 1
  ```

<br/>

## ✅ 33.5\_심벌과 프로퍼티 은닉

- 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요 없는 프로퍼티 은닉 가능

  ```jsx
  // 심벌 값으로 프로퍼티 키 생성
  const obj = { [Symbol("mySymbol")]: 1 };

  for (const key in obj) {
    console.log(key); // 아무것도 출력 X
  }

  console.log(Object.keys(obj)); // []
  console.log(Object.getOwnPropertyNames(obj)); // []

  // But, ES6에서 도입된 getOwnPropertySymbols 메서드 사용하면
  // 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티 찾을 수 있음
  // getOwnPropertySymbols(): 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환
  console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]
  ```

<br/>

## ✅ 33.6\_심벌과 표준 빌트인 객체 확장

- 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 건 권장 ❌ (Read-only 전용으로 사용하는게 👍~) <br/>
  ⌙ why? 개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드 이름이 중복될 수 있기 때문

  ```jsx
  // ❌ (이렇게 표준 빌트인 객체를 확장하는 건 권장하지 않음)
  Array.prototype.sum = function () {
    return this.reduce((acc, cur) => acc + cur, 0);
  }[(1, 2)].sum(); // 3

  // --------------------------------------------------

  // ⭕️ (중복될 가능성이 없는 심벌 값으로 프로퍼티 키 생성하여 표준 빌트인 객체 확장)
  // -> 기존/미래 어떤 프로퍼티 키와도 충돌할 위험이 없어 안전하게 표준 빌트인 객체 확장 가능
  Array.prototype[Symbol.for("sum")] = function () {
    return this.reduce((acc, cur) => acc + cur, 0);
  }[(1, 2)][Symbol.for("sum")]; // 3
  ```

<br/>

## ✅ 33.7_Well-known Symbol

- `Well-known Symbol`: JS가 기본 제공하는 빌트인 심벌 값 (JS엔진 내부 알고리즘에 사용)

🧚‍♀️ 심벌은 중복되지 않는 상수 값 생성 및 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티 추가를 위해, <br/>
⇒ 즉, 하위 호환성 보장을 위해 도입됨!
