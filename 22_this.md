# ☑️ 22. this

p.342~358

✍️ 2024.09.09(Mon)

## ✅ 22.1_this 키워드

- 객체: 프로퍼티(상태; state) + 메서드(동작; behavior)를 하나의 논리적인 단위로 묶은 복합적인 자료구조
- 메서드: 자식이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 함
  - 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 함

```jsx
const circle = {
  // 프로퍼티: 객체 고유 상태 데이터
  radius: 5,

  // 메서드: 상태 데이터를 참조하고 조작하는 동작
  getDiameter() {
    // 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
    // 자신이 속한 객체인 circle을 참조할 수 있어야 함
    return 2 * circle.radius;
  },
};

console.log(circle.getDiameter()); // 10
```

> - 생성자 함수에 의한 객체 생성 방식
>
> 1.  생성자 함수 정의
> 2.  new 연산자와 함께 생성자 함수 호출<br/>
>     ⇒ 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재해야 함

```jsx
// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 함
const circle = new Circle(5);
```

- 생성자 함수 정의 시점엔 인스턴스 생성 전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없음<br/>
  ⇒ 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자 필요 ⇒ `this`

- `this`: 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수

  - 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드 참조 가능
  - JS엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조 가능
  - 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달
  - 지역 변수처럼 사용 가능
  - this 바인딩(this가 가리키는 값)은 함수 호출 방식에 의해 동적으로 결정됨

- this 바인딩
  - 바인딩(binding): 식별자와 값을 연결하는 과정

```jsx
// 생성자 함수
function Circle(radius) {
  // this - 생성자 함수가 생성할 인스턴스
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // this - 생성자 함수가 생성할 인스턴스
  return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

```jsx
// this는 어디서든 참조 가능
// 전역에서 this는 전역 객체 window를 가리킴
console.log(this); // window

// 일반 함수 내부에서 this는 전역 객체 window를 가리킴
function square(number) {
	console.log(this); // window (전역 객체)
	return number * number;
}

square(2);

// ---------------------

// 메서드 내부에서 this는 메서드를 호출한 객체를 가리킴
conster person = {
	name: 'Choi',
	getName() {
		console.log(this); // {name: 'Choi', getName: f}
		return this.name;
	}
}

console.log(person.getName()); // Choi

// ----------------------

// 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킴
function Person(name) {
	this.name = name;
	console.log(this); // Person {name: 'Choi'}
}

const me = new Person('Choi');
```

👀 this는 자기 참조 변수이므로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미있음 <br/>
⇒ strict mode가 적용된 일반 함수 내부의 this에는 undefined 바인딩 (why? 일반 함수 내부에선 this 사용 필요 없으므로)

<br/>

## ✅ 22.2\_함수 호출 방식과 this 바인딩

💡 렉시컬 스코프 & this 바인딩 결정 시기 다름

- 함수의 상위 스코프 결정 방식인 렉시컬 스코프(lexical scope): 함수 정의가 평가되어 함수 객체 생성 시점에 상위 스코프 결정
- this 바인딩: 함수 호출 시점에 결정

```jsx
const foo = function () {
  console.dir(this);
};

// 다양한 방식으로 함수 호출 가능

// 1. 일반 함수 호출
foo(); // window

// 2. 메서드 호출
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
const bar = { name: "bar" };

foo.call(bar); // bar
foo.call(bar); // bar
foo.bind(bar)(); // bar
```

<br/>

### 🔸 22.2.1\_일반 함수 호출

- this에는 전역 객체가 바인딩
- 중첩 함수를 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩됨
- strict mode가 적용된 일반 함수 내부의 this엔 undefined가 바인딩됨

⇒ 어떠한 함수라도 일반 함수로 호출되면 this에 전역 객체 바인딩!

```jsx
var value = 1;

const obj = {
	value: 100,

	// 1번째 방법
	foo() {
		const that = this; // this 바인딩(obj)

		// 콜백 함수 this에는 전역 객체가 바인딩되므로 window.value(1)를 참조가게 됨
		// => this 대신 that을 참조하게 하기
		setTimeout(function () {
			console.log(that.value); // 100
		}, 100);
	}

	// 2번째 방법
	foo() {
		// 콜백 함수에 명시적으로 this 바인딩
		setTimeout(function () {
			console.log(this.value); // 100
		}.bind(this), 100);
	}

	// 3번째 방법
	foo() {
		// 화살표 함수 내부의 this는 상위 스코프 this를 가리킴
		setTimeout(() => console.log(this.value), 100); // 100
	}
};

