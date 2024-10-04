# ☑️ 25\_클래스

p.417~468

✍️ 2024.09.19(Thurs)

## ✅ 25.1\_클래스는 프로토타입의 문법적 설탕인가?

- JS는 프로토타입 기반 객체지향 언어 (클래스 필요 없음)

🍭 클래스는 함수이며, 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 문법적 설탕 느낌 ?

👀 클래스, 생성자 함수 모두 프로토타입 기반 인스턴스 생성하지만, 정확히 동일하게 동작하진 않음!

<br/>

|                            | 클래스                                                                                                           | 생성자 함수                                                                             |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| new 연산자 없이 호출       | 에러 발생                                                                                                        | 일반 함수로서 호출                                                                      |
| extends, super 지원 유무   | ⭕️                                                                                                              | ❌                                                                                      |
| 호이스팅 발생 유무         | ❌ (발생하지 않는 것처럼 동작)                                                                                   | ⭕️ <br/>-함수 선언문으로 정의: 함수 호이스팅<br/> -함수 표현식으로 정의: 변수 호이스팅 |
| strict mode 해제 가능 유무 | ❌ (해제 불가)                                                                                                   | ⭕️ (암묵적으로 지정하지 않음)                                                          |
| -                          | constructor, 프로토타입 메서드, 정적 메서드 모두 프로퍼티 어튜리뷰트 `[[Enumerable]]` 값이 false ⇒ 열거되지 않음 | -                                                                                       |

⇒ 클래스는 새로운 객체 생성 매커니즘 !

<br/>

## ✅ 25.2\_클래스 정의

- `class` 키워드 사용하여 정의
- 일반적으로 `Pascal case` 사용 (사용하지 않아도 에러 발생하진 않음)

  ```jsx
  // 클래스 선언문
  class Person {}
  ```

- 표현식으로 클래스 정의 가능

  ```jsx
  // 익명 클래스 표현식
  const Person = class {};

  // 기명 클래스 표현식
  const Person = class MyClass {};
  ```

  - 클래스가 값으로 사용 가능한 일급 객체임을 의미
    1. 무명의 리터럴로 생성 가능 ⇒ 런타임에 생성 가능
    2. 변수나 자료구조에 저장 가능
    3. 함수의 매개변수에게 전달 가능
    4. 함수의 반환값으로 사용 가능

- 클래스 몸체에서 정의 가능한 메서드

  1. constructor(생성자)
  2. 프로토타입 메서드
  3. 정적 메서드

  ```jsx
  // 클래스 선언문
  class Person {
    // 1_생성자
    constructor(name) {
      // 인스턴스 생성 및 초기화
      this.name = name; // name 프로퍼티는 public
    }

    // 2_프로토타입 메서드
    sayHi() {
      console.log(`Hi! My name is ${this.name}`);
    }

    // 3_정적 메서드
    static sayHello() {
      console.log(`Hello!`);
    }
  }
  ```

<br/>

## ✅ 25.3\_클래스 호이스팅

- 클래스는 함수로 평가됨

  ```jsx
  class Person {} // 소스코드 평가 과정 즉, 런타임 이전에 먼저 평가 -> 함수 객체 생성
  // 생성된 함수 객체 = 생성자 함수로서 호출 가능한 함수 = constructor
  // 이때, 프로토타입도 생성되는데 이는 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 쌍으로 존재하기 때문!

  console.log(typeof Person); // function
  ```

```jsx
// 클래스는 클래스 정의 이전에 참조 불가
console.log(Person); // ReferenceError

class Person {}
```

```jsx
const Person = "";

{
  // If 호이스팅이 발생하지 않는다면 -> '' 출력되어야 함
  console.log(Person); // ReferenceError

  // 클래스 선언문 이전에 일시적 사각지대(TDZ)에 빠져 호이스팅 발생하지 않는 것처럼 동작함

  // 클래스 선언문 -> 호이스팅 발생함
  class Person {}
}
```

<br/>

## ✅ 25.4\_인스턴스 생성

