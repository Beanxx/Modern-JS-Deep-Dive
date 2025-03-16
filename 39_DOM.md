# ☑️ 39_DOM

p.677~753

✍️ 2025.02.11(Tues), 03.05(Wed)~03.16(Sun)

👾 브라우저 렌더링 엔진은 HTML 문서 파싱 → 브라우저가 이해할 수 있는 자료구조인 DOM 생성

- `DOM`(`Document Object Model`): HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API <br/>
  ⇒ 프로퍼티와 메서드를 제공하는 트리 자료구조

<br/>

## ✅ 39.1\_노드

### 🔸 39.1.1_HTML 요소와 노드 객체

- **HTML 요소(element)**: HTML 문서를 구성하는 개별적인 요소

- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환됨

  - `attribute` → `attribute node`
  - `text content` → `text node`

- HTML 요소 간의 부자(parent-child) 관계를 반영하여 HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료 구조로 구성

**🎄 트리 자료구조**

- **트리 자료구조(tree data structure)**: 노드들의 계층 구조로 이뤄짐 <br/>
  ⇒ 부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조를 표현하는 비선형 자료구조를 말함
  - 하나의 최상위 노드에서 시작
  - **최상위 노드**( = 루트 노드; root node) : 부모 노드 없음
  - **루트 노드** : 0개 이상의 자식 노드를 가짐
  - **리프 노드**(leaf node): 자식 노드가 없는 노드

🧚 `DOM` : 노드 객체들로 구성된 트리 자료구조

<br/>

### 🔸 39.1.2\_노드 객체의 타입

**‣ 문서 노드 (`document node`)** <br/>
: DOM 트리 최상위에 존재하는 루트 노드로서 document 객체를 가리킴

- document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 window의 document 프로퍼티에 바인딩되어 있음 <br/>
  ⇒ 문서 노드는 `window.document` | `document`로 참조 가능

👀 브라우저 환경의 모든 JS 코드는 script 태그에 의해 분리되어 있어도 하나의 전역 객체 window 공유

⇒ 모든 JS 코드는 전역 객체 window의 document 프로퍼티에 바인딩되어 있는 하나의 document 객체를 바라봄 (HTML 문서당 document 객체는 유일!)

- 문서 노드 즉, document 객체는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할 담당
  ⇒ 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 함

**‣ 요소 노드 (`element node`)** <br/>
: HTML 요소를 가리키는 객체

- 요소 노드는 HTML 요소 간의 중첩에 의해 부자 관계를 가지며, 이를 통해 정보 구조화
  ⇒ 문서 구조 표현

**‣ 어트리뷰트 노드 (`attribute node`)** <br/>
: HTML 요소의 어트리뷰트를 가리키는 객체

- 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있음
  - ✋ 요소 노드 - 부모 노드와 연결 | 어트리뷰트 노드 - 부모 노드와 연결 X & 요소 노드에만 연결 <br/>
    ⇒ 어트리뷰트 노드는 부모 노드가 없으므로 요소 노드의 형제(sbling) 노드 X <br/>
    → 어트리뷰트를 참조하거나 변경하려면 요소 노드에 접근해야 함

**‣ 텍스트 노드 (`text node`)** <br/>
: HTML 요소의 텍스트를 가리키는 객체 ⇒ 문서 정보 표현

- 텍스트 노드: 요소 노드의 자식 노드 & 자식 노드를 가질 수 없는 리프 노드 <br/>
  → DOM 트리의 최종단 <br/>
  ⇒ 텍스트 노드에 접근하려면 먼저 부모 노드인 요소 노드에 접근해야 함

<br/>

### 🔸 39.1.3\_노드 객체의 상속 구조

- DOM을 구성하는 노드 객체는 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체 <br/>(노드 객체도 JS 객체이므로 프로토타입에 의한 상속 구조를 가짐)

- `Object`
  - `EventTarget`
    - `Node`
      - Document
        - HTMLDocument
      - Element
        - HTMLElement
          - HTMLHtmlElement
          - HTMLBodyElement
          - …
      - Attr
      - CharacterData
        - Text
        - Comment

<br/>

## ✅ 39.2\_요소 노드 취득

### 🔸 39.2.1_id를 이용한 요소 노드 취득

- `Document.prototype.getElementById()`: 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환

```jsx
const $elem = document.getElementById("banana");
```

✋ id 값은 HTML 문서 내에서 유일한 값이어야하며, 공백 문자로 구분하여 여러 개의 값을 가질 수 없음 <br/>
(여러 개 존재해도 에러는 발생하지 않음 → 첫번째 요소 노드만 반환) <br/>
⇒ 항상 단 하나의 요소 노드만을 반환!

✋ 만약 인수로 전달된 id 값을 갖는 HTML 요소가 존재하지 않는 경우 `null` 반환

- HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과 있음

  ```jsx
  <div id="foo"></div>

  console.log(foo === document.getElementById('foo'); // true

  // 암묵적 전역으로 생성도니 전역 프로퍼티는 삭제
  // 전역 변수는 삭제 X
  delete foo;

  // ** id 값과 동일한 이름의 전역 변수가 이미 선언된 경우
  // -> 전역 변수에 노드 객체가 재할당되지 않음

  let foo = 1;
  console.log(foo); // 1
  ```

<br/>

### 🔸 39.2.2\_태그 이름을 이용한 요소 노드 취득

- `Document.prototype/Element.prototype.getElementsByTagName()`: 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환
  - 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 `HTMLCollection` 객체 반환 <br/>
    (`HTMLCollection`; 유사 배열객체 & 이터러블)

```jsx
// 태그 이름이 li인 요소 노드 모두 탐색하여 반환
const $elems = document.getElementsByTagName("li");

// 모든 요소 노드 취득하려면 메서드 인수로 '*' 전달
const $all = document.getElementsByTagName("*");
```

✋ 만약 인수로 전달된 태그 이름을 갖는 요소가 존재하지 않는 경우 빈 HTMLCollection 객체 반환

<br/>

### 🔸 39.2.3_class를 이용한 요소 노드 취득

- `Document.prototype/Element.prototype.getElementsByClassName()`: 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환
  - 인수로 전달할 class 값은 공백으로 구분하여 여러 개의 class 지정 가능
  - 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체 반환

