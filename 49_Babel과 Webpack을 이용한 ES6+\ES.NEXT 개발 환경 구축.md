# ☑️ 49_Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축

p.900~917

✍️ 2025.01.20(Mon), 2025.02.07(Fri)

- IE를 포함한 구형 브라우저에서 문제 없이 동작시키기 위한 개발 환경을 구축하는 것이 필요
- 대부분의 프로젝트가 모듈을 사용하므로 모듈 로더도 필요

  - ES6 모듈(ESM)은 대부분의 모던 브라우저(Chrome, FF, SF, Edge)에서 사용 가능하지만 아래와 같은 이유로 아직까진 별도의 모듈 로더를 사용하는 것이 일반적
    1. IE를 포함한 구형 브라우저는 ESM을 지원하지 않음
    2. ESM을 사용하더라도 트랜스파일링이나 번들링 필요
    3. ESM이 아직 지원하지 않는 기능 존재

- `Babel`: 트랜스파일러(transpiler)
- `Webpack`: 모듈 번들러(module bundler)

<br/>

## ✅ 49.1_Babel

- `Babel`: ES6 최신 사양의 소스코드를 구형 브라우저에서도 동작하는 ES5 사양의 소스코드로 변환(트랜스파일링) 가능

<br/>

### 🔸 49.1.1_Babel 설치

```bash
# package.json 생성
$ npm init -y

# babel-core, babel-cli 설치
$ npm install --save-dev @babel/core @babel/cli

# version 지정 설치
$ npm install --save-dev @babel/core@7.10.3
```

<br/>

### 🔸 49.1.2_Babel 프리셋 설치와 babel.config.json 설정 파일 작성

- Babel을 사용하려면 `@babel/preset-env` (함께 사용되어야 하는 Babel 플러그인 모아 둔 것; Babel 프리셋) 설치해야 함
  - @babel/preset-env
  - @babel/preset-flow
  - @babel/preset-react
  - @babel/preset-typescript

```bash
$ npm install --save-dev @babel/preset-env
```

```json
// 설치 완료 후 package.json

{
  "name": "esnext-project",
  "version": "1.0.0",
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/present-env": "^7.10.3"
  }
}
```

```json
// babel.config.json
// @babel/preset-env 사용하겠다 ~

{
  "presets": ["@babel/preset-env"]
  // 설치한 플러그인 사용시 해당 부분에 위와 같이 추가!
}
```

<br/>

### 🔸 49.1.3\_트랜스파일링

```json
// package.json

{
	...
	// npm scripts에 Babel CLI 명령어 등록
	"script": {
		"build": "babel src/js -w -d dist/js"
		// => src/js 폴더에 있는 모든 JS 파일들을 트랜스파일링한 후, 그 결과물을 dist/js 폴더에 저장
	}
}

// -w (--watch) : 타킷 폴더에 있는 모든 JS 파일들의 변경을 감지하여 자동으로 트랜스파일
// -d (--out-dir) : 트랜스파일링된 결과물이 저장될 폴더 지정 | 지정된 폴더 존재 X -> 자동 생성
```

<br/>

### 🔸 49.1.4_Babel 플러그인 설치

- 설치가 필요한 Babel 플러그인은 [Babel 홈페이지](https://babeljs.io/)에서 검색 후 설치

<br/>

### 🔸 49.1.5\_브라우저에서 모듈 로딩 테스트

- 브라우저는 CommonJS 방식의 require 함수 지원 X → 브라우저에서 트랜스파일링된 결과 그대로 실행하면 에러 발생
  ⇒ Webpack을 통해 문제 해결 가능

<br/>

## ✅ 49.2_Webpack

- `Webpack`: 의존 관계에 있는 JS, CSS, 이미지 등의 리소스들을 하나의 파일로 번들링하는 모듈 번들러 <br/>
  ⇒ 👍 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요 없어짐 <br/>
  ⇒ 👍 여러 개의 JS 파일을 하나로 번들링하므로 HTML 파일에서 script 태그로 여러 개의 JS 파일을 로드해야 하는 번거로움도 사라짐

<br/>

### 🔸 49.2.1_Webpack 설치

```bash
$ npm install --save-dev webpack webpack-cli
```

```json
// 설치 완료 후 package.json

{
	"name": "esnext-project",
	"version": "1.0.0",
	"devDependencies": {
		...
		"webpack": "^4.43.0",
		"webpack-cli": "^3.3.12"
	}
}
```

<br/>

### 🔸 49.2.2_babel-loader 설치

```bash
$ npm install --save-dev babel-loader
# webpack이 모듈 번들링할 때 Babel을 사용하여
# ES6+/ES.NEXT -> ES5 사양의 소스코드로 트랜스파일링하도록 babel-loader 설치
```

```json
// package.json

{
	...
	"script": {
		"build": "webpack -w" // Babel 대신 Webpack 실행하도록 수정
	}
}
```

<br/>

### 🔸 49.2.3_webpack.config.js 설정 파일 작성

```jsx
// webpack.config.js
// webpack이 실행될 때 참조하는 설정 파일

const path = require('path');

module.exports = {
	entry: './src/js/main.js',
	output: {
		// 번들링된 js 파일의 이름(filename) & 저장될 경로(path) 지정
		path: path.resolve(__dirname, 'dist'),
		filename: 'js/bundle.js'
	},
	module: {
		rules: [
			{
				test: ^.js$/,
				include: [path.resolve(__dirname, 'src/js'],
				exclude: /node_modules/,
				use: {
					loader: 'babel-loader',
					options: {
						presets: ['@babel/preset-env'],
						plugins: ['@babel/plugin-proposal-class-properties']
					}
				}
			}
		]
	},
	devtool: 'source-map',
	mode: 'development'
}
```

⇒ `npm run build` 로 webpack 실행시 `dist/js` 폴더에 `bundle.js` 생성 ← `main.js`, `lib.js` 모듈이 하나로 번들링된 결과물

```jsx
// index.html

<!DOCTYPE html>
<html>
<body>
	<script src="./dist/js/bundle.js"></script>
</body>
</html>
```

<br/>

### 🔸 49.2.4_babel-polyfill 설치

- `@babel/polyfill`: `Promise`, `Object.assign`, `Array.from` 등과 같은 객체나 메서드 사용을 위해서 설치

```bash
$ npm install @babel/polyfill
```

```json
// 설치 완료 후 package.json

{
	"name": "esnext-project",
	"version": "1.0.0",
	"devDependencies": {
		...
	},
	"dependencies": {
		"@babel/polyfill": "^7.10.1"
		// 🖐️ @babel/polyfill은 개발환경뿐만 아니라 실제 운영 환경에서도 사용해야 하므로 --save-dev 옵션 지정 X
	}
}
```

**‣ polyfill 사용법**

```jsx
// src/js/main.js

import "@babel/polyfill"; // 진입점 선두에 로드
...
```

- webpack 사용시 위 방법 대신 webpack.config.js 파일의 entry 배열에 폴리필 추가

```jsx
// webpack.config.js

const path = require('path');

module.exports = {
	entry: ['@babel/polyfill', './src/js/main.js'],
	...
}
```