- 클래스는 생성자 함수이며, new 연산자와 함께 호출되어 인스턴스를 생성함

  ```jsx
  class Person {}

  // 인스턴스 생성
  const me = new Person(); // 반드시 new 연산자와 함께 호출해야 함!
  console.log(me); // Person {}
  ```

```jsx
const Person = class MyClass {};

// 클래스를 가리키는 식별자로 인스턴스 생성해야 함
const me = new Person();

// 클래스 이름 MyClass는 클래스 몸체 내부에서만 유효한 식별자 !
// => 클래스 표현식에서 사용한 클래스 이름은 외부 코드에서 접근 불가 !
console.log(MyClass); // ReferenceError

const you = new MyClass(); // ReferenceError
```

<br/>

## ✅ 25.5\_메서드

### 🔸 25.5.1_constructor

- `constructor`: 인스턴스를 생성하고 초기화하기 위한 특수한 메서드로, 이름 변경 불가

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name; // 인스턴스 프로퍼티
  }
}
```

- 프로토타입 객체의 constructor 프로퍼티는 클래스 자신을 가리킴
  → 클래스가 인스턴스를 생성하는 생성자 함수를 의미
  ⇒ new 연산자와 함께 클래스 호출하면 클래스는 인스턴스 생성

- `constructor`가 생성자 함수와 다른 점

  1. `constructor`는 클래스 내 최대 1개만 존재 가능 (2개 이상 존재시 SyntaxError)

     ```jsx
     class Person. {
     	constructor() {}
     	constructor() {}
     }
     // SyntaxError
     ```

  2. `constructor` 생략 가능

     ```jsx
     class Person {
       // constructor 생략 -> 빈 constructor가 암묵적으로 정의
       constructor() {}
     }

     // 빈 객체 생성
     const me = new Person();
     console.log(me); // Person {}
     ```

  3. 프로퍼티가 추가되어 초기화된 인스턴스 생성하려면 constructor 내부에서 this에 인스턴스 프로터피 추가

     ```jsx
     class Person {
       constructor() {
         // 고정값으로 인스턴스 초기화
         this.name = "Choi";
         this.address = "Seoul";
       }
     }

     // 인스턴스 프로퍼티 추가
     const me = new Person();
     console.log(me); // Person {name: 'Choi', address: 'Seoul'};
     ```

  4. 인스턴스 생성시 클래스 외부에서 인스턴스 프로퍼티 초기값 전달하려면 매개변수 선언 후 초기값 전달

     ```jsx
     class Person {
       constructor(name, address) {
         // 인수로 인스턴스 초기화
         this.name = name;
         this.address = address;
       }
     }

     // 인수로 초기값 전달 -> 초기값은 constructor에 전달됨
     const me = new Person("Choi", "Seoul");
     console.log(me); // Person {name: 'Choi', address: 'Seoul'};
     ```

     - 인스턴스 초기화하려면 constructor 생략 ❌

  5. constructor는 별도의 반환문을 갖지 않아야 함 (constructor 내부에서 return문 반드시 생략하기!)
     - new 연산자와 함께 클래스가 호출되면 암묵적으로 this, 즉 인스턴스를 반환하기 때문
       - IF 명시적으로 객체를 반환하면 → 암묵적인 this 반환 무시
       - IF 명시적으로 원시값 반환하면 → 원시값 반환 무시되고, 암묵적으로 this 반환

<br/>

### 🔸 25.5.2\_프로토타입 메서드

- 클래스 몸체에서 정의한 메서드는 클래스의 `prototype` 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 됨

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

// ------

class Person {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    console.log(`My name is ${this.name}`);
  }
}

// -------

// 클래스가 생성한 인스턴스는 프로토타입 체인의 일원!

// me 객체의 프로토타입 = Person.prototype
Object.getPrototypeOf(me) === Person.prototype; // true
me instanceof Person; // true

// Person.prototype의 프로토타입 = Object.prototype
Object.getPrototypeOf(Person.prototype) === Object.prototype; // true
me instanceof Object; // true

// me 객체의 constructor = Person 클래스
me.constructor === Person; // true
```