```jsx
<li class="fruit apple">Apple</li>
<li class="fruit banana">Banana</li>

// class 값이 'fruit'인 요소 노드 모두 탐색하여 HTMLCollection 객체에 담아 반환
const $elems = document.getElementsByClassName('fruit');

const $apples = document.getElementsByClassName('fruit apple');
```

✋ 만약 인수로 전달된 class 값을 갖는 요소가 존재하지 않는 경우 빈 HTMLCollection 객체 반환

<br/>

### 🔸 39.2.4_CSS 선택자를 이용한 요소 노드 취득

- CSS 선택자(selector): 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법

```css
/* 전체 선택자: 모든 요소 선택 */
* { ... }

/* id 선택자: id 값이 'foo'인 요소 모두 선택 */
#foo { ... }

/* class 선택자 */
.foo { ... }

/* attribute 선택자: input 요소 중에 type 어트리뷰트 값이 'text' 요소 모두 선택 */
input[type=text] { ... }

/* 후손 선택자: div 요소의 후손 요소 중 p 요소 모두 선택 */
div p { ... }

/* 자식 선택자: div 요소의 자식 요소 중 p 요소 모두 선택 */
div > p { ... }

/* 인접 형제 선택자: p 요소의 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소 모두 선택 */
p + ul { ... }

/* 일반 형제 선택자: p 요소의 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소 모두 선택 */
p ~ ul { ... }

/* 가상 클래스 선택자: hover 상태인 a 요소 모두 선택 */
a:hover { ... }

/* 가상 요소 선택자: p 요소의 콘텐츠의 앞에 위치하는 공간을 선택 */
p::before {...}}
```

- `Document.prototype/Element.prototype.querySelector()`: 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환

  - 인수로 전달한 CSS 선택자를 만족시키는 요소 노드가 여러 개인 경우 첫번째 요소 노드만 반환
  - 인수로 전달된 CSS 선택자를 만족시키는 요소 노드가 존재하지 않는 경우 null 반환
  - 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러 발생

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul>
        <li class="apple">Apple</li>
        <li class="banana">Banana</li>
        <li class="orange">Orange</li>
      </ul>
    </body>
    <script>
      // class attr value가 'banana'인 첫번째 노드를 탐색하여 반환
      const $elem = document.querySelector(".banana");

      // 취득한 요소 노드의 style.color 프로퍼티 값 변경
      $elem.style.color = "red";
    </script>
  </html>
  ```

- `Document.prototype/Element.prototype.querySelectorAll()` : 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드 탐색하여 반환

  - 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체 반환
    - `NodeList` 객체: 유사 배열 객체 && iterable
  - 인수로 전달된 CSS 선택자를 만족시키는 요소가 존재하지 않는 경우 빈 NodeList 객체 반환
  - 인수로 전달한 CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러 발생

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul>
        <li class="apple">Apple</li>
        <li class="banana">Banana</li>
        <li class="orange">Orange</li>
      </ul>
    </body>
    <script>
      // ul 요소의 자식 요소인 li 요소 모두 탐색하여 반환
      const $elems = document.querySelectorAll("ul > li");

      console.log($elems); // NodeList(3) [li.apple, li.banana, li.orange]

      // NodeList는 forEach 메서드 제공
      $elem.style.color = "red";
    </script>
  </html>
  ```

🍄 모든 요소 노드를 취득하려면 querySelectorAll 메서드의 인수로 전체 선택자 ‘`*`’ 전달

```jsx
const $all = document.querySelectorAll("*");
// NodeList(8) [html, head, body, ul li#apple, li#banana, li#orange, script]
```

- Document.prototype에 정의된 메서드 : DOM의 루트 노드인 문서 노드, 즉 document를 통해 호출 | DOM 전체에서 요소 노드를 탐색하여 반환
- Element.prototype에 정의된 메서드 : 특정 요소 노드를 통해 호출 | 특정 요소 노드의 자손 노드 중에서 요소 노드 탐색하여 반환

👍 id 어트리뷰트가 있는 요소 노드를 취득하는 경우엔 `getElementById` 메서드 사용 | 그 외의 경우엔 querySelector, `querySelectorAll` 메서드 사용 권장 ~\*

<br/>

### 🔸 39.2.5\_특정 요소 노드를 취득할 수 있는지 확인

- `Element.prototype.matches()`: 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <ul id="fruits">
        <li class="apple">Apple</li>
        <li class="banana">Banana</li>
        <li class="orange">Orange</li>
      </ul>
    </body>
    <script>
      const $apple = document.querySelector(".apple");
      console.log($apple.matches("#fruits > li.apple")); // true
    </script>
  </html>
  ```

<br/>

### 🔸 39.2.6_HTMLCollection과 NodeList

- `HTMLCollection` | `NodeList` : DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체
  - 둘 다 유사 배열 객체 && iterable ⇒ for…of문으로 순회 및 spread 문법 사용 가능
  - 노드 객체 상태 변화를 실시간으로 반영하는 살아 있는 객체 ~\*
    - `HTMLCollection`: 항상 live 객체로 동작
    - `NodeList`: non-live 객체로 동작하지만 경우에 따라 live 객체로 동작할 때도 있음

‣ `HTMLCollection` <br/>
: 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는(live) DOM 컬렉션 객체로, `getElementsByTagName`, `getElementsByClassName` 메서드가 반환하는 객체

✋ HTMLCollection 객체는 실시간으로 노드 객체 상태 변경을 반영하여 요소를 제거할 수 있기 때문에 for문으로 순회하면서 노드 객체 상태를 변경해야 할 땐 주의해야 함! <br/>
⇒ for문 역방향으로 순회하는 방법으로 회피 가능

```jsx
for (let i = $elems.length - 1; i >= 0; i--) {
  $elems[i].className = "blue";
}
```

⇒ while문을 사용하여 HTMLCollection 객체에 노드 객체가 남아 있지 않을 때까지 무한 반복하는 방법도 가능

```jsx
let i = 0;
while ($elems.length > i) {
  $elems[i].className = "blue";
}
```

⇒ HTMLCollecction 객체 사용 지양 → 배열의 고차함수 사용 가능

```jsx
// 유사 배열 객체이면서 iterable인 HTMLCollection을 배열로 변환하여 순회
[...$elems].forEach((elem) => (elem.className = "blue"));
```

‣ `NodeList` <br/>
: queryAll 메서드가 반환하는 DOM 컬렉션 객체이며, 실시간으로 노드 객체 상태 변경을 반영하지 않는(non-live) 객체

✋ childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체 상태 변경을 반영하는 live 객체로 동작하므로 주의 필요 ~\* <br/>
⇒ 객체를 배열로 변환하여 사용하는 것을 권장 !

```jsx
const $fruits = document.getElementById("fruits");

