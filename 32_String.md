# ☑️ 32_String

p.592~604

✍️ 2024.10.07(Mon)~10.11(Fri)

## ✅ 32.1_String 생성자 함수

```jsx
// [[StringData]] 내부 슬롯에 String 래퍼 객체 생성

// 인수 전달 X -> 빈문자열 할당한 String 래퍼 객체
const strObj = new String();
console.log(strObj); // String {length: 0, [[PrimitiveValue]]: ''}

// 인수로 문자열 전달 -> 문자열 할당한 String 래퍼 객체
const strObj = new String("Choi");
console.log(strObj);

// String {0: 'C', 1: 'h', 2: 'o', 3: 'i', length: 4, [[PrimitiveValue]]: 'Choi'}
```

- String 래퍼 객체는 유사 배열 객체(각 문자를 프로퍼티 값으로 갖음)이면서 iterable <br/>
  ⇒ 배열과 유사하게 index 사용하여 각 문자 접근 가능

      ```jsx
      console.log(strObj[0]); // C

      // 🚨 문자열은 원시값 -> 변경 불가 -> 에러는 발생 X
      strObj[0] = 'S';
      console.log(strObj); // 'Choi'
      ```

- IF 인수로 문자열이 아닌 값 전달하면, 인수를 문자열로 강제 변환 후, 변환된 문자열을 할당한 String 래퍼 객체 생성

  ```jsx
  let strObj = new String(123);
  console.log(strObj); // String {0: '1', 1: '2', 2: '3', length; 3, [[PrimitiveValue]]: '123'}

  strObj = new String(null);
  console.log(strObj); // String {0: 'n', 1: 'u', 2: 'l', 3: 'l', length; 4, [[PrimitiveValue]]: 'null'}
  ```

- IF new 연산자 없이 String 생성자 함수 호출하면, String 인스턴스가 아닌 문자열 반환 (명시적 타입 변환 ok)

  ```jsx
  // Number -> String
  String(1); // '1'
  String(NaN); // 'NaN'
  String(Infinity); // 'Infinity'

  // Boolean -> String
  String(true); // 'true'
  String(false); // 'false'
  ```

<br/>

## ✅ 32.2_length 프로퍼티

- `length` 프로퍼티: 문자열의 문자 개수 반환
  - 프로퍼티 `key`: index를 나타내는 숫자
  - 프로퍼티 `value`: 문자
    ⇒ String 래퍼 객체는 유사 배열 객체

<br/>

## ✅ 32.3_String 메서드

- 배열엔 원본배열 직접 변경 메서드(`mutator method`) & 원본 배열 직접 변경하지 않고 새로운 배열 생성 반환 메서드(`accessor method`) 존재
- But, String 객체엔 원본 String 래퍼 객체 직접 변경 메서드 존재 ❌ <br/>
  ⇒ String 객체 메서드는 항상 새로운 문자열 반환 <br/>
  ✋ 문자열은 변경 불가능(immutable)한 원시 값이므로 String 래퍼 객체도 읽기 전용(read-only) 객체로 제공함

```jsx
const strObj = new String("Choi");

console.log(Object.getOwnPropertyDescriptors(strObj)); // Read-only Obejct

/* writable property attribute value = false
{
	'0': { value: 'C', writable: false, enumerable: true, configurable: false },
	...
}

IF String 래퍼 객체가 읽기 전용 객체가 아니라면, 변경된 String 래퍼 객체를 문자열로 되돌릴 때 문자열이 변경됨
=> String 객체의 모든 메서드는 String 래퍼 객체 직접 변경 불가하고, String 객체 메서드는 항상 새로운 문자열 반환!
*/
```

<br/>

### 🔸 32.3.1_String.prototype.indexOf

- `indexOf()`: 대상 문자열에서 인수로 전달받은 문자열을 검색하여 1번째 인덱스 반환 (검색 실패시 -1 반환)

  ```jsx
  const str = "Hello World";

  str.indexOf("l"); // 2
  str.indexOf("or"); // 7
  str.indexOf("x"); // -1
  ```

- 2번째 인수로 검색을 시작할 인덱스 전달 가능

  ```jsx
  // index 3 부터 'l' 검색
  str.indexOf("l", 3); // 3
  ```

- 대상 문자열에 특정 문자열이 존재하는지 확인할 때 유용!
  ```jsx
  // str 문자열에 'Hello'가 포함되어 있다면
  if (str.indexOf('Hello') !== -1) { ... }
  ```
  👍 `String.prototype.includes` (ES6) 사용시 가독성 더 굿!
  ```jsx
  if (str.includes('Hello')) { ... }
  ```

