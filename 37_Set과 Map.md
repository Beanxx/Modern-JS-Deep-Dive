# ☑️ 37_Set과 Map

p.643~659

✍️ 2024.10.16(Wed)~10.17(Thurs)

## ✅ 37.1_Set

- Set 객체: 중복되지 않는 유일한 값들의 집합

|                         | 배열 | Set 객체 |
| ----------------------- | ---- | -------- |
| 동일 값 중복 포함 가능  | ⭕️  | ❌       |
| 요소 순서에 의미 있음   | ⭕️  | ❌       |
| 인덱스로 요소 접근 가능 | ⭕️  | ❌       |

<br/>

### 🔸 37.1.1_Set 객체의 생성

- Set 객체는 Set 생성자 함수로 생성 (인수 미전달 → 빈 Set 객체 생성)

  ```jsx
  const set = new Set();
  console.log(set); // Set(0) {}
  ```

- Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체 생성 <br/>
  (이터러블 중복된 값은 Set 객체에 요소로 저장되지 않음)

  ```jsx
  const set1 = new Set([1, 2, 3, 4]);
  console.log(set1); // Set(3) {1, 2, 3}

  const set2 = new Set("hello");
  console.log(set2); // Set(4) {'h', 'e', 'l', 'o'}
  ```

- 중복을 허용하지 않는 Set 객체 특성을 활용하여 배열에서 중복된 요소 제거 가능

  ```jsx
  // 배열 중복 요소 제거
  const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);

  // Set 사용한 배열 중복 요소 제거 -> 훨씬 간단!
  const uniq = array => [...new Set(array)];

  console.log(uniq([2, 1, 2, 3, 4, 3, 4]); // [2, 1, 3, 4]
  ```

<br/>

### 🔸 37.1.2\_요소 개수 확인

- Set 객체 요소 개수 확인할 땐 `Set.prototype.size` 프로퍼티 사용

  ```jsx
  // size 프로퍼티 = setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티
  // -> 숫자 할당하여 Set 객체 요소 개수 변경 불가
  const { size } = new Set([1, 2, 3, 3]);

  size = 10; // 무시됨

  console.log(size); // 3
  ```

<br/>

### 🔸 37.1.3\_요소 추가

- Set 객체에 요소 추가할 땐 `Set.prototype.add` 메서드 사용

  ```jsx
  const set = new Set();
  console.log(set); // Set(0) {}

  set.add(1);
  console.log(set); // Set(1) {1}

  // -----------------------------

  // add() - 새로운 요소가 추가된 Set 객체 반환 => 연속적으로 호출 가능
  set.add(1).add(2);
  console.log(set); // Set(2) {1, 2}

  // But, Set 객체에 중복된 요소 추가는 허용되지 않음 (에러X, 무시)
  set.add(1).add(2).add(2);
  console.log(set); // Set(2) {1, 2}

  // -----------------------------

  console.log(NaN === NaN); // false
  console.log(0 === -0); // true

  // Set 객체는 NaN 간의 비교는 같다고 평가 -> 중복 추가 허용 X
  set.add(NaN).add(NaN);
  console.log(set); // Set(1) {NaN}

  // +0, -0 같다고 평가 -> 중복 추가 허용 X
  set.add(0).add(-0);
  console.log(set); // Set(2) {NaN, 0}
  ```

- Set 객체는 객체나 배열과 같이 JS 모든 값을 요소로 저장 가능

<br/>

### 🔸 37.1.4\_요소 존재 여부 확인

- `Set.prototype.has()`: Set 객체에 특정 요소 존재하는지 확인

  - `has()`: 요소의 존재 여부를 나타내는 Boolean 값 반환

  ```jsx
  const set = new Set([1, 2, 3]);

  console.log(set.has(2)); // true
  console.log(set.has(4)); // false
  ```

<br/>

### 🔸 37.1.5\_요소 삭제

- `Set.prototype.delete()`: Set 객체 특정 요소 삭제

  - 삭제 성공 여부를 나타내는 Boolean 값 반환
  - 삭제하려는 요소값을 인수로 전달해야 함 ⇒ Set 객체는 순서에 의미가 없음 (인덱스를 갖지 않음)

  ```jsx
  const set = new Set([1, 2, 3]);

  set.delete(2); // 요소 2 삭제
  console.log(set); // Set(2) {1, 3}

  set.delete(1); // 요소 1 삭제
  console.log(set); // Set(1) {3}

  // ------------------------------------

  // IF 존재하지 않는 Set 객체 요소 삭제하려 하면 에러 없이 무시됨
  set.delete(0);
  console.log(set); // Set(3) {1, 2, 3}

  // ------------------------------------

  // delete() - 삭제 성공 여부를 나타내는 Boolean 값 반환 => 연속적으로 호출 불가
  set.delete(1).delete(2); // TypeError
  ```

