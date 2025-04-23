p.870~885

✍️ 2025.04.21(Mon)~04.23(Wed)

## ✅ 46.1\_제너레이터란?

- 제너레이터(generator): 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수

1. 제너레이터 함수는 함수 호출자에게 함수 실행 제어권 양도 가능
   - 일반 함수 호출시 제어권이 함수에게 넘어가고 함수 코드 일괄 실행 <br/>
     → 즉, 함수 호출자(caller)는 함수 호출 이후 함수 실행 제어 불가능
   - 제너레이터 함수는 함수 실행을 함수 호출자가 제어 가능 <br/>
     → 함수 호출자가 함수 실행을 일시 중지 또는 재개 가능 ⇒ 함수 제어권을 함수 호출자에게 양도(yield) 가능
2. 제너레이터 함수는 함수 호출자와 함수 상태 주고받기 가능
   - 일반 함수 호출시 매개변수를 통해 함수 외부에서 값 주입받고 함수 코드 일괄 실행하여 결과값을 함수 외부로 반환 <br/>
     → 함수가 실행되는 동안 함수 외부에서 내부로 값 전달하여 함수 상태 변경 불가능
   - 제너레이터 함수는 함수 호출자와 양방향으로 함수 상태 주고받기 가능 <br/>
     ⇒ 함수 호출자에게 상태 전달 가능 & 함수 호출자로부터 상태 전달받기 가능
3. 제너레이터 함수를 호출하면 제너레이터 객체 반환
   - 일반 함수 호출시 함수 코드 일괄 실행 & 값 반환
   - 제너레이터 함수 호출시 이터러블 & 이터레이터 제너레이터 객체 반환

<br/>

## ✅ 46.2\_제너레이터 함수의 정의

- 제너레이터 함수는 `function*` 키워드로 선언 & 하나 이상의 yield 표현식 포함

```jsx
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
  yield 1;
};

// 제너레이터 메서드
const obj = {
  *genObjMethod() {
    yield 1;
  },
};

// 제너레이터 클래스 메서드
class MyClass {
  *genClsMethod() {
    yield 1;
  }
}
```

✋ `*`(애스터리스크) 위치는 function 키워드 ~ 함수 이름 사이라면 어디든지 상관없음!

```jsx
// 다 가능!
function* genFunc() {
  yield 1;
} // 권장!
function* genFunc() {
  yield 1;
}
function* genFunc() {
  yield 1;
}
function* genFunc() {
  yield 1;
}
```

✋ 제너레이터 함수는 화살표 함수로 정의 불가능

```jsx
// ❌
const genArrowFunc = * () => { yield 1 }; // SyntaxError: Unexpected token '*'
```

✋ 제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출 불가능

```jsx
// ❌
function* genFunc() {
  yield 1;
}

new genFunc(); // TypeError: genFunc is not a constructor
```

<br/>

## ✅ 46.3\_제너레이터 객체

- 제너레이터 함수를 호출하면 제너레이터 객체(이터러블(iterable) && 이터레이터(iterator))를 생성해 반환 <br/>
  ⇒ 제너레이터 객체는 Symbol.iterator 메서드를 상속받는 이터러블 && value, done 프로퍼티를 갖는 이터레이터 reject 객체를 반환하는 next 메서드를 소유하는 이터레이터

```jsx
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체 반환
const generator = genFunc();

// 제너레이터 객체는 이터러블이면서 이터레이터
console.log(Symbol.iterator in generator); // true

// 이터레이터는 next 메서드를 가짐
console.log("next" in generator); // true
```

```jsx
// next 메서드 호출 -> value 프로퍼티값: yield된 값, done 프로퍼티값: false
// return 메서드 호출 -> value 프로퍼티값: 인수로 전달받은 값, done 프로퍼티값: true

function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.return("End!")); // {value: 'End!', done: true}

// throw() 호출하면 에러 발생시키고 value프로퍼티값: undefined, done 프로퍼티값: true
console.log(generator.throw("Error!")); // {value: undefined, done: true}
```

<br/>

## ✅ 46.4\_제너레이터의 일시 중지와 재개

- 제너레이터는 yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개 가능
  - 함수 호출자에게 제어권을 양도(yield)하여 필요한 시점에 함수 실행 재개 가능하기 때문