const { childNodes } = $fruits;

// spread 문법을 사용해서 NodeList 객체를 -> 배열로 변환
[...childNodes].forEach((childNode) => {
  $fruits.removeChild(childNode);
});

// 모든 자식 노드 삭제
console.log(childNodes); // NodeList []
```

<br/>

## ✅ 39.3\_노드 탐색

- 노드 탐색 프로퍼티는 모두 접근자 프로퍼티
  - 단, setter 없이 getter만 존재하여 참조만 가능한 읽기 전용 접근자 프로퍼티
    (값 할당시 아무런 에러 없이 무시됨)

### 🔸 39.3.1\_공백 텍스트 노드

- 공백 텍스트 노드: 공백 문자(white space)는 텍스트 노드 생성

<br/>

### 🔸 39.3.2\_자식 노드 탐색

- `Node.prototype.childNodes`
  - 자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환
  - 반한된 NodeList에는 요소 노드 + 텍스트 노드도 포함되어 있을 수 있음
- `Element.prototype.children`
  - 자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환
  - children 프로퍼티가 반환한 HTMLCollection엔 텍스트 노드 포함 X
- `Node.prototype.firstChild`
  - 첫번째 자식 노드 반환 (텍스트 노드 | 요소 노드)
- `Node.prototype.lastChild`
  - 마지막 노드 반환 (텍스트 노드 | 요소 노드)
- `Element.prototype.firstElementChild`
  - 첫번째 자식 요소 노드 반환 (요소 노드만 반환)
- `Element.prototype.lastElementChild`
  - 마지막 자식 요소 노드 반환 (요소 노드만 반환)

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
      <li class="banana">Banana</li>
      <li class="orange">Orange</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    console.log($fruits.childNodes);
    // NodeList(7) [text, li.apple, text, li.banana, text, li.orange, text]

    console.log($fruits.chidlren);
    // HTMLCollection(3) [li.apple, li.banana, li.orange]

    console.log($fruits.firstChild); // #text

    console.log($fruits.firstElementChild); // li.apple
  </script>
</html>
```

<br/>

### 🔸 39.3.3\_자식 노드 존재 확인

- `Node.prototype.hasChildNodes()`: 자식 노드 존재하는지 확인 메서드
  - 자식 노드가 존재하면 true | 존재하지 않으면 false
  - 텍스트 노드 포함하여 노드 존재 확인

💡 자식 노드 중 텍스트 노드가 아닌 요소 노드가 존재하는지 확인하려면 `children.length` | Element 인터페이스의 `childElementCount` 프로퍼티 사용

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits"></ul>
  </body>
  <script>
    const $fruits = document.getElementById("fruits");

    console.log($fruits.hasChildNodes()); // true

    console.log($fruits.chidlren.length); // 0 -> false
    console.log($fruits.childElementCount); // 0 -> false
  </script>
</html>
```

<br/>

### 🔸 39.3.4\_요소 노드의 텍스트 노드 탐색

- 요소 노드의 텍스트 노드는 요소 노드의 자식 노드 → `firstChild` 프로퍼티로 접근 가능

```html
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    console.log(document.getElementById("foo").firstChild); // #text
  </script>
</html>
```

<br/>

### 🔸 39.3.5\_부모 노드 탐색

- `Node.prototype.parentNode`: 부모 노드 탐색 프로퍼티
  - 텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드(leaf node)이므로 부모 노드가 텍스트 노드인 경우는 없음

```jsx
const $banana = document.querySelector(".banana");
console.log($banana.parentNode); // ul#fruits
```

<br/>

### 🔸 39.3.6\_형제 노드 탐색

- `Node.prototype.previousSibling`
  - 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환
- `Node.prototype.nextSibling`
  - 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환
- `Element.prototype.previousElementSibling`
  - 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 요소 노드를 탐색하여 반환 (요소 노드만 반환)
- `Element.prototype.nextElementSibling`
  - 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 요소 노드를 탐색하여 반환 (요소 노드만 반환)

✋ 어튜리뷰트 노드는 요소 노드와 연결되어 있지만 부모 노드가 같은 형제 노드가 아니기 때문에 반환 X <br/>
⇒ 텍스트 노드 또는 요소 노드만 반환

<br/>

## ✅ 39.4\_노드 정보 취득

- `Node.prototype.nodeType`: 노드 객체의 종류, 즉 노드 타입을 나타내는 상수 반환
  - `Node.ELEMENT_NODE`: 요소 노드 타입 상수 1 반환
  - `Node.TEXT_NODE`: 텍스트 노드 타입 상수 3 반환
  - `Node.DOCUMENT_NODE`: 문서 노드 타입 상수 9 반환
- `Node.prototype.nodeName`: 노드의 이름을 문자열로 반환
  - `요소 노드:` 대문자 문자열로 태그 이름(e.g. ‘UL’, ‘LI’ …) 반환
  - `텍스트 노드`: `“#text”` 반환
  - `문서 노드`: 문자열 `“#document”` 반환

<br/>

## ✅ 39.5\_요소 노드의 텍스트 조작

### 🔸 39.5.1_nodeValue

