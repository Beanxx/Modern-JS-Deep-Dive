# ☑️ 31_RegExp

p.578~591

✍️ 2024.10.02(Wed), 10.07(Mon)

## ✅ 31.1\_정규 표현식이란?

- 정규 표현식: 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(formal language)
  - 문자열 대상으로 패턴 매칭 기능(특정 패턴과 일치하는 문자열 검색/추출/치환 가능 기능) 제공

👍 정규표현식을 사용하면 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크 가능

👎 가독성 bad

<br/>

## ✅ 31.2\_정규 표현식의 생성

- 정규 표현식 리터럴: 패턴과 플래그로 구성
  (e.g. `/regexp/i` ⇒ regexp: pattern, i: flag)

```jsx
const target = "Is this all there is?";

// pattern: is
// flag: i => 대소문자 구별하지 않고 검색
const regexp = /is/i;

regexp.test(target); // true
```

- RegExp 생성자 함수를 사용하여 RegExp 객체 동적 생성 가능

  ```jsx
  new RegExp(pattern[, flags]);
  ```

  ```jsx
  const count = (str, char) => {
    return (str.match(new RegExp(char, "gi")) ?? []).length;
  };

  count("Is this all there is?", "is"); // 3
  count("Is this all there is?", "xx"); // 0
  ```

<br/>

## ✅ 31.3_RegExp 메서드

### 🔸 31.3.1_RegExp.prototype.exec

- `exec()`: 인수로 전달받은 문자열에 대해 정규 표현식 패턴 검색하여 매칭 결과를 배열로 반환
  - 매칭 결과가 없는 경우 return `null`

```jsx
const target = "Is this all there is?";
const regExp = /is/;

regExp.exec(target);
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

✋ 문자열 내의 모든 패턴 검색하는 `g` 플래그를 지정해도 1번째 매칭 결과만 반환함에 주의!

<br/>

### 🔸 31.3.2_RegExp.prototype.test

- test(): 인수로 전달받은 문자열에 대해 정규 표현식 패턴 검색하여 매칭 결과를 Boolean 값으로 반환

```jsx
const target = "Is this all there is?";
const regexp = /is/;

regexp.test(target);
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

<br/>

### 🔸 31.3.3_String.prototype.match

- 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환

```jsx
const target = "Is this all there is?";
const regexp = /is/;

regexp.match(target); // true
```

✋ exec와 달리 match는 `g` 플래그가 지정되면 모든 매칭 결과를 배열로 반환

<br/>

## ✅ 31.4\_플래그

- 정규 표현식 검색 방식 설정을 위해 사용
- 플래그는 옵션으로 선택적으로 사용 가능
- 플래그 사용하지 않은 경우 대소문자 구별해서 패턴 검색
- 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 1번째 매칭한 대상만 검색하고 종료

| 플래그 | 의미        | 설명                                |
| ------ | ----------- | ----------------------------------- |
| i      | Ignore case | 대소문자 구별 X 패턴 검색           |
| g      | Global      | 패턴 일치하는 모든 문자열 전역 검색 |
| m      | Multi line  | 문자열 행 바껴도 계속 패턴 검색     |

```jsx
const target = "Is this all there is?";

// 대소문자 구별하여 1번만 검색
target.match(/is/);
// ['is', index: 5, input: 'Is this all there is?', groups: undefined]

// 대소문자 구별하지 않고 1번만 검색
target.match(/is/i);
// ['Is', index: 0, input: 'Is this all there is?', groups: undefined]

// 대소문자 구별하여 전역 검색
target.match(/is/g); // ['is', 'is']

// 대소문자 구별하지 않고 전역 검색
target.match(/is/gi); // ['Is', 'is', 'is']
```

<br/>

## ✅ 31.5\_패턴

- 문자열의 일정한 규칙을 표현하기 위해 사용
- `/` 로 열고 닫으며, 문자열 따옴표 생략 (따옴표 포함하면 따옴표까지 패턴에 포함되어 검색됨)

### 🔸 31.5.1\_문자열 검색

- RegExp 메서드를 사용하여 검색 대상 문자열과 정규 표현식 매칭 결과를 구하면 검색 수행됨