- 제너레이터 객체의 next 메서드를 호출하면 제너레이터 함수 코드 블록을 실행함 (yield 표현식까지만 실행)

💡 `yield`: 제너레이터 함수 실행 일시중지시키거나 yield 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환

```jsx
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.next()); // {value: 2, done: false}
console.log(generator.next()); // {value: 3, done: false}
console.log(generator.next()); // {value: undefined, done: true}
```

```jsx
function* genFunc() {
  // 처음 next 메서드를 호출하면 1번째 yield 표현식까지 실행되고 일시중지
  // 아직 x 변수엔 아무것도 할당되지않음 | x 값은 next 메서드가 2번째 호출될 때 결정됨
  const x = yield 1;

  // 2번째 next 메서드 호출할 때 전달한 인수 10은 x에 할당됨
  // => 즉, 위의 x 할당식은 2번째 next 메서드 호출했을 때 완료

  // 2번째 next 메서드 호출하면 2번째 yield 표현식까지 실행되고 일시중지
  const y = yield x + 10;

  // 3번째 next 메서드 호출할 때 전달한 인수 20은 2번째 y에 할당됨
  // => 즉, 위의 y 할당식dms 3번째 next 메서드 호출했을 때 완료

  // 3번째 next 메서드 호출하면 함수 끝까지 실행됨
  // 일반적으로 제너레이터 반환값은 의미가 없으므로 값을 반환할 필요없고 return은 종료 의미로 사용하기!
  return x + y;
}

const generator = genFunc(0);

// 처음 호출하는 next 메서드엔 인수 전달하지 않음
// 만약 처음 호출하는 next 메서드에 인수 전달하면 무시됨
let res = generator.next();
console.log(res); // {value: 1, done: false}

// 인수 10은 x 변수에 할당
res = generator.next(10);
console.log(res); // {value: 20, done: false}

// 인수 20은 y 변수에 할당
res = generator.next(20);
console.log(res); // {value: 30, done: true}
```

<br/>

## ✅ 46.5\_제너레이터의 활용

### 🔸 46.5.1\_이터러블의 구현

```jsx
// 제너레이터를 사용하여 무한 피보나치 수열 생성 함수 구현

// 무한 이터러블 생성 제너레이터 함수
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];

  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
})();

for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8 ... 4181 6765
}
```

<br/>

### 🔸 46.5.2\_비동기 처리

```jsx
// 프로미스 후속 처리 메서드인 then/catch/finally 없이도 비동기 처리 결과 반환하도록 구현 가능

// 제너레이터 실행기
const async = (generatorFunc) => {
  // 제너레이터 객체 생성
  const generator = generatorFunc(); // 2

  // 상위 스코프의 generator 변수 기억 클로저
  const onResolved = (arg) => {
    const result = generator.next(arg); // 5

    return result.done
      ? result.value // 9 (done: true로 끝까지 실행되었다면 반환값인 undefined 그대로 반환 후 처리 종료)
      : result.value.then((res) => onResolved(res)); // 7 (done: false라면 재귀 호출)
  };

  return onResolved; // 3
};

async(function* fetchTodo() {
  // 1
  const url = "https://~";

  // next 메서드가 처음 호출되면 yield문 까지 실행됨
  const response = yield fetch(url); // 6
  const todo = yield response.json(); // 8
  console.log(todo);
})(); // 4 (onResolved 함수 즉시 호출)
```

```jsx
// async/await 사용하면 위와 같이 구현하지 않아도 되며, 제너레이터 실행기가 필요하다면 co 라이브러리 사용 권장

const fetch = require("node-fetch");
const co = require("co");

co(function* fetchTodo() {
  const url = "https://~";

  const response = yield fetch(url);
  const todo = yield response.json();
  console.log(todo);
});
```

<br/>

## ✅ 46.6_async/await

- ES8에선 제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현 가능한 `async/await` 도입
- `async/await`는 프로미스 기반으로 동작

```jsx
const fetch = require("node-fetch");

async function fetchTodo() {
  const url = "https://~~";

  const response = await fetch(url);
  const todo = await response.json();
  console.log(todo);
}

fetchTodo();
```

<br/>

### 🔸 46.6.1_async 함수

- `await` 키워드는 반드시 async 함수 내부에서 사용해야 함
- `async` 함수는 async 키워드를 사용해 정의하며, 항상 프로미스 반환

