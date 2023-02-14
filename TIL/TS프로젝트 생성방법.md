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
    "target": "ES6"
    // tager은 어떤 버전의 JS로 TS를 컴파일할지 지정할 수 있다.
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