```jsx
const target = "Is this all there is?";

// 플래그 생략 -> 대소문자 구별하여 1번만 검색
target.match(/is/);

regExp.test(target); // true
target.match(regExp);
// // ['is', index: 5, input: 'Is this all there is?', groups: undefined]

// 대소문자 구별 X -> 플래그 i 사용
const regExp = /is/i;

target.match(regExp);
// ['Is', index: 0, input: 'Is this all there is?', groups: undefined]

// 모든 문자열 전역 검색 -> 플래그 g 사용
const regExp = /is/gi;
target.match(regExp); // ['Is', 'is', 'is']
```

<br/>

### 🔸 31.5.2\_임의의 문자열 검색

- `.` : 임의 문자 1개 의미

```jsx
const target = "Is this all there is?";

// 임의 3자리 문자열을 대소문자 구별하여 전역 검색
const regExp = /.../g;

target.match(regExp); // ['Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?']
```

<br/>

### 🔸 31.5.3\_반복 검색

- `{m,n}` : 최소 m번, 최대 n번 반복되는 문자열 의미 (콤마 뒤에 공백 있으면 정상 동작 ❌)

```jsx
const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 1번 ~ 최대 2번 반복되는 문자열 전역 검색
const regExp = /A{1,2}/g;

target.match(regExp); // ['A', 'AA', 'A', 'AA', 'A']
```

- `{n}` : n번 반복되는 문자열 의미 = `{n, n}`

```jsx
// 'A'가 2번 반복되는 문자열 전역 검색
const regExp = /A{2}/g;

target.match(regExp); // ['AA', 'AA']
```

- `{n,}` : 최소 n번 이상 반복되는 문자열 의미

```jsx
// 'A'가 최소 2번 이상 반복되는 문자열 전역 검색
const regExp = /A{2,}/g;

target.match(regExp); // ['AA', 'AAA']
```

- `+` : 최소 1번 이상 반복되는 문자열 의미 = `{1,}`

```jsx
// 'A'가 최소 1번 이상 반복되는 문자열 전역 검색
const regExp = /A+/g;

target.match(regExp); // ['A', 'AA', 'A', 'AAA']
```

- `?` : 최대 1번(0번 포함) 이상 반복되는 문자열 의미 = `{0,1}`

```jsx
const target = "color colour";

// 'u'가 최대 1번 이상(0번 포함) 반복되고, 'r'이 이어지는 문자열 검색 패턴
const regExp = /colou?r/g;

target.match(regExp); // ['color', 'colour']
```

<br/>

### 🔸 31.5.4_OR 검색

- `|` : or

```jsx
const target = "A AA B BB Aa Bb";

// 'A' 또는 'B' 전역 검색 (대소문자 구별)
const regExp = /A|B/g;

target.match(regExp); // ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']

// --------------------------------------------

// 만약 분해되지 않은 단어 레벨로 검색하고 싶다면 + 함께 사용
const regExp = /A+|B+/g;
const regExp = /[AB]+/g; // 위의 코드 간단 버전 ([] 내 문자는 or로 동작)

target.match(regExp); // ['A', 'AA', 'B', 'BB', 'A', 'B']

// --------------------------------------------

// 범위 지정 : [] 내에 - 사용
const target = "A AA BB ZZ Aa Bb 12";

// 'A' ~ 'Z'가 1번 이상 반복되는 문자열 전역 검색
const regExp = /[A-Z]+/g;

target.match(regExp); // ['A', 'AA', 'BB', 'ZZ', 'A', 'B']

// --------------------------------------------

// 대소문자 구별 X
const regExp = /[A-Za-z]+/g;

target.match(regExp); // ['A', 'AA', 'BB', 'ZZ', 'Aa', 'Bb']

// --------------------------------------------

// 숫자 검색
const target = "AA BB 12,345";

// '0' ~'9'가 1번 이상 반복되는 문자열 전역 검색
const regExp = /[0-9]+/g;

// ,(쉼표)로 매칭 결과 분리 <- 쉼표를 패턴에 포함

target.match(regExp); // ['12', '345']

// --------------------------------------------

// '0' ~'9' 또는 ','가 1번 이상 반복되는 문자열 전역 검색
const regExp = /[0-9,]+/g;

// 위의 예제 간단히 표현
// \d : 숫자 = [0-9]
// \D : 숫자가 아닌 문자

// '0' ~ '9' 또는 ','가 1번 이상 반복되는 문자열 전역 검색
let regExp = /[\d,]+/g;

target.match(regExp); // ['12,345']

// -------------------------------------------

// 숫자가 아닌 문자 또는 ','가 1번 이상 반복되는 문자열 전역검색
regExp = /[\D,]+/g;

target.match(regExp); // ['AA BB ', ',']

// -------------------------------------------

// \w : 알파벳, 숫자, _ 의미 = [A-Za-z0-9_]
// \W : 알파벳, 숫자, _ 가 아닌 문자 의미

const target = "Aa Bb 12,345 _$%&";

// 알파벳, 숫자, _, ',' 가 1번 이상 반복되는 문자열 전역 검색
let regExp = /[\w,]+/g;
target.match(regExp); // ['Aa', 'Bb', '12,345', '_']

// 알파벳, 숫자 , _ 가 아닌 문자 또는 ','가 1번 이상 반복되는 문자열 전역 검색
regExp = /[\W,]/g;
target.match(regExp); // [' ', ' ', ',', ' $%&']
```