- `Node.prototype.nodeValue`: setter & getter 모두 존재하는 접근자 프로퍼티 ⇒ 참조 & 할당 모두 가능

  - 노드 객체의 nodeValue 프로퍼티를 참조하면 노드 객체 값(텍스트 노드의 텍스트) 반환 <br/>
    ⇒ 텍스트 노드가 아닌 노드 즉, 문서 노드나 요소 노드의 nodeValue 프로퍼티를 참조하면 `null` 반환

  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo">Hello</div>
    </body>
    <script>
      // 문서 노드
      console.log(document.nodeValue); // null

      // 요소 노드
      const $foo = document.getElementById("foo");
      console.log($foo.nodeValue); // null

      // 텍스트 노드
      const $textNode = $foo.firstChild;
      console.log($textNode.nodeValue); // Hello

      // => 텍스트 노드의 nodeValue 프로퍼티를 참조할 때만 텍스트 노드 값인 텍스트 반환 ~*
    </script>
  </html>
  ```

- 텍스트 노드의 nodeValue 프로퍼티에 값을 할당하면 텍스트 노드의 값인 텍스트 변경 가능

  1. 텍스트를 변경할 요소 노드를 취득한 다음, 취득한 요소 노드의 텍스트 노드 탐색 <br/>
     (텍스트 노드는 요소 노드의 자식 노드이므로 firstChild 프로퍼티 사용하여 탐색)

  2. 탐색한 텍스트 노드의 nodeValue 프로퍼티를 사용하여 텍스트 노드 값 변경

  ```jsx
  const $textNode = document.getElementById("foo").firstChild;

  // 텍스트 노드 값 변경
  $textNode.nodeValue = "World";

  console.log($textNode.nodeValue); // World
  ```

<br/>

### 🔸 39.5.2_textContent

- `Node.prototype.textContent`: setter && getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트 모두 취득하거나 변경
  - 요소 노드의 콘텐츠 영역 내 텍스트 모두 반환 <br/>
    ⇒ 요소 노드의 ChildNodes 프로퍼티가 반환한 모든 노드들의 텍스트 노드의 값인 텍스트 모두 반환 (HTML 마크업은 무시)
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo">Hello <span>world!</span></div>
    </body>
    <script>
      console.log(document.getElementById("foo").textContent); // Hello world!
    </script>
  </html>
  ```
  ```html
  <!-- 요소 노드의 콘텐츠 영역에 다른 요소 노드가 없고 텍스트만 존재하는 경우 -->
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo">Hello</div>
    </body>
    <script>
      console.log($foo.textContent === $foo.firstChild.nodeValue); // true
      // => textContent 사용이 코드 더 간단 ~*
    </script>
  </html>
  ```
  - textContent 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가됨
    - HTML 마크업이 포함된 문자열 그대로 인식 ⇒ HTML 마크업 파싱 X
    ```html
    <!DOCTYPE html>
    <html>
      <body>
        <div id="foo">Hello <span>world!</span></div>
      </body>
      <script>
        document.getElementById("foo").textContent = "Hi <span>there!</span";
        // 브라우저에 태그까지 그대로 보임
      </script>
    </html>
    ```

<br/>

## ✅ 39.6_DOM 조작

- DOM 조작(manipulation): 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 및 교체하는 것
  - DOM에 새로운 노드가 추가/삭제되면 리플로우 && 리페인트 발생 → 성능 영향 미침 <br/>
    ⇒ 성능 최적화를 위해 주의해서 다뤄야 함 ~\*

### 🔸 39.6.1_innerHTML

- Element.prototype.innerHTML 프로퍼티: setter && getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 HTML 마크업 취득 및 변경
  - 참조시 요소 노드의 콘텐츠 영역 내 포함된 모든 HTML 마크업을 문자열로 반환
  ```html
  <!DOCTYPE html>
  <html>
    <body>
      <div id="foo">Hello <span>world!</span></div>
    </body>
    <script>
      console.log(document.getElementById("foo").innerHTML);
      // "Hello <span>world!</span>"
    </script>
  </html>
  ```

💡 textContent 프로퍼티 참조시 HTML 마크업 무시하고 텍스트만 반환하는 반면, innerHTML 프로퍼티는 HTML 마크업 포함된 문자열 그대로 반환!

- 요소 노드의 innerHTML 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고, 할당한 문자열에 포함되어 있는 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영됨
  ```jsx
  // HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영
  document.getElementById("foo").innerHTML = "Hi <span>there!</span>";
  ```

⇒ innerHTML 프로퍼티를 사용하면 HTML 마크업 문자열로 간단히 DOM 조작 가능

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 노드 추가
    $fruits.innerHTML += "<li class='banana'>Banana</li>";

    // 노드 교체
    $fruits.innerHTML = "<li class="orange">Orange</li>";

    // 노드 삭제
    $fruits.innerHTML = '';
  </script>
</html>
```

- innerHTML 프로퍼티에 할당한 HTML 마크업 문자열은 렌더링 엔진에 의해 파싱되어 요소 노드의 자식으로 DOM에 반영됨 <br/>
  ⇒ ⚠️ 입력받은 데이터 그대로 innerHTML 프로퍼티에 할당하는것은 크로스 사이트 스크립팅 공격(`XSS`: `Cross-Site Scripting Attacks`)에 취약하므로 위험 <br/>
  (why? 마크업 내에 JS 악성 코드가 포함되어 있다면 파싱 과정에서 그대로 실행될 가능성이 있기 때문)

> HTML sanitization
> : 사용자로부터 입력받은 데이터에 의해 발생할 수 있는 XSS 공격을 예방하기 위해 잠재적 위험 제거 기능
> ⇒ DOMPurify 라이브러리 사용 권장
>
> ```jsx
> DOMPurify.sanitize('<img src="x"onerror="alert(document.cookie)">');
> // => <img src="x">
> ```

👎 innerHTML 프로퍼티에 HTML 마크업 문자열을 할당하는 경우 요소 노드의 모든 자식 노드를 제거하고 할당한 HTML 마크업 문자열을 파싱하여 DOM 변경함

👎 innerHTML 프로퍼티는 새로운 요소 삽입시 삽입될 위치 지정 불가

<br/>

### 🔸 39.6.2_insertAdjacentHTML 메서드

- `Element.prototype.insertAdjacentHTML(position, DOMString)`: 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소 삽입
  - 2번째 인수로 전달한 HTML 마크업 문자열(DOMString)을 파싱하고 그 결과로 생성된 노드를 1번째 인수로 전달한 위치(position)에 삽입하여 DOM에 반영
    - 1번째 인수로 전달 가능한 문자열: ‘`beforebegin`’, ‘`afterbegin`’, ‘`beforeend`’, ‘`afterend`’

```html
<body>
  <!-- beforebegin -->
  <div id="foo">
    <!-- afterbegin -->
    text
    <!-- beforeend -->
  </div>
  <!-- afterened -->