<br/>

### 🔸 32.3.2_String.prototype.search

- `search()`: 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열 검색하여 일치하는 문자열 인덱스 반환 <br/>
  (검색 실패시 -1 반환)

  ```jsx
  const str = "Hello wrold";

  str.search(/o/); // 4
  str.search(/x/); // -1
  ```

<br/>

### 🔸 32.3.3_String.prototype.includes

- `includes()` (ES6) : 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 그 결과를 `true`/`false` 로 반환

  ```jsx
  const str = "Hello world";

  str.includes("Hello"); // true
  str.includes(""); // true
  str.includes("x"); // false
  str.includes(); // false
  ```

- 2번째 인수로 검색을 시작할 인덱스 전달 가능

  ```jsx
  str.includes("l", 3); // true
  str.includes("H", 3); // false
  ```

<br/>

### 🔸 32.3.4_String.prototype.startsWith

- `startsWith()`(ES6): 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 그 결과를 `true`/`false` 로 반환

  ```jsx
  const str = "Hello world";

  str.startsWith("He"); // true
  str.startsWith("he"); // false
  str.startsWith("wo"); // false
  ```

- 2번째 인수로 검색을 시작할 인덱스 전달 가능
  ```jsx
  str.startsWith(" ", 5); // true
  ```

<br/>

### 🔸 32.3.5_String.prototype.endsWith

- `endsWith()` (ES6): 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를 `true`/`false`로 반환

  ```jsx
  const str = "Hello world";

  str.endsWith("ld"); // true
  str.endsWith("He"); // false
  ```

- 2번째 인수로 검색할 문자열 길이 전달 가능
  ```jsx
  // 문자열 str 처음부터 5자리까지('Hello')가 'lo'로 끝나는지 확인
  str.endsWith("lo", 5); // true
  ```

<br/>

### 🔸 32.3.6_String.prototype.charAt

- `charAt()`: 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환

  ```jsx
  const str = "Hello";

  for (let i = 0; i < str.length; i++) {
    console.log(str.charAt(i)); // H e l l o
  }
  ```

✋ 인덱스가 문자열의 범위를 벗어난 정수인 경우 빈 문자열 반환

```jsx
// str인 'Hello'의 경우 index 0~4 까지 존재
str.charAt(5); // ''
```

<br/>

### 🔸 32.3.7_String.prototype.substring

- `substring()`: 대상 문자열에서 1번째 인수로 전달받은 인덱스에 위치하는 문자부터 2번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열 반환

  ```jsx
  const str = "Hello World";

  // index 1~(4-1) = 1~3 까지의 문자열 반환
  str.substring(1, 4); // ell
  ```

- 2번째 인수 생략 가능 (1번째 인수의 인덱스부터 마지막 문자까지 부분 문자열 반환)

  ```jsx
  const str = "Hello World";

  // index 1~끝까지
  str.substring(1); // 'ello World'
  ```

- 1번째 인수 < 2번째 인수 ⇒ 만족하는 정수여야 정상이지만, 아래와 같은 경우에도 정상 동작
  - 1번째 인수 > 2번째 인수 ⇒ 두 인수는 교환됨
  - 인수 < 0 또는 NaN ⇒ 0으로 취급
  - 인수 > 문자열 길이(`str.length`) ⇒ 인수는 문자열 길이(`str.length`)로 취급

```jsx
const str = "Hello World";

// 1번째 인수 > 2번째 인수 -> 두 인수 교환됨
// 즉, (4, 1) -> (1, 4) 로 해석하여 index 1~3 까지의 문자열 반환
str.substring(4, 1); // 'ell'

// 인수 < 0 -> 0으로 취급
// 즉, (-2) -> (0) => 0번째 인덱스부터 마지막 문자열까지 반환
str.sbstring(-2); // 'Hello World'

// 인수 > 문자열 길이(str.length) => str.length로 취급
str.substring(1, 100); // 'ello World'
str.substring(20); // ''
```

- `String.prototype.indexOf`와 같이 사용하면 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열 취득 가능

  ```jsx
  const str = "Hello World";

  // str.indexOf(' ') = 5
  // str.substring(0, 5) -> index 0~4
  str.substring(0, str.indexOf(" ")); // 'Hello'

  // str.substring(6, 11) -> index 6~10
  str.substring(str.indexOf(" ") + 1, str.length); // 'World'
  ```

<br/>

### 🔸 32.3.8_String.prototype.slice