<br/>

### 🔸 31.5.5_NOT 검색

- `^` (`[…]` 내부): not
  - `[^0-9]` = `\D` : 숫자를 제외한 문자
  - `[^A-Za-z0-9_]` = `\W`

```jsx
const target = "AA BB 12 Aa Bb";

const regExp = /[^0-9]+/g; // 숫자를 제외한 문자열 전역 검색

target.match(regExp); // ['AA BB ', ' Aa Bb']
```

<br/>

### 🔸 31.5.6\_시작 위치로 검색

- `^` (`[…]` 외부): 문자열 시작

```jsx
const target = "https://naver.com";

const regExp = /^https/; // 'https'로 시작하는지 검사

regExp.test(target); // true
```

<br/>

### 🔸 31.5.7\_마지막 위치로 검색

- `$`: 문자열 마지막

```jsx
const target = "https://naver.com";

const regExp = /com$/; // 'com'으로 끝나는지 검사

regExp.test(target); // true
```

<br/>

## ✅ 31.6\_자주 사용하는 정규표현식

### 🔸 31.6.1\_특정 단어로 시작하는지 검사

```jsx
const url = "https://example.com";

// 'http://' or 'https://'로 시작하는지 검사
/^https?\/\//.test(url); // true
// ===
/^(http||https):\/\//.test(url); // true
```

<br/>

### 🔸 31.6.2\_특정 단어로 끝나는지 검사

```jsx
const fileName = "index.html";

// 'html'로 끝나는지 검사
/html$/.test(fileName); // true
```

<br/>

### 🔸 31.6.3\_숫자로만 이루어진 문자열인지 검사

```jsx
const target = "12345";

// 처음과 끝이 숫자이고 최소 1번 이상 반복되는 문자열 => 숫자로만 이루어진 문자열인지 검사
/^\d+$/.test(target); // true
```

<br/>

### 🔸 31.6.4\_하나 이상의 공백으로 시작하는지 검사

```jsx
const target = " Hi!";

// 하나 이상의 공백으로 시작하는지 검사
/^[\s]+/.test(target); // true

// \s : 여러 가지 공백 문자(스페이스, 탭 ...)
```

<br/>

### 🔸 31.6.5\_아이디로 사용 가능한지 검사

```jsx
const id = "abc123";

// 알파벳 대소문자 또는 숫자로 시작&끝나며 4~10자리인지 검사
/^[A-Za-z0-9]{4,10}$/.test(id); // true
```

<br/>

### 🔸 31.6.6\_메일 주소 형식에 맞는지 검사

```jsx
const email = "example@gmail.com";

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(
  email
); // true
```

<br/>

### 🔸 31.6.7\_핸드폰 번호 형식에 맞는지 검사

```jsx
const cellphone = "010-1234-5678";

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // true
```

<br/>

### 🔸 31.6.8\_특수 문자 포함 여부 검사

```jsx
const target = "abc#123";

/[^A-Za-z0-9]/gi.test(target); // true
```

```jsx
// 특수 문자 제거
target.replace(/[^A-Za-z0-9]/gi, ""); // abc123
```
