# 2.0

타입스크립트를 자바스크립트로 변환하는 이유

- 브라우저가 TS가 아닌 JS를 인식하기 때문

타입스크립트 코드 테스트 페이지

- https://www.typescriptlang.org/play

# 2.1

- 데이터와 변수의 타입을 명시적으로 정의 가능
  - 변수가 비어있을때만 사용(타입을 추론한 코드와 결과가 동일하다.)
    - 즉, 타입스크립트가 타입을 추론하지 못할 때 사용한다.

```js
ㅣet b : boolean = true
```

- JS처럼 변수만 선언해도됨
  - 타입스크립트가 타입을 추론함

```js
let a = "hello"; // TS가 a의 타입을 string으로 추론한다.
```

# 2.2

여러가지 타입들(타입 명시적으로 정의하는 방법)

```js
let a: number[] = [1, 2]; // 숫자형 배열
let b: string[] = ["i1", "1"]; // 문자형 배열
let c: boolean[] = [true]; // 불리언형 배열

let d: number = 1; // 숫자형
let e: string = "kamja"; // 문자형
let f: boolean = true; // 불리언형
```

여러가지 타입들(TS가 타입 추론)

```js
let a = [1, 2]; // 숫자형 배열
let b = ["i1", "1"]; // 문자형 배열
let c = [true]; // 불리언형 배열

let d = 1; // 숫자형
let e = "kamja"; // 문자형
let f = true; // 불리언형
```

객체에서 타입 명시 방법 및 사용법

```js
// 타입 명시
const player : {
    name : string, // name 속성은 필수이다. 즉, player 객체에 name 속성은 항상 들어있어야한다.
    age ?: number // age 속성은 선택적이다. 즉, player 객체에 age 속성은 들어있어도 되고 없어도 된다. 단, 들어있다면 age의 속성은 number타입이다.
} = {
    name : "kamja";
}

// 명시한 타입 사용
if(player.age < 10) // error발생 <- player객체에서 age 속성은 undefined or number이기 때문(age ?: number)
// 위 코드를 사용하려면 아래의코드를 이용해야 한다.
if(palyer.age && player.age < 10)
```

`별칭(Alias)`

- 동일한 타입을 여러번 명시해야 할 경우 사용한다.
  - 즉, 코드의 재사용이 가능하다.

```js
type player = {
  name: string,
  age?: number,
};
const kamja: player = {
  name: "kamja",
  age: 10,
};
const kokuma: player = {
  name: "kokuma",
};
```

과도한 코드의 재사용

- 코드는 과하게 재사용하는 것이 아니다.

```js
type Age = number;
type Name = string;
type Player = {
  name: Name,
  age: Age,
};
```

`함수의 return 값의 타입 지정`

```js
type Age = number;
type Name = string;
type Player = {
  name: Name,
  age: Age,
};
// name : string <- argument로 받는 name의 타입이 string이다.
const playerMaker = (name: string): Player => {
  // return 타입을 Player(별칭)으로 지정한다.
  return {
    // name : name 이 코드는 아래의 코드와 동일하다 (ES6 문법)
    name,
  };
};
```

# 2.3

`속성들을 읽기전용(readOnly) 타입으로 지정하기`

- 즉, readOnly로 타입이 지정되면 불변성(immutability)을 가진다.

```js
type Age = number;
type Name = string;
type Player = {
  readonly name : Name, //
  age ?: Age,
};
const playerMaker = (name: string): Player => {
  return {
    name,
  };
};
const kamja = playerMaker("kamja")
kamja.age = 12;
kamja.name = "kokuma" // Error 발생 name 속성은 readOnly이기 때문 Cannot assign to 'name' because it is a read-only property.

const numbers : readonly number[] = [1,2,3,4];
numbers.push(1); // Error 발생 numbers 배열을 readOnly이기 때문 Property 'push' does not exist on type 'readonly number[]'.
```

`Tuple`

- 자바스크립트에는 Tuple없다.
- array를 생성할 수 있게 해준다.
- 최소한의 길이를 가져야 한다.
- 특정 위치에 특정 타입이 있어야 한다.
- 즉, Tuple을 사용하면 항상 정해진 갯수의 요소를 가져야 하는 array를 지정할 수 있다.
  Tuple 예

```js
const player: [string, number, boolean] = ["kamja", 10, false];
player[0] = 1; // Error 발생 player배열의 0번째 인덱스는 string 타입이기 때문 Type 'number' is not assignable to type 'string'.
```

`Tuple과 readOnly(읽기전용) 결합`

```js
// 변경할 수 없는 player tuple 생성
const player : readonly [string, number, boolean] = ["kamja", 1, false];
player[0] = "kokuma" // Error 발생 player배열의 0번째 인덱스는 readOnly이기 때문 Cannot assign to '0' because it is a read-only property.
```

`any, undefined, null 타입`

- any 타입
  - 모든 타입이 허용된다.
  - TS의 보호장치들로부터 빠져나오고 싶을 때 사용한다.
    - 즉, TS가 아닌 JS형태로 사용하고 싶을 때 사용한다.
      - TS의 보호장치가 적용되지 않는다.
      - any 타입의 사용을 최대한 지양해야 한다.

```js
let a: undefined = undefined; // 선택적 타입(?:)은 null이 될 수 있다.
let b: null = null;

//any 타입 <- 기본적으로 비어있는 타입이다.
let a = []; // any 타입

const a: any[] = [1, 2, 3, 4];
const b: any = true;
a + b; // '1,2,3,4true' 출력 -> 타입을 any로 선언해서 TS의 보호장치를 사용하지 않았기에 가능
```
