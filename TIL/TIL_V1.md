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