<br/>

### 🔸 37.1.6\_요소 일괄 삭제

- `Set.prototype.clear()`: Set 객체의 모든 요소 일괄 삭제
  - `clear()`: 항상 undefined 반환
  ```jsx
  set.clear();
  console.log(set); // Set(0) {}
  ```

<br/>

### 🔸 37.1.7\_요소 순회

- `Set.prototype.forEach()`: Set 객체 요소 순회

  - 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달
  - 콜백 함수는 3개의 인수를 전달받게 되는 것

    - 1, 2번째 인수 - 현재 순회 중인 요소값
    - 3번째 인수 - 현재 순회 중인 Set 객체 자체

    ```jsx
    const set = new Set([1, 2, 3]);

    set.forEach((v, v2, set) => console.log(v, v2, set));

    /*
    1 1 Set(3) {1, 2, 3}
    2 2 Set(3) {1, 2, 3}
    3 3 Set(3) {1, 2, 3}
    */
    ```

- Set 객체 = 이터러블 ⇒ for…of문으로 순회, 스프레드 문법, 디스트럭처링 대상 가능

  ```jsx
  const set = new Set([1, 2, 3]);

  console.log(Symbol.iterator in set); // true

  for (const value of set) {
    console.log(value); // 1 2 3
  }

  console.log([...set]); // [1, 2, 3]

  const [a, ...rest] = set;
  console.log(a, rest); // 1, [2, 3]
  ```

  ✋ Set 객체는 요소 순서에 의미를 갖지 않지만, Set 객체를 순회하는 순서는 요소가 추가된 순서를 따름 <br/>
  ⌙ why? 다른 이터러블의 순회와 호환성을 유지하기 위함

<br/>

### 🔸 37.1.8\_집합 연산

- Set 객체는 수학적 집합을 구현하기 위한 자료구조 <br/>
  ⇒ Set 객체를 통해 교집합, 합집합, 차집합 등 구현 가능

#### ‣ 교집합

- 집합 A와 B의 공통 요소로 구성

```jsx
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    if (this.has(value)) result.add(value);
  }

  return result;
};

// ===

Set.prototype.intersection = function (set) {
  return new Set([...this].filter((v) => set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.intersection(setB)); // Set(2) {2, 4}
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

#### ‣ 합집합

- 집합 A와 B의 중복 없는 모든 요소로 구성

```jsx
Set.prototype.union = function (set) {
  const result = new Set(this); // this(Set 객체) 복사

  for (const value of set) {
    // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합 (중복된 요소는 포함 X)
    result.add(value);
  }

  return result;
};

// ===

Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

#### ‣ 차집합

- `A - B` : 집합 A에는 존재하지만 집합 B에는 존재하지 않는 요소로 구성

```jsx
Set.prototype.difference = function (set) {
  const result = new Set(this); // this(Set 객체) 복사

  for (const value of set) {
    // 차집합은 어느 한쪽 집합엔 존재하지만 다른 한쪽 집합엔 존재하지 않는 요소로 구성된 집합
    result.add(value);
  }

  return result;
};

// ===

Set.prototype.difference = function (set) {
  return new Set([...this].filter((v) => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.difference(setB)); // Set(2) {1, 3}
console.log(setB.difference(setA)); // Set(0) {}
```

#### ‣ 부분 집합 & 상위 집합

- 집합 A가 집합 B에 포함되는 경우
  - 집합 A는 집합 B의 부분 집합(subset)
  - 집합 B는 집합 A의 상위 집합(superset)

```jsx
// this가 subset의 상위 집합인지 확인
Set.prototype.isSuperset = function (subset) {
  for (const value of set) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if (!this.has(value)) return false;
  }

  return true;
};

// ===

Set.prototype.isSuperset = function (set) {
  const supersetArr = [...this];
  return [...subset].every((v) => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.isSuperset(setB)); // true (setA이 setB의 상위 집합인지 확인)
console.log(setB.isSuperset(setA)); // false (setB가 setA의 상위 집합인지 확인)
```

<br/>

## ✅ 37.2_Map

- Map 객체: 키와 값의 쌍으로 이루어진 컬렉션

|                        | 객체                      | Map 객체              |
| ---------------------- | ------------------------- | --------------------- |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값       | 객체를 포함한 모든 값 |
| 이터러블               | ❌                        | ⭕️                   |
| 요소 개수 확인         | `Object.keys(obj).length` | `map.size`            |

<br/>

