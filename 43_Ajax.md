# ☑️ 43_Ajax

p.816~829

✍️ 2025.04.02(Wed)

## ✅ 43.1_Ajax란?

- `Ajax`(`Asynchronous JavaScript and  XML`): 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식
  - 브라우저에게 제공하는 Web API인 `XMLHttpRequest` 객체 기반으로 동작
    - `XMLHttpRequest`: HTTP 비동기 통신을 위한 메서드와 프로퍼티 제공

💡 변경할 필요가 없는 부분은 다시 렌더링하지 않고, 변경할 필요가 있는 부분만 한정적으로 렌더링 가능 <br/>
⇒ 브라우저에서도 빠른 퍼포먼스와 부드러운 화면 전환 가능

**👍 Ajax 장점**

1. 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신 발생 X
2. 변경할 필요 없는 부분은 다시 렌더링 X → 화면이 순간적으로 깜박이는 현상 발생 X
3. 클라이언트와 서버와의 통신이 비동기 방식으로 동작하므로 서버에게 요청 보낸 후 블로킹 발생 X

<br/>

## ✅ 43.2_JSON

- `JSON`(`JavaScript Object Notation`): 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷
  - JS에 종속되지 않는 언어 독립형 데이터 포맷으로, 대부분의 프로그래밍 언어에서 사용 가능

### 🔸 43.2.1_JSON 표기 방식

- JSON는 키 & 값으로 구성된 순수한 텍스트
  - 키: 반드시 큰따옴표(작은따옴표 X)로 묶어야 함
  - 값: 객체 리터럴과 같은 표기법 그대로 사용 가능 (반드시 큰따옴표로 묶어야 함)

<br/>

### 🔸 43.2.2_JSON.stringify

- `JSON.stringify()`: 객체 → JSON 포맷의 문자열로 반환
- 직렬화(`serializing`): 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 함

```jsx
const obj = {
	name: 'Lee',
	age: 20,
	alive: true,
	hobby: ['traveling', 'tennis']
};

const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name": "Lee", "age": 20, "alive": true, "hobby": ["traveling", "tennis"]}

// ----------------------------------------------

// 들여쓰기 옵션 추가
const prettyJson = JSON.stringify(obj, null, 2);

// ----------------------------------------------

function filter(key, value) {
	return. typeof value === 'number' ? undefined : value;
}

// 2번째 인수로 replacer 함수 전달
const strFilteredObject = JSON.stringify(obj, filter, 2);
// value 타입이 number인 프로퍼티 표시 X && 들여쓰기
```

```jsx
// JSON.stringify는 배열도 JSON 포맷 문자열로 변환!

const todos = [
  { id: 1, content: "HTML", completed: false },
  { id: 2, content: "CSS", completed: true },
];

const json = JSON.stringify(todos, null, 2);
/*
[
	{ "id": 1,
		"content": "HTML",
		"completed": false
	},
	{ "id": 2,
		"content": "CSS",
		"completed": true
	}
]
*/
```

<br/>

### 🔸 43.2.3_JSON.parse

- `JSON.parse()`: JSON 포맷의 문자열 → 객체로 변환
- 역직렬화(`deserializing`): 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열인데 이를 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야 함

- 배열이 JSON 포맷의 문자열로 변환되어 있는 경우 JSON.parse는 문자열 → 배열 객체로 변환 (type: object)
  - 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환

<br/>

## ✅ 43.3_XMLHttpRequest

- JS 사용하여 HTTP 요청 전송하려면 XMLHttpRequest 객체 사용
  - XMLHttpRequest 객체: Web API로 HTTP 요청 전송 & HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티 제공

### 🔸 43.3.1_XMLHttpRequest 객체 생성

- XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성하며, 브라우저 환경에서만 정상적으로 실행됨

```jsx
const xhr = new XMLHttpRequest(); // XMLHttpRequest 객체 생성
```

<br/>

### 🔸 43.3.2_XMLHttpRequest 객체의 프로퍼티와 메서드

**⌘ XMLHttpRequest 객체의 프로토타입 프로퍼티**

| 프로토타입 프로퍼티 | 설명                                  |
| ------------------- | ------------------------------------- |
| readyState          | HTTP 요청의 현재 상태를 나타내는 정수<br/>- UNSENT: 0<br/>- OPENED: 1<br/>- HEADERS_RECEIVED: 2<br/>- LOADING: 3<br/>- DONE: 4 |
  | status | HTTP 요청에 대한 응답 상태(HTTP 상태 코드)를 나타내는 정수 |
  | statusText | HTTP 요청에 대한 응답 메세지를 나타내는 문자열 |
  | responseType | HTTP 응답 타입<br/>(e.g. document, json, text, blob, arraybuffer) |
  | response | HTTP 요청에 대한 응답 몸체(response body)<br/>* responseType에 따라 타입이 다름 |
  | responseText | 서버가 전송한 HTTP 요청에 대한 응답 문자열 |

**⌘ XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티**

| 프로토타입 프로퍼티            | 설명                                              |
| ------------------------------ | ------------------------------------------------- |
| onreadystatechange             | readyState 프로퍼티 값 변경된 경우                |
| onloadstart                    | HTTP 요청에 대한 응답을 받기 시작한 경우          |
| onprogress                     | HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생 |
| onabort                        | abort 메서드에 의해 HTTP 요청 중단된 경우         |
| onload                         | HTTP 요청이 성공적으로 완료한 경우                |
| ontimeout                      | HTTP 요청 시간이 초과한 경우                      |
| onloadend                      | HTTP 요청이 완료한 경우<br/>HTTP 요청이 성공/실패하면 발생 |