- 클래스 몸체에서 정의한 메서드는 인스턴스의 프로토타입에 존재하는 프로토타입 메서드가 됨
- 인스턴스는 프로토타입 메서드를 상속받아 사용 가능

⇒ 클래스 = 인스턴스를 생성하는 생성자 함수 = 프로토타입 기반의 객체 생성 매커니즘

<br/>

### 🔸 25.5.3\_정적 메서드

- 정적(static) 메서드: 인스턴스를 생성하지 않아도 호출할 수 있는 메서드

1. 생성자 함수의 정적 메서드

   ```jsx
   // 생성자 함수
   function Person(name) {
     this.name = name;
   }

   // 정적 메서드
   Person.sayHi = function () {
     console.log("Hi!");
   };

   // 정적 메서드 호출
   Person.sayHi(); // Hi!
   ```

2. 클래스의 정적 메서드

   - `static` + 메서드

   ```jsx
   // 클래스
   class Person {
     // 생성자
     constructor(name) {
       this.name = name;
     }

     // 정적 메서드
     static sayHi() {
       console.log("Hi!");
     }
   }
   ```

- 정적 메서드는 클래스에 바인딩된 메서드
- 클래스는 함수 객체로 평가 → 자신의 프로퍼티/메서드 소유 불가
- 클래스는 클래스 정의가 평가되는 시점에 함수 객체가 되므로 생성 과정 필요 없음 <br/>
  ⇒ 정적 메서드는 클래스 정의 이후 인스턴스 생성하지 않아도 호출 가능
- 정적 메서드는 인스턴스로 호출하지 않고 클래스로 호출

  ```jsx
  // 정적 메서드는 클래스로 호출
  // 정적 메서드는 인스턴스 없이도 호출 가능
  Person.sayHi(); // Hi!
  ```

- 정적 메서드는 인스턴스로 호출 불가 <br/>
  ⌙ why? 정적 메서드가 바인딩된 클래스는 인스턴스의 프로토타입 체인 상에 존재하지 않기 때문 <br/>
  ⇒ 인스턴스의 프로토타입 체인 상에는 클래스가 존재하지 않기 때문에 인스턴스로 클래스의 메서드 상속받을 수 없음

  ```jsx
  const me = new Person("Choi");
  me.sayHi(); // TypeError
  ```

<br/>

### 🔸 25.5.4\_정적 메서드와 프로토타입 메서드의 차이

‣ 정적 메서드와 프로토타입 메서드의 차이

- 정적 메서드 & 프로토타입 메서드 - 자신이 속해 있는 프로토타입 체인이 다름

|                                  | 정적 메서드   | 프로토타입 메서드 |
| -------------------------------- | ------------- | ----------------- |
| 호출 방법                        | 클래스로 호출 | 인스턴스로 호출   |
| 인스턴스 프로퍼티 참조 가능 여부 | ❌            | ⭕️               |

👀 IF 인스턴스 프로퍼티를 참조해야 한다면 정적 메서드 대신 프로토타입 메서드를 사용해야 함
<br/>

- 정적 메서드는 클래스로 호출해야 하므로 정적 메서드 내부의 this는 인스턴스가 아닌 클래스를 가리킴 <br/>
  ⇒ 프로토타입 메서드와 정적 메서드 내부의 this 바인딩이 다름
- 메서드 내부에서 인스턴스 프로퍼티를 참조할 필요가 있다면 this 사용해야 하며, 프로토타입 메서드로 정의해야 함 <br/>
  (But, 인스턴스 프로퍼티 참조해야 할 필요가 없다면 this 사용 X)
- this 사용하지 않는 메서드는 정적 메서드로 정의하는 것이 좋음 <br/>
  ⌙ why? 반드시 인스턴스를 생성한 다음 인스턴스로 호출해야 하므로

👍 정적 메서드는 애플리케이션 전역에서 사용할 유틸리티 함수를 전역 함수로 정의하지 않고 메서드로 구조화할 때 유용함

<br/>

