# ☑️ 30_Date

p.566~577

✍️ 2024.10.02(Wed)

- UTC(협정 세계시; `Coordinated Universal Time`): 국제 표준시 = GMT(그리니치 평균시)
- KST(한국 표준시; `Korea Standard Tim`e`): UTC + 9시간

```jsx
UTC 00:00 AM = KST 09:00 AM
```

<br/>

## ✅ 30.1_Date 생성자 함수

- `Date`: 생성자 함수

  - `1970.01.01 00:00:00(UTC)` 기점

- Date 생성자 함수로 생성한 Date 객체는 현재 날짜와 시간을 나타내는 정수값

### 🔸 30.1.1_new Date()

- Date 생성자 함수를 인수 없이 new 연산자와 호출하면 현재 날짜와 시간을 가지는 Date 객체 반환

  ```jsx
  new Date(); // Wed Oct 02 2024 15:11:51 GMT+0900 (한국 표준시)
  ```

- new 연산자 없이 호출하면 Date 객체 대신 날짜와 시간 정보를 나타나는 문자열 반환

  ```jsx
  Date(); // 'Wed Oct 02 2024 15:11:51 GMT+0900 (한국 표준시)'
  ```

<br/>

### 🔸 30.1.2_new Date(milliseconds)

- `1970.01.01 00:00:00(UTC)` 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체 반환

```jsx
new Date(0); // Thu Jan 01 1970 09:00:00 GMT+0900 (한국 표준시)

// 86400000ms = 1day
new Date(86400000); // Fri Jan 02 1970 09:00:00 GMT+0900 (한국 표준시)

/*
1s = 1,000ms
1m = 60s * 1,000ms = 60,000ms
1h = 60m * 60,000ms = 3,600,000ms
1d = 24h * 3,600,000ms = 86,400,000ms
*/
```

<br/>

### 🔸 30.1.3_new Date(dateString)

- 지정된 날짜와 시간을 나타내는 Date 객체 반환
  - 인수로 전달한 문자열은 Date.parse 메서드에 의해 해석 가능한 형식이어야 함

```jsx
new Date("Oct 2, 2024 10:00:00");
// Wed Oct 02 2024 10:00:00 GMT+0900 (한국 표준시)

new Date("2024/10/02/10:00:00");
// Wed Oct 02 2024 10:00:00 GMT+0900 (한국 표준시)
```

<br/>

### 🔸 30.1.4_new Date(year, month[, day, hour, minute, second, millisecond])

- 지정된 날짜와 시간을 나타내는 Date 객체 반환

  - 연, 월 반드시 지정해야 함
  - 지정하지 않은 옵션은 0 or 1 로 초기화됨
  - 연, 월 지정하지 않으면 `1970.01.01 00:00:00(UTC)`을 나타내는 Date 객체 반환

  ```jsx
  year : 1900 ~ (0~99 = 1900~1999)
  month : 0 ~ 11 (0 = 1월)
  day : 1 ~ 31
  hour : 0 ~ 23
  minute : 0 ~ 59
  second : 0 ~ 59
  millisecond : 0 ~ 999
  ```

```jsx
// 2024년 10월
new Date(2024, 9); // Tue Oct 01 2024 00:00:00 GMT+0900 (한국 표준시)

// 가독성 👎 - 2024/10/2/10:00:00:00
new Date(2024, 9, 2, 10, 00, 00, 0); // Wed Oct 02 2024 10:00:00 GMT+0900 (한국 표준시)

// 가독성 👍
new Date("2024/10/2/10:00:00:00"); // Wed Oct 02 2024 10:00:00 GMT+0900 (한국 표준시)
```

<br/>

## ✅ 30.2_Date 메서드

### 🔸 30.2.1_Date.now

- `1970.01.01 00:00:00(UTC)` 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환

```jsx
const now = Date.now(); // 1727850809004
new Date(now); // Wed Oct 02 2024 15:33:29 GMT+0900 (한국 표준시)
```

<br/>

### 🔸 30.2.2_Date.parse

- `1970.01.01 00:00:00(UTC)` 기점으로 전달된 지정 시간까지의 밀리초를 숫자로 변환

```jsx
// UTC
Date.parse("Jan 2, 1970 00:00:00 UTC"); // 86400000

// KST
Date.parse("Jan 2, 1970 09:00:00"); // 86400000
Date.parse("1970/01/02/09:00:00"); // 86400000
```

<br/>

### 🔸 30.2.3_Date.UTC

- `1970.01.01 00:00:00(UTC)` 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환
  - `new Date(year, month, day, hour, minute, second, millisecond)`와 같은형식의 인수를 사용해야 함

```jsx
// 1970.01.02
Date.UTC(1970, 0, 2); // 86400000
Date.UTC("1970/1/2"); // NaN
```

<br/>

### 🔸 30.2.4_Date.prototype.getFullYear

- Date 객체의 연도를 나타내는 정수 반환

```jsx
new Date("2024/10/2").getFullYear(); // 2024
```

<br/>

### 🔸 30.2.5_Date.prototype.setFullYear

- Date 객체에 연도를 나타내는 정수 설정 (option: month, date)

```jsx
const today = new Date();

today.setFullYear(2024);
today.getFullYear(); // 2024

today.setFullYear(1900, 0, 1);
today.getFullYear(); // 1900
```

<br/>

### 🔸 30.2.6_Date.prototype.getMonth

- Date 객체의 월을 나타내는 0~11 정수 반환
  - 0: 1월 / 11: 12월

```jsx
new Date("2024/10/02").getMonth(); // 9
```

<br/>

### 🔸 30.2.7_Date.prototype.setMonth

- Date 객체에 월을 나타내는 0~11 정수 설정 (option: date)

```jsx
today.setMonth(0);
today.getMonth(); // 0

today.setMonth(11, 1); // 12월 1일
today.getMonth(); // 11
```

<br/>

### 🔸 30.2.8_Date.prototype.getDate

- Date 객체의 날짜(1~31)를 나타내는 정수 반환

```jsx
new Date("2024/10/02").getDate(); // 2
```

<br/>

### 🔸 30.2.9_Date.prototype.setDate

- Date 객체의 날짜(1~31)를 나타내는 정수 설정

```jsx
today.setDate(1);
today.getDate(); // 1
```

<br/>

### 🔸 30.2.10_Date.prototype.getDay

- Date 객체의 요일(0~6)을 나타내는 정수 반환
  - 0: 일, 1: 월, 2: 화, …, 6: 토

```jsx
new Date("2024/10/02").getDay(); // 3 (수욜)
```

<br/>

### 🔸 30.2.11_Date.prototype.getHours

- Date 객체의 시간(0~23)을 나타내는 정수 반환

```jsx
new Date("2024/10/02/12:00").getHours(); // 12
```

<br/>

### 🔸 30.2.12_Date.prototype.setHours

- Date 객체에 시간(0~23)을 나타내는 정수 설정 (option: m, s, sm)

```jsx
today.setHours(7);
today.getHours(); // 7

today.setHours(0, 0, 0, 0); // 00:00:00:00
today.getHours(); // 0
```

<br/>

### 🔸 30.2.13_Date.prototype.getMinutes

- Date 객체의 분(0~59) 나타내는 정수 반환

```jsx
new Date("2024/10/02/12:30").getMinutes(); // 30
```

<br/>

### 🔸 30.2.14_Date.prototype.setMinutes

- Date 객체에 분(0~59) 나타내는 정수 설정 (option: s, ms)

```jsx
today.setMinutes(50);
today.getMinutes(); // 50

today.setMinutes(5, 10, 999); // HH:05:10:999
today.getMinutes(); // 5
```

<br/>

### 🔸 30.2.15_Date.prototype.getSeconds

- Date 객체의 초(0~59) 나타내는 정수 반환

```jsx
new Date("2024/10/2/12:30:10").getSeconds(); // 10
```

<br/>

### 🔸 30.2.16_Date.prototype.setSeconds

- Date 객체에 초(0~59) 나타내는 정수 설정 (option: ms)

```jsx
today.setSeconds(30);
today.getSeconds(); // 30

today.setSeconds(10, 0); // HH:MM:10:000
today.getSeconds(); // 10
```

<br/>

### 🔸 30.2.17_Date.prototype.getMilliseconds

- Date 객체의 밀리초(0~999) 나타내는 정수 반환

```jsx
new Date("2024/10/2/12:30:10:200").getMilliseconds(); // 200
```

<br/>

### 🔸 30.2.18_Date.prototype.setMilliseconds

- Date 객체에 밀리초(0~999) 나타내는 정수 설정

```jsx
today.setMilliseconds(123);
today.getMilliseconds(); // 123
```

<br/>

### 🔸 30.2.19_Date.prototype.getTime

- `1970.01.01 00:00:00(UTC)` 기점으로 Date 객체의 시간까지 경과된 밀리초 반환

```jsx
new Date("2024/10/2/12:30").getTime(); // 1727839800000
```

<br/>

### 🔸 30.2.20_Date.prototype.setTime

- Date 객체에 `1970.01.01 00:00:00(UTC)` 기점으로 경과된 밀리초 설정

```jsx
today.setTime(86400000); // 1d
console.log(today); // Fri Jan 02 1970 09:00:00 GMT+0900 (한국 표준시)
```

<br/>

### 🔸 30.2.21_Date.prototype.getTimezoneOffset

- UTC와 Date 객체에 지정된 Locale 시간관의 차이를 분 단위로 반환
  - UTC = KST - 9h

```jsx
const today = new Date(); // today의 지정 Locale = KST

today.getTimeZoneOffset() / 60; // -9
```

<br/>

### 🔸 30.2.22_Date.prototype.toDateString

- 사람이 읽을 수 있는 형식의 문자열로 Date 객체 날짜 반환

```jsx
const today = new Date("2024/10/2/12:30");

today.toString(); // 'Wed Oct 02 2024 12:30:00 GMT+0900 (한국 표준시)'
today.toDateString(); // 'Wed Oct 02 2024'
```

<br/>

### 🔸 30.2.23_Date.prototype.toTimeString

- 사람이 읽을 수 있는 형식의 문자열로 Date 객체 시간을 표현한 문자열 반환

```jsx
today.toTimeString(); // '12:30:00 GMT+0900 (한국 표준시)'
```

<br/>

### 🔸 30.2.24_Date.prototype.toISOString

- ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열 반환

```jsx
today.toString(); // 'Wed Oct 02 2024 12:30:00 GMT+0900 (한국 표준시)'
today.toISOString(); // '2024-10-02T03:30:00.000Z'

today.toISOString().slice(0, 10); // '2024-10-02'
```

<br/>

### 🔸 30.2.25_Date.prototype.toLocaleString

- 인수로 전달한 Locale 기준으로 Date 객체 날짜와 시간을 표현한 문자열 반환
  - 인수 생략시 브라우저가 동작 중인 시스템의 Locale 적용

```jsx
const today = new Date("2024/10/2/12:30");

today.toString(); // 'Wed Oct 02 2024 12:30:00 GMT+0900 (한국 표준시)'
today.toLocaleString(); // '2024. 10. 2. 오후 12:30:00' (ko-KR)
today.toLocaleString("en-US"); // '10/2/2024, 12:30:00 PM'
today.toLocaleString("ja-JP"); // '2024/10/2 12:30:00'
```

<br/>

### 🔸 30.2.26_Date.prototype.toLocaleTimeString

- 인수로 전달한 Locale 기준으로 Date 객체 시간을 표현한 문자열 반환
  - 인수 생략시 브라우저가 동작 중인 시스템의 Locale 적용

```jsx
today.toString(); // 'Wed Oct 02 2024 12:30:00 GMT+0900 (한국 표준시)'
today.toLocaleTimeString(); // '오후 12:30:00' (ko-KR)
today.toLocaleTimeString("en-US"); // '12:30:00 PM'
today.toLocaleTimeString("ja-JP"); // '12:30:00'
```
