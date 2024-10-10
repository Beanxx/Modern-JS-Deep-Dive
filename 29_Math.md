# ☑️ 29_Math

p.561~565

✍️ 2024.10.02(Wed)

## ✅ 29.1_Math 프로퍼티

### 🔸 29.1.1_Math.PI

- 원주율 PI값 반환

```jsx
Math.PI; // 3.141592...
```

<br/>

## ✅ 29.2_Math 메서드

### 🔸 29.2.1_Math.abs

- `Math.abs`: 인수로 전달된 숫자의 절대값(absolute value; 0 or 양수) 반환

```jsx
Math.abs(-1); // 1
Math.abs("-1"); // 1
Math.abs(""); // 0
Math.abs([]); // 0
Math.abs(null); // 0
Math.abs(undefined); // NaN
Math.abs({}); // NaN
Math.abs("string"); // NaN
Math.abs(); // NaN
```

<br/>

### 🔸 29.2.2_Math.round

- `Math.round`: 인수로 전달된 숫자의 소수점 이하를 반올림한 정수 반환

```jsx
Math.round(1.4); // 1
Math.round(1.6); // 2
Math.round(-1.4); // -1
Math.round(-1.6); // -2
Math.round(1); // 1
Math.round(); // NaN
```

<br/>

### 🔸 29.2.3_Math.ceil

- `Math.ceil`: 인수로 전달된 숫자의 소수점 이하를 올림한 정수 반환

```jsx
Math.ceil(1.4); // 2
Math.ceil(-1.4); // -1
Math.ceil(-1.6); // -1
Math.ceil(); // NaN
```

<br/>

### 🔸 29.2.4_Math.floor

- Math.floor: 인수로 전달된 숫자의 소수점 이하를 내림한 정수 반환

```jsx
Math.floor(1.9); // 1
Math.floor(9.1); // 9
Math.floor(-1.9); // -2
Math.floor(-9.1); // -10
Mathh.floor(); // NaN
```

<br/>

### 🔸 29.2.5_Math.sqrt

- `Math.sqrt`: 인수로 전달도니 숫자의 제곱근 반환

```jsx
Math.sqrt(9); // 3
Math.sqrt(-9); // NaN
Math.sqrt(1); // 1
Math.sqrt(0); // 0
Math.sqrt(); // NaN
```

<br/>

### 🔸 29.2.6_Math.random

- `Math.random`: 임의의 난수(랜덤 숫자; 0~1미만 실수) 반환

```jsx
// 1~10 범위의 정수 구하기
const random = Math.floor(Math.random() * 10 + 1);
console.log(random); // 1~10 integer
```

<br/>

### 🔸 29.2.7_Math.pow

- `Math.pow`: 밑(base; 1번째 인수), 지수(exponent; 2번째 인수)로 거듭제곱한 결과 반환

```jsx
Math.pow(2, 8); // 256 (2^8)
Math.pow(2, -1); // 0.5 (2^-1)
Math.pow(2); // NaN

Math.pow(Math.pow(2, 2), 2); // 16

// 지수 연산자 사용(ES7) -> 가독성 good
2 ** (2 ** 2); // 16
```

<br/>

### 🔸 29.2.8_Math.max

- `Math.max`: 전달받은 인수 중에서 가장 큰 수 반환
  - 인수 전달되지 않으면 `-Infinity` 반환

```jsx
Math.max(1); // 1
Math.max(1, 2, 3); // 3
Math.max(); // -Infinity
```

✋ 배열을 인수로 전달받아 배열 요소 중에서 최대값을 구하려면 `Function.prototype.apply` 나 스프레드 문법 사용해야 함

```jsx
Math.max.apply(null, [1, 2, 3]); // 3

Math.max(...[1, 2, 3]); // 3
```

<br/>

### 🔸 29.2.9_Math.min

- Math.min: 전달받은 인수 중에서 가장 작은 수 반환
  - 인수 전달되지 않으면 `Infinity` 반환

```jsx
Math.min(1); // 1
Main.min(1, 2, 3); // 1
Math.min(); // Infinity
```

✋ 배열을 인수로 전달받아 배열 요소 중에서 최소값을 구하려면 `Function.prototype.apply` 나 스프레드 문법 사용해야 함

```jsx
Math.min.apply(null, [1, 2, 3]); // 1

Math.min(...[1, 2, 3]); // 1
```