**⌘ XMLHttpRequest 객체의 메서드**

| 프로토타입 프로퍼티 | 설명                                   |
| ------------------- | -------------------------------------- |
| open                | HTTP 요청 초기화                       |
| send                | HTTP 요청 전송                         |
| abort               | 이미 전송된 HTTP 요청 중단             |
| setRequestHeader    | 특정 HTTP 요청 헤더 값 설정            |
| getResponseHeader   | 특정 HTTP 요청 헤더 값을 문자열로 반환 |

**⌘ XMLHttpRequest 객체의 정적 프로퍼티**

| 프로토타입 프로퍼티 | 값  | 설명                                   |
| ------------------- | --- | -------------------------------------- |
| UNSET               | 0   | open 메서드 호출 이전                  |
| OPENED              | 1   | open 메서드 호출 이후                  |
| HEADERS_RECEIVED    | 2   | send 메서드 호출 이후                  |
| LOADING             | 3   | 서버 응답 중 (응답 데이터 미완성 상태) |
| DONE                | 4   | 서버 응답 완료                         |

<br/>

### 🔸 43.3.3_HTTP 요청 전송

⌘ HTTP 요청 전송하는 경우의 순서

1. `XMLHttpRequest.prototype.open` 메서드로 HTTP 요청 초기화
2. 필요에 따라 `XMLHttpRequest.prototype.setRequestHeader` 메서드로 특정 HTTP 요청 헤더 값 설정
3. `XMLHttpRequest.prototype.send` 메서드로 HTTP 요청 전송

```jsx
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open("GET", "/users");

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader("content-type", "application/json");

// HTTP 요청 전송
xhr.send();
```

**⌘ XMLHttpRequest.prototype.open**

- `open()`: 서버에 전송할 HTTP 요청 초기화

```jsx
xhr.open(method, url[, async])
```

| 매개변수                                         | 설명                                           |
| ------------------------------------------------ | ---------------------------------------------- |
| method                                           | HTTP 요청 메서드 (e.g. GET, POST, PUT, DELETE) |
| url                                              | HTTP 요청 전송할 URL                           |
| async                                            | 비동기 요청 여부<br/> 옵션으로 기본값은 true이며, 비동기 방식으로 동작 |

- HTTP 요청 메서드: 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법

| HTTP 요청 메서드 | 종류           | 목적                  | 페이로드 |
| ---------------- | -------------- | --------------------- | -------- |
| GET              | index/retrieve | 모든/특정 리소스 취득 | X        |
| POST             | create         | 리소스 생성           | O        |
| PUT              | replace        | 리소스의 전체 교체    | O        |
| PATCH            | modify         | 리소스의 일부 수정    | O        |
| DELETE           | delete         | 모든/특정 리소스 삭제 | X        |

**⌘ XMLHttpRequest.prototype.send**

- `send()`: open()로 초기화된 HTTP 요청을 서버에 전송
  - `GET()`: 데이터를 URL의 일부분인 쿼리 문자열(query string)로 서버에 전송
  - `POST()`: 데이터(payload)를 requesy body에 담아 전송

✋ 페이로드가 객체인 경우 반드시 JSON.stringify 메서드를 사용하여 직렬화한 다음 전달해야 함

```jsx
xhr.send(JSON.stringify({ id: 1, content: "HTML", completed: false }));
```

- GET 요청 메서드인 경우 send 메서드에 페이로드로 전달한 인수는 무시되고 request body는 null로 설정됨

**⌘ XMLHttpRequest.prototype.setRequestHeader**

- `setRequestHeader()`: 특정 HTTP 요청의 헤더 값 설정 | 반드시 open() 호출 이후에 호출해야 함

- `Content-type`: request body에 담아 전송할 데이터의 MIME 타입의 정보 표현


  | MIME 타입   | 서브타입                                           |
  | ----------- | -------------------------------------------------- |
  | text        | text/plain, text/html, text/css, text/javascript   |
  | application | application/json, application/x-www-form-urlencode |
  | multipart   | multipart/formed-data                              |

```jsx
const xhr = new XMLHttpRequest();
xhr.open("POST", "/users");

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader("content-type", "application/json");

// 서버가 응답할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader("accept", "application/json");
// Accept 헤더 미설정시 send() 호출될 때 Accept 헤더가 */*으로 전송

xhr.send(JSON.stringify({ id: 1, content: "HTML", completed: false }));
```

<br/>

### 🔸 43.3.4_HTTP 응답 처리

```jsx
// HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가 변경될 때마다 발생
xhr.onreadystatechange = () => {
  // readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타냄

  // 응답이 완료되지 않은 상태 -> 아무런 처리 X
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.error("ERROR", xhr.status, xhr.statusText);
  }
};
```

```jsx
// readystatechange 이벤트 대신 load 이벤트를 캐치해도 OK!
// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생
// => xhr.readyState가 XMLHttpRequest.DONE인지 확인할 필요 없음

xhr.onload = () => {
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
  } else {
    console.error("ERROR", xhr.status, xhr.statusText);
  }
};
```