```jsx
// async 함수 선언문
async function foo(n) {
  return n;
}
foo(1).then((v) => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) {
  return n;
};
bar(2).then((v) => console.log(v)); // 2

// async 화살표 함수
const baz = async (n) => n;
baz(3).then((v) => console.log(v)); // 3

// async 메서드
const obj = {
  async foo(n) {
    return n;
  },
};
obj.foo(4).then((v) => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
  async bar(n) {
    return n;
  }
}
const myClass = new MyClass();
myClass.bar(5).then((v) => console.log(v)); // 5
```

```jsx
// 클래스 constructor 메서드는 async 메서드가 될 수 없음
// constructor(): 인스턴스 반환 | async(): 프로미스 반환

class MyClass {
	async constructor() { }
	// SyntaxError: Class constructor may not be an async method
}

const myClass = new MyClass();
```

<br/>

### 🔸 46.6.2_await 키워드

- `await` 키워드는 프로미스가 settled 상태(비동기 처리 수행된 상태)가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과 반환
- `await` 키워드는 반드시 프로미스 앞에서 사용해야 함

```jsx
const fetch = require("node-fetch");

const getGithubUserName = async (id) => {
  // 서버 응답이 도착해서 fetch 함수가 반환한 프로미스가 settled 상태가 될 때까지 대기
  // 이후 settled 상태가 되면 resolve한 처리 결과가 res 변수에 할당됨
  const res = await fetch(`https://api.github.com/users/${id}`); // 1
  const { name } = await res.json(); // 2
  console.log(name); // bean choi
};

getGithubUserName("bean");
```

```jsx
// 모든 프로미스에 await 키워드를 사용하면 아래 코드는 약 6초 소요
// => 서로 연관없이 개별적으로 수행되는 비동기 처리의 경우 순차적으로 처리할 필요가 없으므로 Promise.all로 처리*~

async function foo() {
	const res = await Promise.all({
		new Promise(resolve => setTimeout(() => resolve(1), 3000)),
		new Promise(resolve => setTimeout(() => resolve(2), 2000)),
		new Promise(resolve => setTimeout(() => resolve(3), 1000)),
	});

	console.log(res); // [1, 2, 3]
}

foo(); // 약 3초 소요

// ---------------------------------------------------------

// 아래 예시와 같이 비동기 처리 순서가 보장되어야 하는 경우엔 모든 프로미스에 await 키워드를 사용할 수 밖에 없음

async function bar(n) {
	const a = await new Promise(resolve => setTimeout(() => resolve(n), 3000));
	const b = await new Promise(resolve => setTimeout(() => resolve(a+1), 2000));
	const c = await new Promise(resolve => setTimeout(() => resolve(b+1), 1000));

	console.log([a,b,c]); // [1, 2, 3]
}

bar(1); // 약 6초 소요
```

<br/>

### 🔸 46.6.3\_에러 처리

- 에러는 호출자(caller) 방향으로 전파 <br/>
  ⇒ 즉, 콜 스택의 아래 방향으로 전파되지만, 비동기 함수의 콜백함수를 호출한 것은 비동기 함수가 아니므로 try…catch문으로 에러 캐치 불가
- async/await 에러 처리는 try…catch문 사용 가능!

  - 프로미스를 반환하는 비동기 함수는 명시적으로 호출 가능하기 때문에 호출자 명확

  ```jsx
  const foo = async () => {
    try {
      const wrongUrl = "https://wrong.url";
      const response = await fetch(wrongUrl);
      const data = await response.json();
      console.log(data);
    } catch (err) {
      // HTTP 네트워크 에러 ~ try 코드 블록 내 모든 에러까지 모두 캐치 가능
      console.error(err); // TypeError: Failed to fetch
    }
  };

  foo();

  // --------------------------------------

  // catch문으로 에러 처리하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스 반환
  // => async 함수 호출 후 Promise.prototype.catch 후속 처리 메서드로 에러 캐치도 가능

  const foo = async () => {
    const wrongUrl = "https://wrong.url";
    const response = await fetch(wrongUrl);
    const data = await response.json();
    return data;
  };

  foo().then(console.log).catch(console.error); // TypeError: Failed to fetch
  ```