- `slice()`: `substring`와 동일하게 동작 / But, `slice`엔 음수 인수 전달 가능

  - 음수 인수 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열 잘라내어 반환

  ```jsx
  const str = "hello world";

  // 동일하게 동작
  str.substring(0, 5); // 'hello'
  str.slice(0, 5); // 'hello'

  // 인수 0 취급
  str.substring(-5); // 'hello world'

  // 뒤에서 5자리 잘라내어 반환
  str.slice(-5); // 'world'
  ```

<br/>

### 🔸 32.3.9_String.prototype.toUpperCase

- `toUpperCase()`: 대상 문자열을 모두 대문자로 변경한 문자열 반환

  ```jsx
  const str = "Hello World!";

  str.toUpperCase(); // 'HELLO WORLD!'
  ```

<br/>

### 🔸 32.3.10_String.prototype.toLowerCase

- `toLowerCase()`: 대상 문자열을 모두 소문자로 변경한 문자열 반환
  ```jsx
  str.toLowerCase(); // 'hello world!'
  ```

<br/>

### 🔸 32.3.11_String.prototype.trim

- `trim()`: 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열 반환

  ```jsx
  const str = "    foo    ";

  str.trim(); // 'foo'
  str.trimStart(); // 'foo    '
  str.trimEnd(); // '    foo'

  // String.prototype.replace 메서드에 정규 표현식을 인수로 전달하여 공백 문자 제거 가능
  str.replace(/\s/g, ""); // 'foo'
  str.replace(/^\s+/g, ""); // 'foo    '
  str.replace(/\s+$/g, ""); // '    foo'
  ```

<br/>

### 🔸 32.3.12_String.prototype.repeat

- `repeat()` (ES6): 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열 반환

  - 인수로 전달받은 정수가 0 → 빈 문자열 반환
  - 인수로 전달받은 정수가 음수 → `RangeError`
  - 인수 생략 → 기본값 0

  ```jsx
  const str = "abc";

  str.repeat(); // ''
  str.repeat(0); // ''
  str.repeat(2); // 'abcabc'
  str.repeat(2.5); // 'abcabc' (2.5 -> 2)
  str.repeat(-1); // RangeError
  ```

<br/>

### 🔸 32.3.13_String.prototype.replace

- `replace()`: 대상 문자열에서 1번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 2번째 인수로 전달한 문자열로 치환한 문자열 반환

  ```jsx
  const str = "Hello world";

  str.replace("world", "Choi"); // 'Hello Choi'

  // 특수 교체 패턴 사용 가능
  // $& => 검색된 문자열
  str.replace("world", "<stong>$&</string>");
  ```

✋ 검색된 문자열이 여럿 존재할 경우 1번째로 검색된 문자열만 치환

```jsx
const str = "Hello world world";

str.replace("world", "Choi"); // 'Hello Choi world'
```

- 1번째 인수로 정규 표현식 전달 가능

  ```jsx
  const str = "Hello Hello";

  // 대소문자 구별 없이 'hello' 전역 검색
  str.replace(/hello/gi, "Choi"); // 'Choi Choi'
  ```

- 2번째 인수로 치환 함수 전달 가능

  ```jsx
  function calmelToSnake(camelCase) {
    // /.[A-Z]/g => 임의 한 문자 + 대문자 로 이루어진 문자열에 매치
    return camelCase.replace(/.[A-Z]/g, (match) => {
      console.log(match); // 'oW'
      return match[0] + "_" + match[1].toLowerCase();
    });
  }

  const camelCase = "helloWorld";
  camelToSnake(camelCase); // 'hello_world';
  ```

<br/>

### 🔸 32.3.14_String.prototype.split

- `split()`: 대상 문자열에서 1번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열 반환
  - 인수로 빈 문자열 전달 → 각 문자 모두 분리
  - 인수 생략 → 대상 문자열 전체를 단일 요소로 하는 배열 반환

```jsx
const str = "How are you doing?";

str.split(" "); // ['How', 'are', 'you', 'doing?']

// \s : 여러 가지 공백 문자(space, tab ...) 의미
str.split(/\s/); // ['How', 'are', 'you', 'doing?']

str.split(""); // ['H', 'o', 'w', ' ', 'a', 'r', 'e', ' ', ... , '?']

str.split(); // ['How are you doing?']
```

- 2번째 인수: 배열 길이 지정 가능

  ```jsx
  str.split(" ", 3); // ['How', 'are', 'you']
  ```

- 배열 반환하므로 `Array.prototype.reverse`, `Array.prototype.join` 메서드와 함께 사용하면 문자열 역순 뒤집기 가능

  ```jsx
  function reverseString(str) {
    return str.split("").reverse().join("");
  }

  reverseString("Hello world!"); // '!dlrow olleH'
  ```