obj.foo();
```

<br/>

### 🔸 22.2.2\_메서드 호출

- 메서드 호출시 메서드 이름 앞의 마침표 연산자 앞에 기술한 객체가 바인딩

✋ 메서드를 호출한 객체에 바인딩됨

```jsx
const person = {
  name: "Choi",

  // 메서드 - 프로퍼티에 바인딩된 함수
  // getName 프로퍼티가 가리키는 함수 객체 - 독립적으로 존재하는 별도의 객체
  getName() {
    // 메서드 내부 this - 메서드를 호출한 객체에 바인딩
    return this.name;
  },
};

// 메서드 getName을 호출한 객체: person
console.log(person.getName()); // Choi
```

```jsx
const anotherPerson = {
  name: "Kim",
};

// getName 메서드를 anotherPerosn 객체 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체: anotherPerson
console.log(anotherPerson.getName()); // Kim
```

<br/>

### 🔸 22.2.3\_생성자 함수 호출

- 생성자 함수 내부의 this에는 생성자 함수가 미래에 생성할 인스턴스가 바인딩

```jsx
// 생성자 함수
function Circle(radius) {
  // this - 생성자 함수가 생성할 인스턴스
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자와 함께 호출 -> 생성자 함수로 동작
// But, new 연산자 없이 생성자 함수 호출시 -> 일반 함수로 동작
const circle1 = new Circle(5); // 10
const circle2 = Circle(15); // undefined (반환문이 없으므로 undefined) -> Circle 내부 this: 전역 객체
```

<br/>

### 🔸 22.2.4_Function.prototype.apply/call/bind 메서드에 의한 간접 호출

- `apply`, `call`, `bind` 메서드: Function.prototype 메서드
  - 모든 함수가 해당 메서드 상속받아 사용 가능

```jsx
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

console.log(getThisBinding.apply(thisArg)); // {a: 1}

// apply 메서드: 호출할 함수의 인수를 배열로 묶어 전달
console.log(getThisBinding.apply(thisArg, [1, 2, 3])); // {a: 1}

// call 메서드: 호출할 함수의 인수를 쉼표로 구분한 리스트 형식으로 전달
console.log(getThisBinding.apply(thisArg, 1, 2, 3)); // {a: 1}

// => 인수 전달 방식만 다를 뿐 this로 사용할 객체를 전달하면서 함수를 호출하는 것은 동일!
```

- `arguments` 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우 대표적으로 apply, call 메서드 사용

```jsx
function convertArgsToArray() {
  console.log(arguments);

  // arguments 객체 -> 배열로 변환 (인수 없이 호출시 배열 복사본 생성)
  const arr = Array.prototype.slice.call(arguments);

  console.log(arr);

  return arr;
}

convertArgsToArray(1, 2, 3); // [1, 2, 3]
```

👀 `Function.prototype.bind` 메서드는 apply, call 메서드와 달리 함수 호출 X

- 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 사용됨

```jsx
function getThisBinding() {
  return this;
}

const thisArg = { a: 1 };

// thisArg로 this 바인딩이 교체된 getThisBinding 함수를 새롭게 생성해 반환
console.log(getThisBinding.bind(thisArg)); // getThisBinding

// bind는 함수를 호출하지 않으므로 명시적으로 호출해야 함
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

```jsx
const person = {
  name: "Choi",
  foo(callback) {
    // bind로 callback 함수 내부 this 바인딩 전달
    setTimeout(callback.bind(this), 100);
  },
};

person.foo(function () {
  console.log(this.name); // ''
});
```

<br/>

🧚‍♀️ 요약!

| 함수 호출 방식                                             | this 바인딩                                                           |
| ---------------------------------------------------------- | --------------------------------------------------------------------- |
| 일반 함수 호출                                             | 전역 객체                                                             |
| 메서드 호출                                                | 메서드를 호출한 객체                                                  |
| 생성자 함수 호출                                           | 생성자 함수가 생성할 인스턴스                                         |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에 첫번째 인수로 전달한 객체 |