</body>

$foo.insertAdjacentHTML('beforebegin', '
<p>beforebegin</p>
');
```

👍 insertAdjacentHTML 메서드는 기존 요소에 영향을 주지 않고 새롭게 삽입될 요소만 파싱하여 자식 요소로 추가하므로 innerHTML 프로퍼티보다 효율적이고 fast ~\* <br/>
But, HTML 마크업 문자열을 파싱하므로 XSS 공격엔 똑같이 취약 !

<br/>

### 🔸 39.6.3\_노드 생성과 추가

```jsx
const $fruits = document.getElementById("fruits");

// 1. 요소 노드 생성
const $li = document.createElement("li");

// 2. 텍스트 노드 생성
const textNode = document.createTextNode("Banana");

// 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
$li.appendChild(textNode);

// 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
$fruits.appendChild($li);
```

**‣ 요소 노드 생성**

- `Document.prototype.createElement(tagName)`: 요소 노드 생성하여 반환

  - `tagName`: 태그 이름을 나타내는 문자열을 인수로 전달

  ```jsx
  const $li = document.createElement("li");

  // 기존 DOM에 추가되지 않고 홀로 존재하는 상태
  // => 요소 노드를 생성할 뿐 DOM에 추가하지 않음 ~*

  // createElement 메서드로 생성한 요소 노드는 아무런 자식 노드를 가지고 있지 않으므로
  // 요소 노드의 자식 노드인 텍스트 노드도 없음!

  console.log($li.childNodes); // NodesList []
  ```

**‣ 텍스트 노드 생성**

- `Document.prototype.createTextNode(text)`: 텍스트 노드 생성하여 반환

  - `text`: 텍스트 노드의 값으로 사용할 문자열을 인수로 전달

  ```jsx
  const textNode = document.createElement("Banana");

  // 요소 노드의 자식 노드로 추가되지 않고 홀로 존재하는 상태
  // => createTextNode 메서드는 텍스트 노드를 생성할 뿐 요소 노드에 추가 X
  ```

**‣ 텍스트 노드를 요소 노드의 자식 노드로 추가**

- `Node.prototype.appendChild(childNode)`: 매개변수 childNode에게 인수로 전달한 노드를 appendChild 메서드를 호출한 노드의 마지막 자식 노드로 추가
  - 인수로 createTextNode 메서드로 생성한 텍스트 노드를 전달하면 appendChild 메서드를 호출한 노드의 마지막 자식 노드로 텍스트 노드가 추가됨
    ```jsx
    $li.appendChild(textNode); // 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
    ```

```jsx
// 텍스트 노드를 생성하여 요소 노드의 자식 노드로 추가
$li.appendChild(document.createTextNode("Banana"));

$li.textContent = "Banana";

// 요소 노드에 자식 노드가 하나도 없는 경우엔 textContent 프로퍼티 사용하는 것이 더 간편 ~*

// ✋ 요소 노드에 자식 노드가 있는 경우엔
// 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가되므로 주의!
```

**‣ 요소 노드를 DOM에 추가**

- `Node.prototype.appendChild` 메서드를 사용하여 텍스트 노드와 부자 관계로 연결한 요소 노드를 #fruits 요소 노드의 마지막 자식 요소로 추가
  ```jsx
  // $li 요소 노드를 #fruits 요소 노드의  마지막 자식 노드로 추가
  $fruits.appendChild($li);
  ```

⇒ 이러한 과정에서 새롭게 생성한 요소 노드가 DOM에 추가됨 <br/>
(단 하나의 요소 노드를 생성하여 DOM에 1번 추가하므로 DOM은 1번 변경됨 → 리플로우 && 리페인트 실행)

<br/>

### 🔸 39.6.4\_복수의 노드 생성과 추가

```jsx
['Apple', 'Banana', 'Orange'].forEach(text => {
	const $li = document.createElement('li'); // 요소 노드 생성
	const textNode = document.createTextNode(text); // 텍스트 노드 생성

	$li.appendChild(textNode); // 텍스트 노드를 $li 요소 노드의 자식 노드로 추가

	$fruits.appendChild($li); // $li 요소 노드를 fruits 요소 노드의 마지막 자식 노드로 추가
}

// DOM에 3번 추가하므로 DOM이 3번 변경됨
// => 리플로우 && 리페인트 3번 실행됨
// ** 기존 DOM에 요소 노드를 반복하여 추가하는 것은 비효율적!
```

```jsx
// 위와 같은 문제를 해결하기 위해 컨테이너 요소 사용해보기!

// 1. 컨테이너 요소 미리 생성
// 2. DOM에 추가해야 할 3개의 요소 노드를 컨테이너 요소에 자식 노드로 추가
// 3. 컨테이너 요소를 #fruit 요소에 자식으로 추가
// => DOM은 1번만 변경됨

const $container = document.createElement('div'); // 컨테이너 요소 노드 생성

['Apple', 'Banana', 'Orange'].forEach(text => {
 // 위의 과정과 동일
}

$fruits.appendChild($container); // 컨테이너 요소 노드를 마지막 자식 노드로 추가

// DOM을 1번만 변경하므로 성능엔 유리하지만
// 불필한 컨테이너 요소(div)가 DOM에 추가되는 부작용 존재
```

```jsx
// 위의 문제는 DocumentFragment 노드를 통해 해결 가능
// DocumentFragment 노드: 노드 객체의 일종으로, 부모 노드가 없어서 기존 DOM과는 별도로 존재
// => 자식 노드들의 부모 노드로서 별도의 서브 DOM을 구성하여 기존 DOM에 추가하기 위한 용도로 사용

// 기존 DOM과 별도로 존재하므로 자식 노드를 추가해도 기존 DOM엔 어떠한 변경도 발생 X
// DocumentFragment 노드를 DOM에 추가하면 자신은 제거되고 자신의 자식 노드만 DOM에 추가됨

// Document.prototype.createDocumentFragment 메서드
// : 비어 있는 DocumentFragment 노드를 생성하여 반환

const $fragment = document.createDocumentFragment();
```

- `Document.prototype.createDocumentFragment`: 비어 있는 DocumentFragment 노드를 생성하여 반환 <br/>
  ⇒ 실제 DOM 변경은 1번뿐이며, 리플로우와 리페인트도 1번만 실행됨 <br/>
  → 여러 개의 요소 노드를 DOM에 추가하는 경우 DocumentFragment 노드를 사용하는 것이 효율적 ~\*

<br/>

### 🔸 39.6.5\_노드 삽입

**‣ 마지막 노드로 추가**

- `Node.prototype.appendChild`: 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가
  - 노드를 추가할 위치를 지정할 순 없고 항상 마지막 자식 노드로 추가

```jsx
const $li = document.createElement("li"); // 요소 노드 생성

// 텍스트 노드를 $li 요소 노드의 마지막 자식 노드로 추가
$li.appendChild(document.creaeteTextNode("Orange"));

// $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
document.getElementById("fruits").appendChild($li);
```

**‣ 지정한 위치에 노드 삽입**

- `Node.prototype.insertBefore(newNode, childNode)`: 1번째 인수로 전달받은 노드를 2번째 인수로 전달받은 노드 앞에 삽입

```jsx
<li>Apple</li>
<li>Banana</li>

const $li = document.createElement('li'); // 요소 노드 생성

// 텍스트 노드를 $li 요소 노드의 마지막 자식 노드로 추가
$li.appendChild(document.creaeteTextNode('Orange'));

// $li 요소 노드를 #fruits 요소 노드의 마지막 자식 요소 앞에 삽입
$fruits.insertBefore($li, $fruits.lastElementChild);
// Apple - Orange - Banana

// ✋ 2번째 인수로 전달받은 노드는 반드시 insertBefore 메서드를 호출한 노드의 자식 노드여야 함
// 그렇지 않으면 DOMException Error 발생

// 2번째 인수로 전달받은 노드가 null이면 1번째 인수로 전달받은 노드를
// insertBefore 메서드로 호출한 노드의 마지막 자식 노드로 추가됨
// => appendChild 메서드처럼 동작

$fruits.insertBefore($li, null);
// Apple - Banana - Orange
```

<br/>

### 🔸 39.6.6\_노드 이동

- DOM에 이미 존재하는 노드를 `appendChild` | `insertBefore` 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드 추가 ⇒ 노드 이동

```jsx
<li>Apple</li>
<li>Banana</li>
<li>Orange</li>

// 이미 존재하는 요소 노드 취득
const [$apple, $banana, ] = $fruits.children;

// 이미 존재하는 $apple 요소 노드를 #fruits 요소 노드의 마지막 노드로 이동
$fruits.appendChild($apple); // Banana - Orange - Apple

// 이미 존재하는 $banana 요소 노드를 #fruits 요소의 마지막 자식 노드 앞으로 이동
$fruits.insertBefore($banana, $fruits.lastElementChild);
// Orange - Banana - Apple
```

<br/>

### 🔸 39.6.7\_노드 복사

- `Node.protoype.cloneNode([deep: true | false])`: 노드의 사본을 생성하여 반환

  - 매개변수 deep에 true를 인수로 전달 : 노드를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성
  - false를 인수로 전달 | 생략 : 노드를 얕은 복사하여 노드 자신만의 사본 생성 → 자손 노드 복사 X → 텍스트 노드 X

  ```jsx
  const $fruits = document.getElementById("fruits");
  const $apple = $fruits.firstElementChild;

  // $apple 요소를 얕은 복사 -> 사본 생성. 텍스트 노드 없는 사본 생성
  const $shallowClone = $apple.cloneNode();

  // 사본 요소 노드에 텍스트 추가
  $shallowClone.textContent = "Banana";

  // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
  $fruits.appendChild($shallowClone);

  // #fruits 요소를 깊은 복사 -> 모든 자손 노드가 포함된 사본 생성
  const $deepClone = $fruits.cloneNode(true);

  // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
  $fruits.appendChild($deepClone);

  -Apple - Banana - Apple - Banana;
  ```

<br/>

### 🔸 39.6.8\_노드 교체

- `Node.prototype.replaceChild(newChild, oldChild)`: 자신을 호출한 노드의 자식 노드를 다른 노드로 교체

  - `newChild`: 교체할 새로운 노드
  - `oldChild`: 이미 존재하는 교체될 노드 (replaceChild 메서드를 호출한 노드의 자식 노드여야 함) <br/>
    ⇒ 자신을 호출한 노드의 자식 노드인 oldChild 노드를 newChild 노드로 교체 (oldChild 노드는 DOM에서 제거됨)

  ```jsx
  const $fruits = document.getElementById("fruits");

  // 기존 노드와 교체할 요소 노드 생성
  const $newChild = document.createElement("li");
  $newChild.textContent = "Banana";

  // #fruits 요소 노드의 1번째 자식 요소 노드를 $newChild 요소 노드로 교체
  $fruits.replaceChild($newChild, $fruits.firstElementChild);
  ```

<br/>

### 🔸 39.6.9\_노드 삭제

- `Node.prototype.removeChild(child)`: child 매개변수에 인수로 전달한 노드(removeChild 메서드를 호출한 노드의 자식 노드여야 함)를 DOM에서 삭제
  ```jsx
  // #fruits 요소 노드의 마지막 요소를 DOM에서 삭제
  $fruits.removeChild($fruits.lastElementChild);
  ```

<br/>

## ✅ 39.7\_어트리뷰트

### 🔸 39.7.1\_어트리뷰트 노드와 attributes 프로퍼티

- HTML 요소는 여러 개의 attribute(속성)를 가질 수 있음
- HTML attribute: HTML 요소의 동작을 제어하기 위한 추가적인 정보 제공
  - HTML 요소의 시작 태그에 `attribute name=”attribute value”` 형식으로 정의
  ```jsx
  <input id="user" type="text" value="hello">
  ```

✋ id, class, style 어트리뷰트는 모든 HTML 요소에 사용 가능한 반면, type, value, checked 어튜리뷰트는 input 요소에만 사용 가능

- HTML 문서가 파싱될 때 HTML 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드와 연결됨
  - 이때, HTML 어트리뷰트당 하나의 어트리뷰트 노드 생성됨
  - 이때 , 모든 어트리뷰트 노드의 참조는 유사 배열 객체 && iterable인 `NamedNodemap` 객체에 담겨서 요소 노드의 `attributes` 프로퍼티에 저장됨 <br/>
    ⇒ 요소 노드의 모든 어트리뷰트 노드는 요소 노드의 Element.prototype.attributes 프로퍼티로 취득 가능
    - `attributes` 프로퍼티: getter만 존재하는 읽기 전용 접근자 프로퍼티이며, NamedNodeMap 객체 반환

```jsx
const { attributes } = document.getElementById("user");
console.log(attributes);
// NamedNodeMap {0: id, 1: type, 2: value, id: id, type: type, value: value, length: 3}

// 어트리뷰트 값 취득
console.log(attributes.id.value); // user
consolelog(atrributes.type.value); // text
```

<br/>

### 🔸 39.7.2_HTML 어트리뷰트 조작

- `Element.prototype.getAttribute/setAttribute` 메서드를 사용하면 요소 노드에서 메서드를 통해 직접 HTML 어트리뷰트 값을 취득하거나 변경할 수 있어서 편리함!

  - `Element.prototype.getAttribute(attributeName)`: HTML 어트리뷰트 값 참조
  - `Element.prototype.setATtribute(attributeName, attributeValue)`: HTML 어트리뷰트 값 변경
  - `Element.prototype.hasAttribute(attributeName)`: 특정 HTML 어튜리뷰트 존재하는지 확인
  - `Element.prototype.removeAttribute(attributeName)`: 특정 HTML 어트리뷰트 삭제

  ```jsx
  const $input = document.getElementById("user");

  // value 어트리뷰트 값 취득
  const inputValue = $input.getAttribute("value");

  // value 어트리뷰트 값 변경
  $input.setAttribute("value", "foo");
  ```

<br/>

### 🔸 39.7.3_HTML 어트리뷰트 vs. DOM 프로퍼티

- 요소 노드 객체엔 HTML 어트리뷰트에 대응하는 프로퍼티가 존재함

  - 이 DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가지고 있음

    - DOM 프로퍼티: setter && getter 모두 존재하는 접근자 프로퍼티 ⇒ 참조 & 변경 가능

    ```jsx
    const $input = document.getElementById("user");

    // 요소 노드의 value 프로퍼티 값 변경
    $input.value = "foo";

    // 요소 노드의 value 프로퍼티 값 참조
    console.log($input.value); // foo
    ```

> Q: HTML 어트리뷰트는 아래와 같이 DOM에서 중복 관리되는걸까?
>
> 1.  요소 노드의 attributes 프로퍼티에서 관리하는 어트리뷰트 노드
> 2.  HTML 어트리뷰트에 대응하는 요소 노드의 프로퍼티 (DOM 프로퍼티)
>
> A: 🙅‍♀️ NO!!
> HTML 어트리뷰트 역할은 HTML 요소의 초기 상태를 지정하는 것 즉, HTML 요소의 초기 상태를 의미하며 이는 변하지 않음
>
> 요소 노드는 상태(state)를 가지고 있음
>
> → 요소 노드는 2개의 상태 즉 초기 상태와 최신 상태를 관리해야 함
>
> - 요소 노드의 초기 상태 : 어트리뷰트 노드가 관리
> - 요소 노드의 최신 상태 : DOM 프로퍼티가 관리

**‣ 어트리뷰트 노드**

- HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리
- 사용자 입력에 의해 상태가 변경되어도 초기 상태 그대로 유지

- 초기 상태 값 취득/변경 - `getAttribute`/`setAttribute` 메서드 사용
  - `getAttribute` 메서드로 취득한 값은 HTML 요소에 지정한 어트리뷰트 값인 초기 상태 값!
  - HTML 요소에 지정한 어트리뷰트 값은 사용자의 입력에 의해 변하지 않으므로 결과는 항상 동일!
  - `setAttribute` 메서드 - 초기 상태 값 변경
  ```jsx
  document.getElementById("user").getAttribute("value");
  document.getElementById("user").setAttribute("value", "foo"); // 초기 상태 값 변경
  ```

**‣ DOM 프로퍼티**

- 사용자가 입력한 최신 상태는 HTML 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티가 관리 <br/>
  ⇒ DOM 프로퍼티는 사용자 입력에 의한 상태 변화에 반응하여 항상 최신 상태 유지 <br/>
  ↔ getAttribute 메서드로 취득한 HTML 어트리뷰트 값인 초기 상태 값은 변하지 않고 유지

  ```html
  <input id="user" type="text" value="hello" />
  ```

  ```jsx
  const $input = document.getElementById('user');

  $input.oninput = () => {
  	console.log($input.value); // input 값 입력할 때마다 최신 상태 값 취득 => 값 동적 변경
  }

  console.log($input.getAttribute('value')); // 초기상태 값 유지

  // DOM 프로퍼티에 값 할당하여 HTML 요소의 최신 상태 변경
  $input.value = 'foo';
  console.log($input.value); // foo

  // 초기 상태 값은 변하지 않고 유지
  console.log($input.getAttribute('value'); // hello

  // 예외케이스도 존재하는데, id 어트리뷰트와 id 프로퍼티는 사용자 입력과 관계없이 항상 동일 값 유지
  $input.id = 'foo';
  console.log($input.id); // foo
  console.log($input.getAttribute('id')); // foo
  ```

🧚‍♂️ 사용자 입력에 의한 상태 변화와 관계있는 DOM 프로퍼티만 최신 상태 값 관리 (그 외의 관계 없는 값은 항상 동일 값으로 연동)

**‣ HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계**

- id 어트리뷰트와 id 프로퍼티는 1:1 대응하며, 동일한 값으로 연동
- input 요소의 value 어트리뷰트(초기 상태)는 value 프로퍼티(최신 상태)와 1:1 대응
- class 어트리뷰트는 className, classList 프로퍼티와 대응
- for 어트리뷰트는 htmlFor 프로퍼티와 1:1 대응
- td 요소의 colspan 어트리뷰트, textContent 프로퍼티는 대응하는 프로퍼티 존재 X
- 어트리뷰트 이름은 대소문자 구별 X But, 대응하는 프로퍼티 키는 카멜 케이스
  (e.g. maxlength → maxLength)

**‣ DOM 프로퍼티 값의 타입**

- getAttribute 메서드로 취득한 어트리뷰트 값은 항상 문자열

  - But, DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수도 있음

  ```jsx
  <input type="checkbox" checked/>

  const $checkbox = document.querySelector('input[type=checkbox]');

  // 항상 문자열
  console.log($checkbox.getAttribute('checked'); // ''

  // 문자열 아닐 수도 ~*
  console.log($checkbox.checked); // true
  ```

<br/>

### 🔸 39.7.4_data 어트리뷰트와 dataset 프로퍼티

- data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰와 JS 간 데이터 교환 가능

  - data 어트리뷰트는 `data-` 접두사 다음에 임의의 이름을 붙여 사용 (e.g. data-role)

  ```jsx
  <ul class="users">
    <li id="1" data-user-id="123" data-role="admin">
      CHOI
    </li>
    <li id="2" data-user-id="456" data-role="tester">
      KIM
    </li>
  </ul>
  ```

- data 어트리뷰트 값은 `HTMLElement.dataset` 프로퍼티로 취득 가능

  - dataset 프로퍼티: `DOMStringMap` 객체 반환
    - `DOMStringMap` 객체: `data-` 접두사 다음에 붙인 임의의 이름을 카멜 케이스로 변환한 프로퍼티를 가지고 있음 (이로 data 어트리뷰트 값을 취득/변경 가능)

  ```jsx
  const users = [...document.querySelector(".users").children];

  const user = users.find((user) => user.dataset.userId === "123");

  // data-role 값 취득
  console.log(user.dataset.role); // 'admin'

  // data-role 값 변경
  user.dataset.role = "tester";
  console.log(user.dataset); // DOMStringMap {userId: '123', role: 'tester'}
  ```

- data 어트리뷰트의 data- 접두사 다음에 존재하지 않는 이름을 키로 사용하여 dataset 프로퍼티에 값을 할당하면 HTML 요소에 data 어트리뷰트 추가됨

  - 카멜케이스 → 케밥케이스로 자동 변경되어 추가 (e.g. `fooBar` → `data-foo-bar`)

  ```jsx
  user.dataset.role = "admin";
  console.log(user.dataset);

  /*
  DOMStringMap {userId: '123', role: 'admin'};
  -> <li id="1" data-user-id="123" data-role="admin">CHOI</li>
  */
  ```

<br/>

## ✅ 39.8\_스타일

### 🔸 39.8.1\_인라인 스타일 조작

- `HTMLElement.prototype.style` : setter && getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 인라인 스타일을 취득하거나 추가 또는 변경

  ```jsx
  <div style="color : red">Hello World!</div>;

  // 인라인 스타일 취득
  console.log($div.style);

  // 인라인 스타일 변경
  $div.style.color = "blue";

  // 인라인 스타일 추가
  $div.style.backgroundColor = "yellow";
  ```

  - `CSSStyleDeclartaion` 타입 객체 반환

    - 카멜 케이스를 따름 (e.g. backgroundColor)

    ```jsx
    $div.style.backgrounColor = 'red;

    // 케밥 케이스 사용하려면 대괄호 표기법 사용
    $div.style['background-color'] = 'red';

    // 반드시 단위 지정 필요
    $div.style.width = '100px';
    ```

<br/>

### 🔸 39.8.2\_클래스 조작

- class 어트리뷰트에 대응하는 DOM 프로퍼티는 className & classList

**‣ className**

- `Element.prototype.className`: setter && getter 모두 존재하는 접근자 프로퍼티로서 HTML 요소의 class 어트리뷰트 값을 취득하거나 변경
  - class 어트리뷰트 값을 문자열로 반환하고, 요소 노드의 className 프로퍼티에 문자열을 할당하면 class 어트리뷰트 값을 할당한 문자열로 변경 <br/>
    😫 문자열을 반환하므로 공백으로 구분된 여러 개의 클래스를 반환하는 경우 다루기 불편

**‣ classList**

- `Element.prototype.classList` : class 어트리뷰트 정보를 담은 `DOMTokenList` 객체 반환

  ```jsx
  const $box = document.querySelector(".box");

  // classList가 반환하는 DOMTokenList 객체는 HTMLCollection, NodeList와 같이
  // 노드 객체 상태 변화를 실시간으로 반영하는 live 객체 ~*
  console.log($box.classList);
  // DOMTokenList(2) [length: 2, value: 'box blue', 0: 'box', 1: 'blue']
  ```

- `DOMTokenList` 객체: class 어트리뷰트 정보를 나타내는 컬렉션 객체로서 유사 배열 객체이면서 iterable
  - 유용한 메서드 제공
    - `add(…className)` : 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가
    - `remove(…className)` : 인수로 전달한 1개 이상의 문자열과 일치하는 클래스 삭제 (일치하는 클래스가 없으면 에러 없이 무시됨)
    - `item(index)` : 인수로 전달한 index에 해당하는 클래스를 class 어트리뷰트에서 반환
    - `contains(className)` : 인수로 전달한 문자열과 일치하는 클래스가 c포함되어 있는지 확인
    - `replace(oldClassName, newClassName)` : 첫번째 인수로 전달한 문자열을 두번째 인수로 변경
    - `toggle(className[, force])` : 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거 | 존재하지 않으면 추가
      - 2번째 인수: 조건식 전달 (true: 강제로 추가 | false: 강제로 제거)

<br/>

### 🔸 39.8.3\_요소에 적용되어 있는 CSS 스타일 참조

- style 프로퍼티는 인라인 스타일만 반환 <br/>
  ⇒ HTML 요소에 적용되어 있는 모든 CSS 스타일 참조해야 할 경우 `getComputedStyle` 메서드 사용

  ```jsx
  window.getComputedStyle($box);

  // 2번째 인수로 의사 요소 지정 문자열(e.g. :after, :before) 전달 가능 | 일반 요소의 경우 생략
  window.getComputedStyle($box, ":before");
  ```
