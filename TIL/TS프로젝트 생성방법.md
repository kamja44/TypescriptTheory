수동으로 TS 프로젝트를 생성하는 방법이다.

- 웹팩을 사용하는것과 비슷하다.
  - reactJS, next JS 등 프레임워크는 자동으로 TS를 설정한다.

1. cmd의 경로를 프로젝트 경로(typeChain)로 이동 후 명령어 입력
   `npm init -y`
2. package.json 수정

```json
{
  "name": "typechain",
  "version": "1.0.0",
  "description": "",
  "scripts": {},
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

3. 타입스크립트 설치
   `npm install -D typescript`

- -D는 타입스크립트를 devDependencies에 설치한다.

4. tsconfig.json 파일 생성

- tsconfig.json 파일이 있어야 TS로 작업하는것을 알 수 있다.

5. tsconfig.json 파일 설정

- TS는 컴파일러이다. 즉, TS파일들을 일반적인 JS로 컴파일 시켜준다.

```json
{
  "include": ["src"],
  // include의 배열에는 JS로 컴파일하고 싶은 모든 디렉터리를 넣어준다. 즉, TS가 src의 모든 파일을 확인하는 것을 의미한다.
  "compilerOptions": {
    "outDir": "build",
    // outDir은 JS 파일이 생성될 디렉터리를 지정한다. 즉, 컴파일된 JS 코드를 build라는 폴더에 넣는다고 명시한다.
    "esModuleInterop" : true,
    // ES6 모듈 사양을 준수하여 CommonJS 모듈을 가져올 수 있게 된다.
    "target": "ES6",
    // tager은 어떤 버전의 JS로 TS를 컴파일할지 지정할 수 있다.
    "lib": ["ES6", "DOM"],
    // lib는 합쳐진 라이브러리의 정의 파일을 특정해주는 역할을 한다. lib는 ES6를 사용하며, 브라우저 위에서 실행한다고 특정한다. 즉, TS는 개발자가 사용할 API가 무엇인지 알기 때문에 자동완성 기능을 제공할 수 있다.
    // lib 속성은 TS에게 어떤 API를 사용하고 어떤 환경에서 코드를 실행하는지 말해준다.
    "strict" ; true,
    "allowJs" : true,
    // TS 안에 JS를 허용한다.
  }
}
```

- package.json 파일에 script에 build 스크립트를 생성한다.

```json
{
  "name": "typechain",
  "version": "1.0.0",
  "description": "",
  "scripts": {
    "build": "tsc"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "typescript": "^4.9.5"
  }
}
```

6. npm run build 명령어를 사용하여 script를 실행한다.

- build 폴더가 생긴걸 확인할 수 있다.

7. myPackage.js 파일 생성

8. tsconfig.json 파일 설정

```js
{
  "include": ["src"],
  "compilerOptions": {
    "outDir": "build",
    "target": "ES6",
    "lib": ["ES6", "DOM"],
    "strict" : true,
    // strict를 true로 설정하면 TS가 코드를 보호할 수 있다. 즉, 정의 파일이 없는 경우에도 에러를 발생시킨다.
  }
}
```

`정의 파일`

- 정의 파일은 JS코드의 모양을 TS에 설명해주는 파일이다.
  - 정의 파일은 d.ts에 작성한다.
    - myPackage.d.ts라는 파일을 만든 후 정의 파일을 작성한다.

9. 정의 파일 작성(myPackage.d.ts)

`TS로 JS파일 확인하기`

- JSDoc 사용

JSDoc

- 코멘트로 이뤄진 문법

JSDoc 사용법

```js
// @ts-check
/**
 * Initializes the project
 * @param {object} config
 * @param {boolean} config.debug
 * @param {string} config.url
 * @returns {void}
 */
```

- TS가 위의 comment를 읽고 타입을 체크한다.