### 🔸 37.2.1_Map 객체의 생성

- Map 객체는 Map 생성자 함수로 생성 (인수 미전달 → 빈 Map 객체 생성)

  ```jsx
  const map = new Map();
  console.log(map); // Map(0) {}
  ```

- Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체 생성 <br/>
  (✋ 이터러블은 키-값 쌍으로 이루어진 요소로 구성되어야 함)

      ```jsx
      const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
      console.log(map1); // Map(2) {'key1' => 'value1', 'key2' => 'value2'}

      const map2 = new Map([1, 2]); // TypeError
      ```

- Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써짐 <br/>
  ⇒ Map 객체엔 중복된 키를 갖는 요소 존재 불가
  ```jsx
  const map = new Map([
    ["key1", "value1"],
    ["key1", "value2"],
  ]);
  console.log(map); // Map() {'key1' => 'value2'}
  ```

<br/>

### 🔸 37.2.2\_요소 개수 확인

- `Map.prototype.size()`: Map 객체의 요소 개수 확인

  ```jsx
  const { size } = new Map([
    ["key1", "value1"],
    ["key2", "value2"],
  ]);
  console.log(size); // 2

  // size 프로퍼티 = setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티
  // -> 숫자 할당하여 Map 객체 요소 개수 변경 불가
  size = 10; // 무시됨
  console.log(size); // 2
  ```

<br/>

### 🔸 37.2.3\_요소 추가

- `Map.prototype.set()`: Map 객체에 요소 추가

  ```jsx
  const map = new Map();
  console.log(map); // Map(0) {}

  map.set("key1", "value1");
  console.log(map); // Map(1) {'key1' => 'value1'}
  ```

- set 메서드는 새로운 요소가 추가된 Map 객체 반환 <br/>
  ⇒ set 메서드를 호출한 후에 set 메서드를 연속적으로 호출 가능

      ```jsx
      const map = new Map();

      map.set('key1', 'value1').set('key2', 'value2');

      console.log(map); // Map(2) {'key1' => 'value1', 'key2' => 'value2'}

      // ---------------------------------------------

      // Map 객체에는 중복된 키를 갖는 요소 존재 불가
      // => 중복된 키를 갖는 요소 추가시 값이 덮어써짐 (에러 발생 X)

      map.set('key1', 'value1').set('key1', 'value2');

      console.log(map); // Map(1) {'key1' => 'value2'}

      // -----------------------------

      console.log(NaN === NaN); // false
      console.log(0 === -0); // true

      // Set 객체는 NaN 간의 비교는 같다고 평가 -> 중복 추가 허용 X
      map.set(NaN, 'value1').set(NaN, 'value2');
      console.log(map); // Map(1) { NaN => 'value2' }

      // +0, -0 같다고 평가 -> 중복 추가 허용 X
      map.set(0, 'value1').set(-0, 'value2');
      console.log(map); // Map(2) { NaN => 'value2', 0 => 'value2' }
      ```

- Map 객체는 키 타입에 제한 없음 (객체는 문자열 또는 심벌 값만 키로 사용 가능) <br/>
  ⇒ 객체를 포함한 모든 값을 키로 사용 가능

  ```jsx
  const map = new Map();

  const lee = { name: "Lee" };
  const kim = { name: "Kim" };

  // 객체도 키로 사용 가능
  map.set(lee, "developer").set(kim, "designer");
  console.log(map); // Map(2) { {name: 'Lee'} => 'developer', {name: 'Kim'} => 'designer' }
  ```

<br/>

### 🔸 37.2.4\_요소 취득

- `Map.prototype.get()`: Map 객체에서 특정 요소 취득

  - get 메서드의 인수로 키를 전달하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환 <br/>
    (키를 갖는 요소가 존재하지 않으면 `undefined` 반환)

  ```jsx
  const map = new Map();

  const lee = { name: 'Lee' };
  const kim = { name: 'Kim' };

  map.set(lee, 'developer').set(kim, 'designer');

  console.log(map.get(lee)); // developer
  console.log(map.get('key')); // undefined

  Are you learning English these days?
  I just started. I think it’ll help with my future.
  It’ll probably take a while before you get fluent.
  Now that you mention it, I’m getting cold feet.
  No worries. They say “Well begun is half done.”
  I go along with that.
  ```

<br/>

### 🔸 37.2.5\_요소 존재 여부 확인

- `Map.prototype.has()`: Map 객체에 특정 요소가 존재하는지 확인

  - has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값 반환

  ```jsx
  const lee = { name: "Lee" };
  const kim = { name: "Kim" };

  const map = new Map([
    [lee, "developer"],
    [kim, "designer"],
  ]);

  console.log(map.has(lee)); // true
  console.log(map.has("key")); // false
  ```

