# ☑️ 27\_배열

p.552~560

✍️ 2024.10.02(Wed)

## ✅ 28.1_Number 생성자 함수

- 표준 빌트인 객체 Number 객체는 생성자 함수 객체! <br/>
  ⇒ new 연산자와 함께 호출하여 Number 인스턴스 생성 가능

```jsx
// 인수 할당 X
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0} <- [[NumberData]] 내부 슬롯

// 인수 할당 O
const numObj = new Number(10);
console.log(numObj); // Number {[[PrimitiveValue]]: 10}

// 숫자가 아닌 값 인수로 할당
// 인수를 숫자로 강제 변환 후 Number 래퍼 객체 생성
let numObj = new Number("10");
console.log(numObj); // Number {[[PrimitiveValue]]: 10}

// 숫자로 변환 불가능하다면 NaN을 내부 슬롯에 할당
numObj = new Number("Hello");
console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
```

- new 연산자 사용하지 않고 Number 생성자 함수 호출시 → Number 인스턴스가 아닌 숫자 반환

```jsx
Number("0"); // 0
Number(true); // 1
Number(false); // 0
```

<br/>

## ✅ 28.2_Number 프로퍼티

### 🔸 28.2.1_Number.EPSILON

- `Number.EPSILON`: 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이

  - 부동소수점으로 인해 발생하는 오차 해결을 위해 사용

    ```jsx
    function isEqual(a, b) {
      return Math.abs(a - b) < Number.EPSILON;
    }

    isEqual(0.1 + 0.2, 0.3); // true
    ```

    <br/>

### 🔸 28.2.2_Number.MAX_VALUE

- `Number.MAX_VALUE`: JS에서 표현 가능한 가장 큰 양수 값으로, 이보다 큰 숫자는 `Infinity`

```jsx
Infinity > Number.MAX_VALUE; // true
```

<br/>

### 🔸 28.2.3_Number.MIN_VALUE

- `Number.MIN_VALUE`: JS에서 표현 가능한 가장 작은 양수 값으로, 이보다 작은 숫자는 `0`

```jsx
Number.MIN_VALUE > 0; // true
```

<br/>

### 🔸 28.2.4_Number.MAX_SAFE_INTEGER

- `Number.MAX_SAFE_INTEGER`: JS에서 안전하게 표현 가능한 가장 큰 정수값 (`9007199254740991`)

<br/>

### 🔸 28.2.5_Number.MIN_SAFE_INTEGER

- `Number.MIN_SAFE_INTEGER`: JS에서 안전하게 표현 가능한 가장 작은 정수값 (`-9007199254740991`)

<br/>

### 🔸 28.2.6_Number.POSITIVE_INFINITY

- `Number.POSITIVE_INFINITY` = 양의 무한대를 나타내는 숫자값 `Infinity`

<br/>

### 🔸 28.2.7_Number.NEGATIVE_INFINITY

- `Number.NEGATIVE_INFINITY` = 음의 무한대를 나타내는 숫자값 `-Infinity`

<br/>

### 🔸 28.2.8_Number.NaN

- `Number.NaN`: 숫자가 아님(`Not-a-Number`)을 나타내는 숫자값
  - `Number.NaN` = `window.NaN`

<br/>

## ✅ 28.3_Number 메서드

### 🔸 28.3.1_Number.isFinite

- `Number.isFinite`: 인수로 전달된 숫자값이 정상적인 유한수인지 (`Infinity` / `-Infinity` 아닌지) 검사 → 결과를 `Boolean` 값으로 반환

```jsx
// 인수가 정상적인 유한수 -> true
Number.isFinite(0); // true
Number.isFinite(Number.MAX_VALUE); // true

// 인수가 무한수 -> false
Number.isFinite(Infinity); // false

// 인수가 NaN -> 항상 false
Number.isFinite(NaN); // false
```

✋ `Number.isFinite`는 전달받은 인수를 숫자로 암묵적 타입 변환 ❌ ⇒ 숫자가 아닌 인수의 경우 반환값은 항상 `false` <br/>
(빌트인 전역 함수 isFinite는 전달받은 인수를 암묵적 타입 변환하여 수행)