### 🔸 25.5.5\_클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약 표현 사용
2. 클래스에 메서드 정의시 콤마(`,`) 필요 없음
3. 암묵적으로 strict mode로 실행됨
4. `for...in`, `Object.keys` 메서드 등으로 열거 불가
5. `non-constructor` (내부 메서드 [[Construct]]를 갖지 않음) ⇒ new 연산자와 함께 호출 불가

<br/>

## ✅ 25.6\_클래스의 인스턴스 생성 과정

- new 연산자와 함께 클래스 호출시 클래스 내부 메서드 [[Constructor]] 호출됨 <br/>
  ⇒ 클래스는 new 연산자 없이 호출 불가

### ‣ 인스턴스 생성 과정

1. 인스턴스 생성과 this 바인딩
   1. new 연산자와 함께 클래스 호출시 constructor 내부 코드 실행 전 암묵적으로 빈 객체가 생성됨 <br/>
      = 클래스가 생성한 인스턴스
   2. 클래스가 생성한 인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가르키는 객체가 설정됨
   3. 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩
   4. constructor 내부의 this는 클래스가 생성한 인스턴스를 가리킴
2. 인스턴스 초기화
   1. constructor 내부 코드 실행 → this에 바인딩되어 있는 인스턴스 초기화
   2. this에 바인딩되어 있는 인스턴스에 프로퍼티 추가
   3. constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티값을 초기화
3. 인스턴스 반환

   1. 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환됨

   ```jsx
   class Person {
     // 생성자
     constructor(name) {
       // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩됨
       console.log(this); // Person {}
       console.log(Object.getPrototypeOf(this) === Person.prototype); // true

       // 2. this에 바인딩되어 있는 인스턴스 초기화
       this.name = name;

       // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환됨
     }
   }
   ```

<br/>

## ✅ 25.7\_프로퍼티

### 🔸 25.7.1\_인스턴스 프로퍼티

- 인스턴스 프로퍼티는 `constructor` 내부에서 정의해야 함

```jsx
class Person {
  constructor(name) {
    // 인스턴스 프로퍼티
    this.name = name; // public
  }
}
```

✋ ES6 클래스는 접근 제한자(e.g. private, public, protected) 지원 ❌ ⇒ 인스턴스 프로퍼티는 언제나 `public`

<br/>

### 🔸 25.7.2\_접근자 프로퍼티

- `접근자 프로퍼티`(`accessor property`): 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로, `getter` 함수와 `setter` 함수로 구성되어 있음
- 접근자 프로퍼티는 클래스에서도 사용 가능

- `getter` : 인스턴스 프로퍼티에 접근할 때 사용 (get + 메서드명)
- `setter` : 인스턴스 프로퍼티에 값을 할당할 때 사용 (set + 메서드명)

- `getter` & `setter` 이름은 인스턴스 프로퍼티처럼 사용됨
  - `getter`: 호출하는 것이 아니라 프로퍼티처럼 참조하는 형식으로 사용
    - 반드시 무언가 반환해야 함
  - `setter`: 호출하는 것이 아니라 프로퍼티처럼 값을 할당하는 형식으로 사용
    - 반드시 매개변수 있어야 함 (단 하나의 값만 할당받으므로 단 하나의 매개변수만 선언 가능)

⇒ 클래스의 접근자 프로퍼티 또한 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티가 됨

<br/>

### 🔸 25.7.3\_클래스 필드 정의 제안

- `클래스 필드`: 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어

- 클래스 몸체에서 클래스 필드를 정의하는 경우 this에 클래스 필드 바인딩해선 안됨

  - this는 클래스의 constructor와 메서드 내에서만 유효

    ```jsx
    class Person {
      this.name = ''; // SyntaxError
    }
    ```

- 클래스 필드를 참조하는 경우 JS에선 this를 반드시 사용해야 함

  ```jsx
  class Person {
    name = "Choi"; // 클래스 필드

    constructor() {
      console.log(name); // ReferenceError
    }
  }

  new Person();
  ```