<br/>

### 🔸 37.2.6\_요소 삭제

- `Map.prototype.delete()`: 객체 요소 삭제

  - delete 메서드는 삭제 성공 여부를 나타내는 불리언 값 반환

  ```jsx
  map.delete(kim);
  console.log(map); // Map(1) { {name: 'Lee'} => 'developer' }

  // IF 존재하지 않는 키로 Map 객체 요소 삭제하려 하면 에러 없이 무시됨
  const map = new Map([["key1", "value1"]]);

  map.delete("key2");
  console.log(map); // Map(1) {'key1' => 'value1'}
  ```

- 삭제 성공 여부를 나타내는 불리언 값 반환 ⇒ 연속적으로 호출 불가

  ```jsx
  const lee = { name: "Lee" };
  const kim = { name: "Kim" };

  const map = new Map([
    [lee, "developer"],
    [kim, "designer"],
  ]);

  map.delete(lee).delete(kim); // TypeError
  ```

<br/>

### 🔸 37.2.7\_요소 일괄 삭제

- `Map.prototype.clear()`: Map 객체 요소 일괄 삭제 (항상 `undefined` 반환)

  ```jsx
  const lee = { name: "Lee" };
  const kim = { name: "Kim" };

  const map = new Map([
    [lee, "developer"],
    [kim, "designer"],
  ]);

  map.clear();
  console.log(map); // Map(0) {}
  ```

<br/>

### 🔸 37.2.8\_요소 순회

- `Map.prototype.forEach()`: Map 객체 요소 순회

  - 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달
  - 콜백 함수는 3개의 인수를 전달받게 되는 것

    - 1번째 인수 - 현재 순회 중인 요소값
    - 2번째 인수 - 현재 순회 중인 요소키
    - 3번째 인수 - 현재 순회 중인 Map 객체 자체

    ```jsx
    const lee = { name: "Lee" };
    const kim = { name: "Kim" };

    const map = new Map([
      [lee, "developer"],
      [kim, "designer"],
    ]);

    map.forEach((v, k, map) => console.log(v, k, map));

    /*
    developer {name: 'Lee'} Map(2) {
    	{name: 'Lee'} => 'developer',
    	{name: 'Kim'} => 'designer'
    }
    developer {name: 'Kim'} Map(2) {
    	{name: 'Lee'} => 'developer',
    	{name: 'Kim'} => 'designer'
    }
    */
    ```

- Map 객체 = 이터러블 ⇒ for…of문으로 순회, 스프레드 문법, 디스트럭처링 대상 가능

  ```jsx
  const lee = { name: "Lee" };
  const kim = { name: "Kim" };

  const map = new Map([
    [lee, "developer"],
    [kim, "designer"],
  ]);

  // Map 객체는 Map.prototype의 Symbol.iterator 메서드를 상속받는 이터러블
  console.log(Symbol.iterator in map); // true

  for (const entry of map) {
    console.log(entry); // [{name: 'Lee'}, 'developer'] [{name: 'Kim'}, 'designer']
  }

  console.log([...map]); // [[{name: 'Lee'}, 'developer'], [{name: 'Kim'}, 'designer']]

  const [a, b] = map;
  console.log(a, b); // [{name: 'Lee'}, 'developer'] [{name: 'Kim'}, 'designer']
  ```

- Map 객체는 이터러블 && 이터레이터인 객체 반환 메서드 제공

  - `Map.prototype.keys`: Map 객체에서 요소키를 값으로 갖는 이터러블 && 이터레이터인 객체 반환
  - `Map.prototype.values`: Map 객체에서 요소값을 값으로 갖는 이터러블 && 이터레이터인 객체 반환
  - `Map.prototype.entries`: Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블 && 이터레이터인 객체 반환

  ```jsx
  const lee = { name: "Lee" };
  const kim = { name: "Kim" };

  const map = new Map([
    [lee, "developer"],
    [kim, "designer"],
  ]);

  for (const key of map.keys()) {
    console.log(key); // {name: 'Lee'} {name: 'Kim'}
  }

  for (const value of map.values()) {
    console.log(value); // developer designer
  }

  for (const entry of map.entries()) {
    console.log(entry); // [{name: 'Lee'}, 'developer'] [{name: 'Kim'}, 'designer']
  }
  ```

  ✋ Map 객체는 요소 순서에 의미를 갖지 않지만, Map 객체를 순회하는 순서는 요소가 추가된 순서를 따름 <br/>
  ⌙ why? 다른 이터러블의 순회와 호환성을 유지하기 위함