```jsx
// 암묵적 타입 변환 X
Number.isFinite(null); // false

// 숫자로 암묵적 타입 변환 O (null -> 0)
isFinite(null); // true
```

<br/>

### 🔸 28.3.2_Number.isInteger

- `Number.isInteger`: 인수로 전달된 숫자값이 정수인지 검사 → 결과를 Boolean 값으로 반환
  - 검사 전 인수를 숫자로 암묵적 타입 변환 ❌

```jsx
Number.isInteger(0); // true

Number.isInteger(0.5); // false
Number.isInteger("123"); // false
Number.isInteger(false); // false
Number.isInteger(Infinity); // false
```

<br/>

### 🔸 28.3.3_Number.isNaN

- `Number.isNaN`: 인수로 전달된 숫자값이 NaN인지 검사 → 결과를 Boolean 값으로 반환

```jsx
Number.isNaN(NaN); // true
```

✋ `Number.isNaN`는 전달받은 인수를 숫자로 암묵적 타입 변환 ❌ ⇒ 숫자가 아닌 인수의 경우 반환값은 항상 `false`
(빌트인 전역 함수 isNaN는 전달받은 인수를 암묵적 타입 변환하여 수행)

```jsx
// 암묵적 타입 변환 X
Number.isNaN(undefined); // false

// 암묵적 타입 변환 O (undefined -> NaN)
isNaN(undefined); // true
```

<br/>

### 🔸 28.3.4_Number.isSafeInteger

- `Number.isSafeInteger`: 인수로 전달된 숫자값이 안전한 정수인지 검사 → 결과를 Boolean 값으로 반환

  - 안전한 정수값: `-(2^53 - 1) ~ (2^53 -1)`
  - 검사 전 인수를 숫자로 암묵적 타입 변환 ❌

  <br/>

### 🔸 28.3.5_Number.prototype.toExponential

- `toExponential`: 숫자를 지수 표기법으로 변환하여 문자열로 반환

- 지수 표기법: 매우 크거나 작은 숫자를 표기할 때 주로 사용하며, e 앞에 있는 숫자에 10^n승을 곱하는 형식으로 수를 나타냄
  - 인수로 소수점 이하로 표현할 자릿수 전달 가능
  ```jsx
  (77.1234).toExponential(); // '7.71234e+1'
  (77.1234).toExponential(2); // '7.71e+1'
  ```

```jsx
// JS엔진은 숫자 뒤의 .을 부동 소수점 숫자의 소수 구분 기호로 해석함
// But, 여기서 77은 Number 래퍼 객체로, .을 소수 구분 기호로 해석하여 뒤의 toExponential 해석 불가
77.toExponential(); // SyntaxError

// 그룹 연산자 () 사용
(77).toExponential(); // '7.7e+1'
```

<br/>

### 🔸 28.3.6_Number.prototype.toFixed

- `toFixed`: 숫자 반올림하여 문자열로 반환
  - 인수: 반올림하는 소수점 이하 자릿수 (0~20 정수값) (생략시 default `0` 지정)

```jsx
(12345.6789).toFixed(); // '12346'
(12345.6789).toFixed(2); // '12345.68'
```

<br/>

### 🔸 28.3.7_Number.prototype.toPrecision

- `toPrecision`: 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환
  - 인수: 전체 자릿수를 나타나는 `0~21` 사이의 정수값 (생략시 default `0` 지정)

```jsx
// 전체 자릿수 유효 (생략시 default 0 지정)
(12345.6789).toPrecision(); // '12345.6789'
(12345.6789).toPrecision(2); // '1.2e+4' <- 전체 2자릿수 유효 / 나머지 반올림
```

<br/>

### 🔸 28.3.8_Number.prototype.toString

- `toString`: 숫자 → 문자열로 변환하여 반환
  - 인수: 2~36 사이의 정수값 (생략시 default 10진법 지정)

```jsx
(10).toString(); // '10'
(16).toString(2); // '10000' <- 2진수 문자열 반환
(16).toString(16); // '10' <- 16진수 문자열 반환
```