- 클래스 필드에 초기값 할당하지 않으면 `undefined`

  ```jsx
  class Person {
    name;
  }

  const me = new Person();
  console.log(me); // Person {name: undefined}
  ```

- 인스턴스 생성시 외부 초기값으로 클래스 필드를 초기화해야 한다면 `constructor`에서 클래스 필드 초기화해야 함

  ```jsx
  class Person {
    name; // 클래스 필드 초기화할 필요가 있을 때 굳이 이렇게 정의할 필요가 없음
    // 어차피 constructor 내부에서 클래스 필드를 참조하여 초기값을 할당해야 함
    // this, 즉 클래스가 생성한 인스턴스에 클래스 필드에 해당하는 프로퍼티가 없다면 자동 추가되기 때문

    constructor(name) {
      this.name = name; // 클래스 필드 초기화
    }
  }
  ```

- 함수는 일급 객체이므로 함수를 클래스 필드에 할당할 수 있음 ⇒ 클래스 필드를 통해 메서드 정의 가능

  ```jsx
  class Person {
    name = "Choi"; // 클래스 필드에 문자열 할당

    // 클래스 필드에 함수를 할당
    getName = function () {
      return this.name;
    };

    // 화살표 함수로도 정의 가능
    // getName = () => this.name;
  }
  ```

👀 클래스 필드에 화살표 함수를 할당하여 화살표 함수 내부의 this가 인스턴스를 가리키게 하는 경우도 있음

<br/>

### 🔸 25.7.4_private 필드 정의 제안

- JS는 캡슐화를 완전하게 지원 X→ 인스턴스 프로퍼티는 인스턴스를 통해 클래스 외부에서 언제나 참조 가능 → 언제나 public
- 클래스 필드 정의 제안을 사용하더라도 클래스 필드는 기본적으로 public → 외부에 그대로 노출

#### ‣ private 필드 정의

```jsx
class Person {
  // private 필드 정의
  #name = "";

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }
}

const me = new Person("Choi");

// private 필드 #name은 클래스 외부에서 참조 X
console.log(me.#name); // SyntaxError
```

- public 필드는 어디서든 참조 가능 / private 필드는 클래스 내부에서만 참조 가능

  | 접근 가능성                 | public | private |
  | --------------------------- | ------ | ------- |
  | 클래스 내부                 | ⭕️    | ⭕️     |
  | 자식 클래스 내부            | ⭕️    | ❌      |
  | 클래스 인스턴스를 통한 접근 | ⭕️    | ❌      |

- 클래스 외부에서 private 필드에 직접 접근할 순 없지만, 접근자 프로퍼티를 통해 간접적으로 접근 가능

  ```jsx
  class Person {
    // private 필드 정의
    #name = "";

    constructor(name) {
      this.#name = name;
    }

    // name - 접근자 프로퍼티
    get name() {
      // private 필드를 참조하여 trim 후 반환
      return this.#name.trim();
    }
  }

  const me = new Person("Choi");
  console.log(me.#name); // Choi
  ```

<br/>

### 🔸 25.7.5_static 필드 정의 제안

- static로 정적 메서드 정의는 가능하지만 정적 필드는 정의할 수 없었지만 현재 static public, static private 정의 가능한 새로운 표준 사양이 제안되어 있음

  ```jsx
  class MyMath {
    // static public 필드 정의
    static PI = 22 / 7;

    // static private 필드 정의
    static #num = 10;

    // static 메서드
    static increment() {
      return ++MyMath.#num;
    }
  }

  console.log(MyMath.PI); // 3.14..
  console.log(MyMath.increment()); // 11
  ```

<br/>

## ✅ 25.8\_상속에 의한 클래스 확장

### 🔸 25.8.1\_클래스 상속과 생성자 함수 상속

- 상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것

- 클래스는 상속을 통해 기존 클래스를 확장할 수 있는 문법이 제공되지만 생성자 함수는 아님

```jsx
class Animal {
  constructor(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  eat() {
    return "eat";
  }

  move() {
    return "move";
  }
}

class Bird extends Animal {
  fly() {
    return "fly";
  }
}

const bird = new Bird(1, 5);

console.log(bird); // Bird { age: 1, weight: 5 }
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true
```

