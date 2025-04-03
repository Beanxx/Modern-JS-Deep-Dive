# ☑️ 44_REST API

p.830~841

✍️ 2025.04.02(Wed)

- `REST`: HTTP 기반으로 클라이언트가 서버 리소스에 접근하는 방식을 규정한 아키텍처
- `REST API`: REST 기반으로 서비스 API를 구현한 것
- `RESTful`: `REST`(`REpresentational State Transfer`)의 기본 원칙을 성실히 지킨 서비스 디자인

## ✅ 44.1_REST API의 구성

- REST API는 자원(resource), 행위(verb), 표현(representations) 3가지 요소로 구성

| 구성 요소             | 내용                           | 표현 방법        |
| --------------------- | ------------------------------ | ---------------- |
| 자원(resource)        | 자원                           | URI (end point)  |
| 행위(verb)            | 자원에 대한 행위               | HTTP 요청 메서드 |
| 표현(representations) | 자원에 대한 행위의 구체적 내용 | Payload          |

<br/>

## ✅ 44.2_REST API 설계 원칙

1. URL은 리소스 표현

- 리소스 식별 이름은 동사보단 명사 사용

  ```jsx
  // 👎
  GET / getTodos / 1;
  GET / todos / show / 1;

  // 👍
  GET / todos / 1;
  ```

2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현

- 리소스에 대한 행위는 URI에 표현 X (리소스 행위 명확히 표현)

  ```jsx
  // 👎
  GET /todos/delete/1

  // 👍
  DELETE /todos/1
  ```

<br/>

## ✅ 44.3_JSON Server를 이용한 REST API 실습

### 🔸 44.3.1_JSON Server 설치

- JSON Server: json 파일을 사용하여 가상 REST API 서버 구축 가능 툴
  ```bash
  $ mkdir json-server-exam && cd json-server-exam
  $ npm init -y
  $ npm install json-server --save-dev
  ```

<br/>

### 🔸 44.3.2_db.json 파일 생성

- db.json 파일은 리소스를 제공하는 데이터베이스 역할

<br/>

### 🔸 44.3.3_JSON Server 실행

- JSON Server가 DB 역할을 하는 db.json 파일 변경 감지하게 하려면 watch 옵션 추가
- 기본 포트: 3000 | 포트 변경시 port 옵션 추가

  ```bash
  $ json-server --watch db.json --port 5000
  ```

- `package.json` 파일의 scripts 수정하여 JSON Server 실행하면 매번 명령어 입력하지 않아도 됨

  ```json
  // package.json

  {
  	...
  	"script": {
  		"start": "json-server --watch db.json"
  	}
  	...
  }

  // npm start 명령어로 JSON Server 실행
  ```

<br/>

### 🔸 44.3.4_GET 요청

```html
<!-- get_index.html -->
<!-- public 폴더에 get_index.html 파일 추가 후 http://localhost:3000/get_index.html 접속 -->

<!DOCTYPE html>
<html>
  <body>
    <pre></pre>
    <script>
      const xhr = new XMLHttpRequest();

      xhr.open("GET", "/todos"); // todos 리소스에서 모든 todo 취득(index)
      xhr.send();

      xhr.onload = () => {
        if (xhr.status === 200) {
          document.querySelector("pre").textContent = xhr.response;
        } else {
          console.error("Error", xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
</html>
```

<br/>

### 🔸 44.3.5_POST 요청

- POST 요청시엔 `setRequestHeader` 메서드를 사용하여 request body에 담아 서버로 전송할 페이로드의 MIME 지정해야 함

```html
<!-- post.html -->
<!-- public 폴더에 post.html 파일 추가 후 http://localhost:3000/post.html 접속 -->

<!DOCTYPE html>
<html>
  <body>
    <pre></pre>
    <script>
      const xhr = new XMLHttpRequest();

      xhr.open("POST", "/todos"); // todos 리소스에 새로운 todo 생성
      xhr.setRequestHeader("content-type", "application/json");

      xhr.send(JSON.stringify({ id: 1, content: "REACT", completed: false }));

      xhr.onload = () => {
        // 200(OK) | 201(created) -> 정상 응답 상태
        if (xhr.status === 200 || xhr.status === 201) {
          document.querySelector("pre").textContent = xhr.response;
        } else {
          console.error("Error", xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
</html>
```

<br/>

### 🔸 44.3.6_PUT 요청

- `PUT`: 특정 리소스 전체 교체할 때 사용
  - `setRequestHeader` 메서드를 사용하여 request body에 담아 서버로 전송할 페이로드의 MIME 지정해야 함

```html
<!-- put.html -->
<!-- public 폴더에 put.html 파일 추가 후 http://localhost:3000/put.html 접속 -->

<!DOCTYPE html>
<html>
  <body>
    <pre></pre>
    <script>
      const xhr = new XMLHttpRequest();

      xhr.open("PUT", "/todos/4"); // todos 리소스에서 id로 todo를 특정하여 id 제외한 리소스 전체 교체
      xhr.setRequestHeader("content-type", "application/json");

      xhr.send(JSON.stringify({ id: 1, content: "Angular", completed: true }));

      xhr.onload = () => {
        if (xhr.status === 200) {
          document.querySelector("pre").textContent = xhr.response;
        } else {
          console.error("Error", xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
</html>
```

<br/>

### 🔸 44.3.7_PATCH 요청

- `PATCH`: 특정 리소스의 일부 수정할 때 사용
  - `setRequestHeader` 메서드를 사용하여 request body에 담아 서버로 전송할 페이로드의 MIME 지정해야 함

```html
<!-- patch.html -->
<!-- public 폴더에 patch.html 파일 추가 후 http://localhost:3000/patch.html 접속 -->

<!DOCTYPE html>
<html>
  <body>
    <pre></pre>
    <script>
      const xhr = new XMLHttpRequest();

      xhr.open("PATCH", "/todos/4"); // todos 리소스의 id로 todo를 특정하여 completed만 수정
      xhr.setRequestHeader("content-type", "application/json");

      xhr.send(JSON.stringify({ completed: false }));

      xhr.onload = () => {
        if (xhr.status === 200) {
          document.querySelector("pre").textContent = xhr.response;
        } else {
          console.error("Error", xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
</html>
```

<br/>

### 🔸 44.3.8_DELETE 요청

```html
<!-- delete.html -->
<!-- public 폴더에 delete.html 파일 추가 후 http://localhost:3000/delete.html 접속 -->

<!DOCTYPE html>
<html>
  <body>
    <pre></pre>
    <script>
      const xhr = new XMLHttpRequest();

      xhr.open("DELETE", "/todos/4"); // todos 리소스에서 id 사용하여 todo 삭제
      xhr.send();

      xhr.onload = () => {
        if (xhr.status === 200) {
          document.querySelector("pre").textContent = xhr.response;
        } else {
          console.error("Error", xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
</html>
```
