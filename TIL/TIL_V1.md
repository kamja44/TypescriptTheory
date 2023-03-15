1. [TS 기본 타입](#ts-기본-타입)<br>
   &nbsp;&nbsp;1-1. [Object의 타입 정의 1](#object의-타입-정의-1)<br>
   &nbsp;&nbsp;1-2. [Object의 타입 정의 2](#object의-타입-정의-2)<br>
   &nbsp;&nbsp;1-3. [함수의 return 타입 지정](#함수의-return-타입-지정)<br>
   &nbsp;&nbsp;1-4. [Arrow Function 사용 시 타입 지정 방법](#arrow-function-사용-시-타입-지정-방법)<br>
2. [readonly 속성을 타입에 추가](#readonly-속성을-타입에-추가)<br>
   &nbsp;&nbsp;2-1. [객체의 요소를 readonly로 설정](#객체의-요소를-readonly로-설정)<br>
   &nbsp;&nbsp;2-2. [배열을 readonly로 설정](#배열을-readonly로-설정)<br>
3. [Tuple](#tuple)<br>
   &nbsp;&nbsp;3-1. [Tuple과 readonly 같이 사용](#tuple과-readonly-같이-사용)<br>
4. [any 타입](#any-타입)<br>
5. [TS에만 존재하는 타입](#ts에만-존재하는-타입)<br>
   &nbsp;&nbsp;5-1. [unknown 타입](#unknown-타입)<br>
   &nbsp;&nbsp;5-2. [void 타입](#void-타입)<br>
   &nbsp;&nbsp;5-3. [never 타입](#never-타입)<br>
6. [call signatures](#call-signatures)<br>
   &nbsp;&nbsp;6-1. [나만의 call signature 선언 방법](#나만의-call-signature-선언방법)<br>
7. [Overloading](#overloading)<br>
8. [Polymorphism](#polymorphism)<br>
9. [Generic](#generic)<br>
10. [추상 클래스(abstract class), 추상 메소드](#추상-클래스abstract-class-추상-메소드)<br>
11. [private, protected, public 구분](#private-protected-public-구분)<br>
12. [해시맵 만들기](#해시맵-만들기)<br>
    &nbsp;&nbsp;12-1. [Object의 타입 선언하기](#object의-타입-선언하기)<br>
13. [interface](#interface)<br>
    &nbsp;&nbsp;13-1. [interface 상속](#interface-상속)<br>
    &nbsp;&nbsp;13-2. [추상클래스를 인터페이스로 바꾸기](#추상클래스를-인터페이스로-바꾸기)<br>
    &nbsp;&nbsp;&nbsp;&nbsp;13-2-1. [implements]<br>
    &nbsp;&nbsp;13-3. [interface, class를 타입으로 사용하기](#interface-class를-타입으로-사용하기)

# TS 기본 타입

- 타입은 항상 변수명 뒤에 콜론을 작성한 후 작성한다.

배열 : 자료형[]
숫자 : number
문자열: string
논리: boolean

## Object의 타입 정의 1

```js
const player: {
  name: string,
  age?: number, // optional 사용 player 객체에 age가 없어도 에러 발생 x
  // age의 타입은 number | undefined;
} = {
  name: "kamja",
};
if (player.age && player.age < 10) {
  // age가 optional 타입이라 undefined일 수 있기에 player.age &&를 사용하여 player 객체에 age 요소가 있을 때만 실행한다.
}
```

## Object의 타입 정의 2

-Alias 타입 사용(별칭 사용)

```js
type Player = {
  name: string,
  age?: number,
};
const player: Player = {
  name: "kamja",
};
```

## 함수의 return 타입 지정

- Alias 타입 사용(별칭 사용)

```js
type Player = {
  name: string,
  age?: number,
};
function playerMaker(name: string): Player {
  // playerMaker 함수는 name을 string 타입으로 받는다.
  return {
    name,
  };
}
const kamja = playerMaker("kamja");
kamja.age = 20;
```

## Arrow Function 사용 시 타입 지정 방법

- Alias 타입 사용(별칭 사용)

```js
type Player = {
  name: string,
  age?: number,
};
const playerMaker = (name: string): Player => {
  return {
    name,
  };
};
```

# readonly 속성을 타입에 추가

- 변수명 앞에 readonly를 추가하면 된다.
- readonly를 추가하면 변수는 불변성을 갖는다.

## 객체의 요소를 readonly로 설정

```js
type Player = {
  readonly name: string,
  age?: number,
};
const playerMaker = (name: string): Player => {
  return {
    name,
  };
};
const kamja = playerMaker("kamja");
kamja.name = "kokumi"; // Error kamja 객체의 name은 readonly이기에 수정이 불가능하다.
```

## 배열을 readonly로 설정

```js
const numbers: readonly number[] = [1,2,3,4];
numbers.push(1); // Error numbers 배열은 readonly이기에 수정이 불가능하다.

const names: readonly string[] = ["1","2"];
names.push("kamja"); // Error names 배열은 readonly이기에 수정이 불가능하다.
```

# Tuple

- array를 생성할 수 있게 한다.

  - 최소한의 길이를 가져야 하며
  - 특정 위치에 특정 타입이 있어야 한다.

- 최소 3개의 아이템을 가지며 string, number, boolean 타입을 순서를 가지는 tuple 생성 예제

```js
const player: [string, number, boolean] = ["kamja", 1, true];
player[0] = 1; // Error player배열의 0번째 index는 string 타입이기 때문
// 즉, string타입이 들어와야하는데 number타입으로 수정하려 하니 에러가 발생한다.
```

**[string, number, boolean]** 이 tuple이다.

- tuple은 항상 정해진 갯수의 요소를 가져야 하는 array를 지정할 때 유용하다.

## Tuple과 readonly 같이 사용

```js
const player: readonly [string, number, boolean] = ["kamja", 1, true];
player[0] = "hi" // Error player배열이 readonly이기 때문이다.
// 즉, string타입으로 값을 수정해도 readonly이기에 에러가 발생한다.
```

# any 타입

- any타입은 TS의 모든 보호장치를 비활성화시킨다.

# TS에만 존재하는 타입

## unknown 타입

- unknown 타입을 사용하기 위해선 먼저 타입을 체크해야 한다.

```js
let a: unknown;
let b = a + 1; // Error unknown타입인 a를 사용하기 위해선 먼저 a의 타입을 체크해야 한다.
if (typeof a === "number") {
  let b = a + 1;
}
if (typeof a === "string") {
  let b = a.toUpperCase();
}
```

## void 타입

- 아무것도 return하지 않는 함수를 대상으로 사용한다.
  - 함수가 아무것도 return하지 않을 경우 TS가 자동으로 함수를 void 타입으로 추론한다.

## never 타입

- 함수가 절대 return하지 않을 때 발생한다.

- never 타입 사용 예 1

```js
function hello(): never {
  // return "x" // never 타입은 함수가 return 값을 가질 수 없다.
  throw new Error("xxx"); // hello 함수가 return값을 갖지 않으니 에러가 발생하지 않는다.
}
```

- never 타입 사용 예 2

```js
function hello(name: sring | number) {
  // name은 string, number 2가지 타입을 가질 수 있다.
  if (typeof name === "string") {
  } else if (typeof name === "number") {
  } else {
    name; // name의 타입이 string, number 둘 다 아니므로 name의 타입은 never이다.
    // 즉, 타입이 올바르게 들어왔다면 else문은 절대 실행되지 않는다.
  }
}
```

# call signatures

- 함수 위에 마우스를 올렸을 때 보게 되는 것을 말한다.
  - call signatures는 함수를 어떻게 호출해야 하는지 알려준다.
    - 즉, 함수의 argument 타입과 함수의 반환 타입을 알려준다.

```js
const add = (a: number, b: number) => a + b;
// 위의 함수에 마우스를 올리면  `cosnt add: (a: number, b: number) => number `이 보인다. 이것이 call signatures이다.
// 즉, 함수를 call signatures 방식으로 호출해야 하는 것을 알려준다.
```

## 나만의 call signature 선언방법

- 함수의 call signature 타입 생성

```js
type Add = (a: number, b: number) => number; // call signature 타입 생성

const add: Add = (a, b) => a + b; // TS에게 add 함수의 a와 b의 argument가 number 타입이라고 명시할 필요가 없다.
// Add 타입을 이용하여 call signature를 선언했기 때문
```

# Overloading

- 함수가 여러개의 call signatures를 가지고 있을 때 발생한다.

- 오버로딩 예 1

```js
type Add = {
    (a: number, b: number) : number
    (a: number, b: string) : number
}
const add: Add = (a, b ) => {
    // b는 Add 타입의 정의에 의해 number or string이다.
    if(typeof b === "string") return a; // b의 타입이 string일 경우의 동작을 정의한다.
    return a + b; // b의 타입이 number일 경우의 동작을 정의한다.
}
```

- 오버로딩 예 2

```js
type Config = {
  path : string,
  state : object
}
type Push = {
  (path: string):void
  (config: Config) : void
}

const push: Push = (config) => {
  if(typeof config === "string"){
    console.log(config) // config 매개변수가 string 타입일 경우의 동작
  }else{
    console.log(config.path) // config 매개변수가 Config 타입 객체일 경우의 동작
    // 즉, config 매개변수는 path와 state 값을 가질 수 있다.
  }
}
```

- 여러개의 argument를 가지고 있을 때의 오버로딩
  - 즉, 파라미터의 개수가 다를때 일어나는 경우

```js
type Add = { // Add 타입은 서로 다른 call signatures와 파라미터의 개수도 다르다.
  (a: number, b: number): number
  (a: number, b: number, c: number): number
}

// add 함수를 호출할 때 a와 b는 공통으로 호출하지만 c는 옵션이다. 즉, c의 옵션을 명시적으로 지정해줘야 오류가 발생하지 않는다.
// c ?: number; c의 타입을 옵셔널로 지정하며 number 타입으로 지정한다.
const add: Add = (a, b, c ?: number) => {
  if(c) return a + b + c;
  return a + b;
}
add(1, 2)
add(1, 2, 3)
```

# Polymorphism

- 여러가지 다른 구조들

- 다형성 예제

```js
type SuperPrint = {
  (arr: number[]): void
  (arr: boolean[]): void
}
const superPrint: SuperPrint = (arr) => {
  arr.forEach(i => console.log(i));
}
superPrint([1,2,3,4]);
superPrint([true, false, true]);
```

# generic

- 타입의 placeholder와 비슷하다
  - 즉, TS가 타입을 유추한다.
    - call signature를 작성할 때, call signature에 들어올 확실한 타입을 모를 때 generice을 사용한다.

generic 사용예제

```js
type SuperPrint = {
  <TypePlaceholder>(arr: TypePlaceholder[]): TypePlaceholder,
  // TS가 TypePlacehoder자리에 추론한 타입을 넣는다.
};
const superPrint: SuperPrint = (arr) => arr[0];
superPrint([1, 2, 3, 4]);
superPrint([true, false, true]);
superPrint(["a", "b", "c"]);
superPrint([1, 2, true, false, "a"]);
```

- TS는 generic이 처음 사용되는 지점을 기반으로 타입이 무엇인지 추론한다.

# Class

- TS는 파라미터들을 써주기만 하면 TS 가 알아서 Constructor 함수를 만들어준다.

```js
class Player{
  constructor(
    private firstName: string,
    private lastName: string
  ){}
}
```

- 위의 TS코드는 아래의 JS 코드로 컴파일된다.

```js
"use strict";
calss Player{
  constructor(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
  }
}
```

```js
class Player{
  constructor(
    private firstName: string,
    private lastName: string,
    public nickname: string
  ){}
}
const kamja = new Player("k","a","mja");
// firstName과 lastName은 private이므로 외부에서 접근이 불가능하다.
// nickname은 public이므로 외부에서 접근이 가능하다.
kamja.firstName // Error
kamja.lastName // Error
kamja.nickname // 가능
```

# 추상 클래스(abstract class), 추상 메소드

- 추상클래스는 다른 클래스가 상속받을 수 있는 클래스이다.

  - 추상 클래스는 직접 새로운 인스턴스를 만들 수 없다.
    - 즉, 추상 클래스는 상속만 받을 수 있다.

- 추상 메소드는 추상 클래스를 상속받는 모든 것들이 구현을 해야 하는 메소드를 의미한다.

```js
abstract class User{
constructor(
    private firstName: string,
    private lastName: string,
    public nickname: string
  ){}
  // 추상 메소드
  // 메소드는 클래스 안에 존재하는 함수이다.

  getFullName(){// getFullName()을 private으로 선언하면 외부에서 사용할 수 없다.
    return `${this.firstName} ${this.lastName}`
  } // Player 클래스는 User를 상속받았으므로 getFullName()을 사용할 수 있다.
}
class Player extends User{// User 추상 클래스는 getFullName 추상 메서드를 가지고 있기에 Player 클래스는 getFullName을 구현해야 한다.
}
const kamja = new Player("k","a","mja");
const kokuma = new User("ko","ku","ma"); // Error 추상 클래스는 직접 인스턴스를 생성할 수 없다.
kamja.getFullName() // Player 클래스는 User 추상 클래스를 상속받았으므로 getFullName()을 사용할 수 있다.
```

# private, protected, public 구분

|           | 선언한 클래스 내 | 상속받은 클래스 내 | 인스턴스 |
| :-------: | :--------------: | :----------------: | :------: |
|  private  |        ⭕        |         ❌         |    ❌    |
| protected |        ⭕        |         ⭕         |    ❌    |
|  public   |        ⭕        |         ⭕         |    ⭕    |

`추상 클래스, 추상 메소드, protected, public, private 등은 TS에서만 제공하는 기능들이다.`

- 즉, JS로 컴파일 되면 추상 클래스로 인스턴스를 생성할 수 있다.

# 해시맵 만들기

## Object의 타입 선언하기

```js
// Object의 타입을 선언해야할 때 사용한다.
type Words = { // Words 타입이 string만을 property로 가지는 오브젝트라고 명시한다.
  [key: string] : string // 제한된 양의 property 혹은 key를 가지는 타입을 정의해 주는 방법이다.
}

class Dict{ // 사전이 words 오브젝트를 가진다고 알려준다.
  private words: Words // Constructor 에서 직접 초기화 되지 않는 property
  constructor(){
    this.words = {} // constructor에서 words를 수동으로 초기화
  }
  add(word: Word){ // Word 클래스를 타입처럼 사용
  // 즉, word 클래스를 타입으로 사용
    if(this.words[word.term] === undefined){
      this.words[word.term] = word.def;
    }
  }
  def(term: string){
    return this.words[term]
  }
}

// 단어를 만드는 Word 클래스
class Word{
  constructor(
    public trem: string,
    public def: string
  ){}
}
const kamja = new Word("kamja", "감자"); // 단어 생성
Dict.add(kamja); // 생성한 dictionary에 단어 추가
Dict.def("kamja"); // kamja 단어 찾기
```

# interface

- 객체의 모양을 특정해주기 위해 사용한다.
- 인터페이스를 여러개 생성하면 TS가 하나로 합쳐준다.
  - 즉, 같은 인터페이스에 다른 이름을 가진 property들을 쌓을 수 있다.

```js
// type만 가능한 문법
type Team = "red" | "blue" | "yellow"; // 타입 Team은 red, blue, yellow 3개 중 하나의 값만 가질 수 있다.
type Health = 1 | 5 | 10; // 타입 Health은 1, 5, 10 3개 중 하나의 값만 가질 수 있다.

// type 사용
type kamja = string;
type Player = {
  nickname: string,
  team: string,
  health: nubmer,
};
const kokuma: kamja = "potato";

// interface 사용
interface Player {
  nickname: string;
  team: string;
  health: nubmer;
}
const kamja: Player = {
  nickname: "kamja",
  team: "kokuma",
  health: 1,
};
```

## interface 상속

```js
interface User {
  name: string;
}
interface Player extends User {}
const kamja: Player = {
  name: "kamja",
};
```

복습

```js
// 추상 클래스
// 추상 클래스는 인스턴스를 생성할 수 없다. -> new User 불가능 -> 왜? User가 추상클래스이기 때문에
// 추상클래스는 User 클래스를 상속 받는 다른 클래스가 가질 property와 메소드를 지정해준다.
abstract class User{
  constructor(
    // 변수를 protected로 설정하면 상속받은 클래스는 접근할 수 있다.
    protected firstName: string,
    protected lastName: string
  ){}
  abstract sayHi(name: string): string // 추상메서드
  abstract fullName(): string // 추상메서드
}
// 즉, User 추상클래스를 상속한다면 추상메서드인 sayHi와 fullName을 구현해야 하고 firstName과 lastName을 갖는다.

class Player extends User{
  fullName(){
    return `${this.firstName} ${this.lastName}`
  }
  sayHi(name: string){
    return `Hello ${name}. My name is ${this.fullName()}`
  }
}
```

## 추상클래스를 인터페이스로 바꾸기

- 인터페이스는 생성자와 추상메서드가 없다.
- 하지만, 인터페이스는 오브젝트나 클래스의 모양을 묘사할 수 있다.

### implements

- implements는 인터페이스를 상속해야 한다는 것을 의미한다.
  - 인터페이스를 상속할 때는 property를 private, protected으로 만들지 못한다.
    - property를 public으로 만들어야 한다.
  - `여러개의 인터페이스도 상속할 수 있다.`
- 기존의 코드는 interface를 이용하여 상속을 알렸는데 implements를 이용하여 상속을 알릴 수 있다.
- implement는 JS에서 사용할 수 없는 문법이기에 코드가 가벼워진다. -> TS 컴파일러가 JS로 컴파일 하지 않는다.

```js
interface User{
  firstName: string,
  lastName: string,
  sayHi(name: string): string
  fullName(): string
}
interface Human{
  health: number
}
class Player implements User, Human{
  constructor(
     firstName: string,
     lastName: string,
     public health: number
  ){}
    fullName(){
    return `${this.firstName} ${this.lastName}`
  }
  sayHi(name: string){
    return `Hello ${name}. My name is ${this.fullName()}`
  }
}
```

## interface, class를 타입으로 사용하기

```js
interface User{
  firstName: string,
  lastName: string,
  sayHi(name: string): string
  fullName(): string
}
interface Human{
  health: number
}
class Player implements User, Human{
  constructor(
     firstName: string,
     lastName: string,
     public health: number
  ){}
    fullName(){
    return `${this.firstName} ${this.lastName}`
  }
  sayHi(name: string){
    return `Hello ${name}. My name is ${this.fullName()}`
  }
}

// interface를 타입으로 사용하기
function makeUser(user: User): User{ // 결과값으로  interface 리턴
  return {
  firstName: "kamja",
  lastName: "las",
  fullName: () => "xxxx",
  sayHi: (name) => "string"
}
}
makeUser({
  firstName: "kamja",
  lastName: "las",
  fullName: () => "xxxx",
  sayHi: (name) => "string"
})

// class를 타입으로 사용하기
function makePlayer(player: Player){

}
```

# 다형성, 제네릭, 클래스, 인터페이스 모두 복습

다형성

- 다른 모양의 코드를 가질 수 있게 해준다.

제네릭

- palceholder 타입을 쓸 수 있도록 해준다.
  - 사용자가 사용한 타입을 TS가 추론하여 concrete 타입으로 변형해준다.

# 로컬 스토리지 API와 비슷한 API 생성

Storage

- 기존에 존재하는 인터페이스

```js
interface SStorage<T> {
  [key: string]: T
}
class LocalStorage<T>  { // LocalStorage를 초기화할 때 TS에게 T라고 불리는 제네릭을받는다.
  // 클래스에서 받은 제네릭을 interface(SStorage)에 보내줄 수 있다.
  private storage : SStorage<T> = { }
  set(key: string, value: T){
    this.storage[key] = value;
  }
  remove(key: string){
    delete this.storage[key]
  }
  get(key: string): T{ // return type이 generic(T)
    return this.storage[key]
  }
  clear(){ // sotrage 비우기
    this.storage={}
  }
}

// 클래스 사용

// string 타입의 class
const stringStorage = new LocalStorage<string>() // string 타입의 class를 사용한다고 명시
stringStorage.get("kamja"); // string을 보내주고 string을 받는다.
stringStorage.set("hello", "how are you")

// boolean 타입의 class
const booleanStorage = new LocalStorage<boolean>(); // boolean 타입의 class를 사용한다고 명시
booleanStorage.get("xxx") // string을 보내주고 boolean을 받는다.
booleanStorage.set("hello", true)
```