### 🔸 25.8.2_extends 키워드

- 상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스 정의

  - `extends` 역할: super class & sub class 간의 상속 관계 설정
  - 클래스도 프로토타입을 통해 상속 관계 구현
  - 프로토타입 메서드, 정적 메서드 모두 상속 가능

  ```jsx
  // super(base/parent) class
  // 서브클래스에게 상속된 클래스
  class Base {}

  // sub(derived/child) class
  // 상속을 통해 확장된 클래스
  class Derived extends Base {}
  ```

<br/>

### 🔸 25.8.3\_동적 상속

- extends는 생성자 함수를 상속받아 클래스 확장도 가능 <br/>
  ✋ extends 키워드 앞에 반드시 클래스가 와야 함!

<br/>

### 🔸 25.8.4\_서브클래스의 constructor

- 클래스에서 constructor를 생략하면 클래스에 비어있는 constructor가 암묵적으로 정의됨 <br/>
  `constructor() {}`
- 서브클래스에서 constructor를 생략하면 아래와 같은 constructor가 암묵적으로 정의됨 <br/>
  `constructor(...args) { super(...args); }`
  - `super()`: super class의 `constructor`를 호출하여 인스턴스 생성

```jsx
// constructor 생략
class Base {} // super class
class Derived extends Base {} // sub class

// --->

class Base {
  constructor() {}
}

class Derived extends Base {
  constructor(...args) {
    super(...args);
  }
}

const derived = new Derived();
console.log(derived); // Derived {}
```

<br/>

### 🔸 25.8.5_super 키워드

#### ‣ super 호출

- super 호출하면 슈퍼클래스의 constructor 호출

  - 슈퍼클래스의 constructor 내부에서 추가한 프로퍼티를 그대로 갖는 인스턴스를 생성한다면 서브클래스의 constructor 생략 가능 <br/>
    → 서브클래스에 암묵적으로 정의된 constructor의 super 호출을 통해 슈퍼클래스의 constructor에 전달
  - 슈퍼클래스에서 추가한 프로퍼티와 서브클래스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브클래스의 constructor 생략 불가 ❌<br/>
    → 슈퍼클래스 constructor에 전달할 필요가 있는 인수는 서브클래스의 constructor에서 호출하는 super를 통해 전달

    ```jsx
    class Base {
      constructor(a, b) {
        this.a = a;
        this.b = b;
      }
    }

    class Derived extends Base {
      constructor(a, b, c) {
        super(a, b); // Base 클래스의 constructor에 일부 전달
        this.c = c;
      }
    }

    const derived = new Derived(1, 2, 3);
    console.log(derived); // Derived {a: 1, b: 2, c: 3}

    // => 인스턴스 초기화를 위해 전달한 인수는 슈퍼클래스와 서브클래스에 배분되고
    // 상속 관계의 두 클래스는 서로 협력하여 인스턴스 생성
    ```

  - super 호출 주의사항
    1. 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에선 반드시 super 호출해야 함
    2. 서브클래스의 constructor에서 super 호출 전엔 this 참조 불가
    3. super는 반드시 서브클래스 constructor에서만 호출한다.
       서브클래스가 아닌 클래스의 constructor나 함수에서 super 호출시 에러 발생

#### ‣ super 참조

- super 참조하면 슈퍼클래스의 메서드 호출 가능
  - 서브클래스의 프로토타입 메서드 내에서 `super.sayHi`는 슈퍼클래스의 프로토타입 메서드 sayHi를 가리킴
  - super 참조를 통해 슈퍼클래스 메서드를 참조하려면 super가 슈퍼클래스의 메서드가 바인딩되 객체, 즉 슈퍼클래스의 prototype 프로퍼티에 바인딩된 프로토타입을 참조할 수 있어야 함
