# ☑️ 20. strict mode

p.313~319

✍️ 2024.05.27(Mon)

## ✅ 20.1_strict mode란?

- 암묵적 전역(implicit global) <br/>
  : JS엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성하며, 이때 전역 객체의 x 프로퍼티는 전역 변수처럼 사용 가능한 현상

<br/>

## ✅ 20.2_strict mode의 적용

`‘use strict’;`

<br/>

## ✅ 20.3\_전역에 strict mode를 적용하는 것은 피하자

- 전역에 적용한 strict mode는 스크립트 단위로 적용됨 <br/>
  ⇒ 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode 적용

<br/>

## ✅ 20.4\_함수 단위로 strict mode를 적용하는 것도 피하자

<br/>

## ✅ 20.5_strict mode가 발생시키는 에러

### 🔸 20.5.1\_암묵적 전역

- 선언하지 않은 변수 참조 → `ReferenceError` 발생

<br/>

### 🔸 20.5.2\_변수, 함수, 매개변수의 삭제

- delete 연산자로 변수, 함수, 매개변수 삭제 → `SyntaxError` 발생

<br/>

### 🔸 20.5.3\_매개변수 이름의 중복

- 중복된 매개변수 이름 사용 → `SyntaxError`

<br/>

### 🔸 20.5.4_with문의 사용

- with문 사용 → `SyntaxError` 발생
  - 전달된 객체를 스코피 체인에 추가
  - 동일 객체의 프로퍼티 반복 사용시 객체 이름 생략 가능 → 코드 간단 <br/>
    But, 성능 & 가독성 Bad ⇒ with문 사용 지양

<br/>

## ✅ 20.6_strict mode 적용에 의한 변화

### 🔸 20.6.1\_일반 함수의 this

- strict mode에서 함수를 일반 함수로서 호출하면 this에 `undefind`가 바인딩됨 <br/`>
  why? 생성자 함수가 아닌 일반 함수 내부에선 this 사용할 필요 없기 때문 → error는 발생 X

```tsx
(function () {
  "use strict";

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
});
```

<br/>

### 🔸 20.6.2_arguments 객체

- strict mode에선 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않음