- super는 자신을 참조하고 있는 메서드가 바인딩되어 있는 객체의 프로토타입을 가리킴
- [[HomeObject]]를 가지는 함수만이 super 참조 가능<br/>
  ⇒ ES6의 메서드 축약 표현으로 정의된 함수만이 super 참조 가능 <br/>
  (super 참조는 슈퍼클래스의 메서드를 참조하기 위해 사용하므로 서브클래스의 메서드에서 사용해야 함)
- 서브클래스의 정적 메서드 내에서 super.sayHi는 슈퍼클래스의 정적 메서드 sayHi를 가리킴

<br/>

### 🔸 25.8.6\_상속 클래스의 인스턴스 생성 과정

1. 서브클래스의 super 호출
   - JS엔진은 클래스를 평가할 때 슈퍼클래스와 서브클래스를 구분하기 위해 `‘base’` 또는 `‘derived’`를 값으로 갖는 내부 슬롯 `[[ConstructorKind]]`를 갖음
     - `base`: 다른 클래스를 상속받지 않는 클래스의 내부 슬롯 값
     - `derived`: 다른 클래스를 상속받는 서브클래스의 내부 슬롯 값
   - 서브클래스는 자신이 직접 인스턴스를 생성하지 않고 슈퍼클래스에게 인스턴스 생성을 위임함 <br/>
     ⇒ 서브클래스의 constructor에서 반드시 super를 호출해야 하는 이유
2. 슈퍼클래스의 인스턴스 생성과 this 바인딩
   - 슈퍼클래스의 constructor 내부의 this는 생성된 인스턴스를 가리킴
3. 슈퍼클래스의 인스턴스 초기화
   - 슈퍼클래스의 constructor가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화함 <br/>
     ⇒ 즉, this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화함
4. 서브클래스 constructor로의 복귀와 this 바인딩
   - super 호출 종료 & 제어 흐름이 서브클래스 constructor로 돌아옴
   - super가 반환한 인스턴스가 this에 바인딩됨
   - 서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용함
   - 서브클래스 constructor 내부 인스턴스 초기화는 반드시 super 호출 이후에 처리되어야 함 <br/>
     ⌙ why? super가 호출되지 않으면 인스턴스가 생성되지 않고 this 바인딩도 할 수 없으므로
5. 서브클래스의 인스턴스 초기화
   - super 호출 이후, 서브클래스의 constructor에 기술되어 있는 인스턴스 초기화가 실행됨
   - this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 초기화함
6. 인스턴스 반환

   - 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환됨

   ```jsx
   class ColorRectangle extends Rectangle {
     constructor(width, height, color) {
       super(width, height);

       // super가 반환한 인스턴스가 this에 바인딩됨
       console.log(this); // ColorRectangle {width: 2, height: 4}

       // 인스턴스 초기화
       this.color = color;

       // 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환됨
       console.log(this); // ColorRectangle {width: 2, height: 4, color: 'red'}
     }
   }
   ```

<br/>

### 🔸 25.8.7\_표준 빌트인 생성자 함수 확장

```jsx
class MyArray extends Array {
  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

// Array.prototype의 메서드 중 map, filter와 같이 새로운 배열을 반환하는 메서드는
// MyArray 클래스의 인스턴스를 반환함

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray); // MyArray(4) [1, 1, 2, 3]

console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]

// IF Array 인스턴스를 반환하면 MyArray 클래스의 메서드와 메서드 체이닝 불가능!
// 메서드 체이닝
console.log(
  myArray
    .filter((v) => v % 2)
    .uniq()
    .average()
); // 2

// -----------

// IF MyArray 클래스의 uniq 메서드가 Array가 생성한 인스턴스를 반환하게 하려면
// Symbol.species 사용하여 정적 접근자 프로퍼티 추가

class MyArray extends Array {
  // 모든 메서드가 Array 타입의 인스턴스 반환하도록 함
  static get [Symbol.species]() {
    return Array;
  }
}

const myArray = new MyArray(1, 1, 2, 3);

console.log(myArray.uniq() instanceof MyArray); // false
console.log(myArray.uniq() instanceof Array); // true

// 메서드 체이닝 불가
console.log(myArray.uniq().average()); // TypeError
```
