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

# 2.4

`TS에서만 존재하는 타입`

1. unknown

- 변수가 어떤 타입인지 모를경우 사용한다.
  - 변수를 사용할 때 변수의 타입을 먼저 확인하는 과정이 필요하다.

`unknown 타입 사용`

```js
let a: unknown;
let b = a + 1; // Error 발생 a의 타입이 unknown이다. 'a' is of type 'unknown'.
if (typeof a === "number") {
  // a변수의 타입이 number인지 확인
  let b = a + 1; // if문 안에서는 a는 number이기에 에러가 발생하지 않는다.
}
if (typeof a === "string") {
  // a변수의 타입이 string인지 확인
  let b = a.toUpperCASE();
}
```

2. void

- 아무것도 return하지 않는 함수를 대상으로 사용한다. - TS가 함수가 아무것도 return 하지 않는다는 것을 자동으로 추론한다. - 즉, 명시적으로 void 타입을 선언할 필요가 없다.
  `void 타입 사용`

```js
// TS의 타입 추론
function hello() {
  // function hello(): void
  console.log("void");
}
// void타입 명시적 선언
function hi(): void {
  console.log("void");
}
// void타입은 비어있는걸 의미한다. 즉, 아래와 같은 작업은 에러가 발생한다.
hello().toUpperCase(); // hello() 함수가 아무것도 반환하지 않는 void 타입이기에 에러 발생 Property 'toUpperCase' does not exist on type 'void'.
```

3. never

- 함수가 절대 return 하지 않을 때 발생한다.
  - ex) 함수에서 exception(예외)가 발생할 때
  - 즉, 오류가 발생하여 에러를 발생시킬 때 사용한다.
- 타입이 두가지 일 수도 있는 상황에서도 발생한다.
  `neve 타입 사용(함수가 절대 return 하지 않을 경우)`

```js
function hello(): never {
  // hello 함수는 never 타입이라 return 하지 않는다.
  return "X"; // string 타입을 return 하기에 에러 발생 Type 'string' is not assignable to type 'never'.
}
function Error(): never {
  // error 함수는 never 타입이라 return 하지 않는다.
  // 오류를 발생시키는 함수(throw new Error("발생시킬 오류 메시지"))
  throw new Error("xxx"); // return을 하지 않고 오류를 발생시키는 함수
}
```

`never 타입 사용(타입이 두 가지 일 수도 있는 상황)`

```js
function hello(name: string | number) {
  if (typeof name === "string") {
    // name의 타입이 string인지 확인 후 if문 진행
    name; // name의 타입은 string (parameter) name: string
  } else if (typeof name === "number") {
    // name의 타입이 string타입이 아니라면 name의 타입이 number인지 확인 후 else if문 진행
    name; // name의 타입은 number (parameter) name: number
  } else {
    // 이 부분의 코드는 절대 실행되지 않아야 한다.(string의 타입이 string과 number 둘 다 아니기 때문)
    // name의 타입이 string타입도 아니고 number타입도 아니라면 name의 타입은 TS가 never 타입으로 추론
    name; // name의 타입은 never (parameter) name: never
  }
}
```

# 3.0

`call signatures`

- 함수를 구현하기 전에 타입을 만들 수 있고, 함수가 어떻게 동작하는지 서술할 수 있다.
  - 즉, 타입을 먼저 생각하고 코드를 구현한다.

```js
type Add = (a: number, b: number) => number;
const add: Add = (a, b) => a + b;
const add1: Add = (a, b) => {
  a + b;
}; // return 타입이 void가 된다.
// return 타입이 void가 되기때문에 에러가 발생한다. Type '(a: number, b: number) => void' is not assignable to type 'Add'. Type 'void' is not assignable to type 'number'.
```

# 3.1

`overloading`

- 함수가 서로 다른 여러개의 call signatures를 가지고 있을 때 발생한다.

overloading의 예 1

```js
type Add = {
    // a는 항상 number타입이지만 b는 number일 수도 있고 string일 수도 있다.
    (a: number, b:number) : number
    (a: number, b: string) : number
}
const add : Add = (a,b) => {
    // a의 타입은 number로 고정되어있지만, b의 타입은 number 혹은 string이기에 if문을 사용하여 b의 타입을 확인해줘야한다.
    if(typeof b === "string"){ // b의 타입이 string인 경우 a만 return 한다.
        return a;
    }
    return a + b; // b의 타입이 number인 경우 a+b를 반환한다.
}

```

overloading의 예 2

```js
type Config = {
    path : string,
    state : object
}
type Push = {
    (path : string): void
    (config: Config) : void
}
const push: Push = (config) => { // config의 타입 string 아니면 Config인지 TS가 추론한다. (parameter) config: string | Config
    if(typeof config === "string"){ // config가 string일 경우
        console.log(config);
    }else{ // config가 Config타입일 경우
        console.log(config.path)
    }
}
```

`overloading시 다른 여러 개의 argument를 가지고 있을 때의 효과`

- 즉, 파라미터의 개수가 다를 때의 overloading

```js
type Add = {
    (a: number, b: number) : number
    (a: number, b: number, c: number) : number
}
const add: Add = (a,b,c?:number) => { // c를 number타입 or undefined타입으로 선언하여 c argument가 없을 경우 타입을 undefined로 설정한다.
    if(c === "number"){
        return a + b + c;
    }else{
        return a + b;
    }
}
add(1,2);
add(1,2,3);
```

# 3.2

다형성

- 변수들은 다양한 타입을 가질 수 있다.

`다형성 적용 전 코드`

```js
type SuperPrint = {
  (arr: number[]): void
  (arr: boolean[]): void
  (arr: string[]): void
}
const superPrint: SuperPrint = (arr) => {
  arr.forEach(i => console.log(i));
}
superPrint([1,2,3,4])
superPrint([true, false, true])
superPrint(["a","b"])
superPrint(["a","b",true]) // 2번째 index가 boolean이기에 에러 발생 No overload matches this call.Overload 3 of 3, '(arr: string[]): void', gave the following error.Type 'boolean' is not assignable to type 'string'.Overload 3 of 3, '(arr: string[]): void', gave the following error.Type 'number' is not assignable to type 'string'.
```

concrete type

- 기존 알고 있는 타입들
  - ex) number, string 등
    generic type
- TS가 타입을 유추한다.
  - 즉, `call signature`를 작성할 때 무슨 타입이 들어올지 모를 때 generic type을 사용한다.
  - `call signature`를 작성할때 들어올 타입을 알고 있다면 concrete type을 이용한다.

`generic type 사용법[다형성 적용 코드]`

1. TS에 generic type 사용을 선언한다.

- `<generic 타입 이름>`

```js
type SuperPrint = {
  // generic 타입의 이름을 TypePlaceholder로 설정
  <TypePlaceholder>(arr: TypePlaceholder[]): void,
};
const superPrint: SuperPrint = (arr) => {
  arr.forEach((i) => console.log(i));
};
superPrint([1, 2, 3, 4]);
superPrint([true, false, true]);
superPrint(["a", "b"]);
superPrint(["a", "b", true, 1]); // 다형성 적용 전 에러가 발생하던 코드가 generic 타입 적용 후 에러가 발생하지 않는다. -> TS가 타입을 유추하기 때문
// const superPrint: <string | boolean>(arr: (string | boolean)[]) => void
```

`generic type을 이용하여 함수의 리턴타입까지 변경하기[다형성 적용 코드]`

```js
type SuperPrint = {
  // generic 타입의 이름을 TypePlaceholder로 설정
  <TypePlaceholder>(arr: TypePlaceholder[]): TypePlaceholder,
};
const superPrint: SuperPrint = (arr) => arr[0];
const a = superPrint([1, 2, 3, 4]); // const superPrint: <number>(arr: number[]) => number
const b = superPrint([true, false, true]); // const superPrint: <number>(arr: number[]) => number
const c = superPrint(["a", "b"]); // const superPrint: <string>(arr: string[]) => string
const d = superPrint(["a", "b", true, 1]); // const superPrint: <string | number | boolean>(arr: (string | number | boolean)[]) => string | number | boolean
```

# 3.3

`generic type 사용법 2(2개의 argument)`

```js
type SuperPrint = <T, M>(a: T[], b: M) => T; // TS는  함수의 첫 번째 파라미터로 배열이 오는 것(T[])을 알고, 두 번째 파라미터로는 M이 온다고 알고있다.

const superPrint: SuperPrint = (arr) => arr[0];
const a = superPrint([1, 2, 3, 4]); // SuperPrint는 2개의 argument를 기대하지만 한개의 arguemnt만 사용하기에 에러 발생 Expected 2 arguments, but got 1.
const b = superPrint([1, 2, 3, 4], "true");
```

`TS는 generic을 처음 인식했을 때와 generic의 순서를 기반으로 generice의 타입을 알게된다.`

# 3.4

`generic type은 대문자로 시작해야 한다.`
`타입을 생성하고 그 타입을 또다른 타입에 넣어서 사용할 수 있다.`
`타입에 타입을 넣는 방법 1`

```js
type Player<E> = {
  name : string
  extraInfo: E
}
// extraInfo에 object가 들어간다.
const kamja : Player<{favFood: string}> = {
  name : "kamja",
  extraInfo : {
    favFood : "kokuma"
  }
}
```

`타입에 타입을 넣는 방법 2`

```js
type Player<E> = {
  name : string
  extraInfo: E
}
type kamjaPlayer = Player<{favFood : string}>
const kamja : kamjaPlayer{
  name : "kamja",
  extraInfo : {
    favFood : "kokuma"
  }
}
```

`타입에 타입을 넣는 방법 3`

```js
type Player<E> = {
  name : string
  extraInfo: E
}
type kamjaExtra = {
  favFood : string
}
type kamjaPlayer = Player<kamjaExtra>
const kamja : kamjaPlayer{
  name : "kamja",
  extraInfo : {
    favFood : "kokuma"
  }
}
```

`큰 타입을 하나 가지고 있는데 그 중 하나가 달라질 수 있는 타입이라면 그 타입을 generic 타입으로 선언하면 타입을 재사용할 수 있다.`
`generic type의 재활용`

```js
type Player<E> = {
  name : string
  extraInfo: E
}
const kamja : Player<{favFood : string}>{
  name : "kamja",
  extraInfo : {
    favFood : "kokuma"
  }
}
// extraInfo가 null인 kokuma 상수 생성
const kokuma : Player<null>{
  name : "kokuma",
  extraInfo : null
}
```

# 4.0

`TS를 이용한 객체지향 프로그래밍`
`TS에서 클래스 생성방법`

```js
class Player{ // Player class
  constructor( // 파라미터들만 쓰면 TS가 알아서 Constructor(생성자) 함수를 생성한다.
    private firstName : string,
    private lastName : string,
    public nickname : string
  ) {}
}
const kamja = new Player("kamja","44","캄자");
kamja.firstName; // Error발생 Property 'firstName' is private and only accessible within class 'Player'. Property 'lastName' is declared but its value is never read.
kamja.nickname; // "캄자"
```

`추상클래스`

- 추상 클래스는 오직 다른 클래스가 상속받을 수 있는 클래스이다.
  - 단, 추상 클래스는 직접 새로운 인스턴스를 만들 순 없다.
  - 즉, 추상 클래스는 오직 다른곳에서 상속받을수만 있는 클래스이다.

```js
abstract class User{
  constructor(
    private firstName : string,
    private lastName : string,
    public nickname : string
  ) {}
  getFullName(){
    return `${this.firstName}${this.lastName}`
  }
}
class Player extends User{

}
const kamja = new User("kamja","44","캄자"); // Error 발생 Cannot create an instance of an abstract class.
// 추상 클래스는 직접 새로운 인스턴스를 만들 수 없다.
const kamja = new Player("kamja","44","캄자");
// Player클래스가 User 추상 클래스를 상속받았으므로 kamja는 getFullName() 메서드를 사용할 수 있다.
kamja.getFullName(); // "kamja44"
```

`메소드`

- 클래스 안에 존재하는 함수이다.

`추상 메소드`

- 추상 메소드는 추상 클래스를 상속받는 모든 것들이 구현을 해야하는 메소드를 의미한다.
  - 즉, 추상 클래스를 상속하는 클래스에서 추상 메서드를 구현한다.
- 추상 클래스 안에서는 추상 메소드를 만들 수 있다.
  - 단, 메소드를 직접 구현해서는 안되며 메소드의 call signature만 적어둬야 한다.
  - 즉, 추상 메소드를 만들려면 메소드를 클래스 안에서 구현하지 않으면 된다.
  - 추상 메소드는 arguement를 가질 수 있다.

```js
abstract class User{
  constructor(
    private firstName : string, // private는 외부에서 접근 불가, 자식 클래스(상속받은 클래스)도 접근 불가
    private lastName : string, // private는 외부에서 접근 불가, 자식 클래스(상속받은 클래스)도 접근 불가
    private nickname : string, // private는 외부에서 접근 불가, 자식 클래스(상속받은 클래스)도 접근 불가
    protected protectedNickname : string // private는 외부에서 접근 불가, 자식 클래스(상속받은 클래스)도 접근 가능
  ) {}
  abstract getNickName() : void // 메소드의 call signature만 가지고 있다. 즉, getNickName은 추상 메소드이다.
  getFullName(){
    return `${this.firstName}${this.lastName}` // 메소드의 구현 부분
  }
}
class Player extends User{
  // Player class는 User 추상 클래스를 상속받는다.
  // User 추상 클래스에는 추상 메서드인 getNickName()이 있다.
  // 즉, Player 클래스는 getNickName을 구현해야한다.
  getNickName(){
    console.log(this.protectedNickname); // protectedNickName이 protected이기에 상속받은 클래스에서 접근이 가능하다.
  }
}
const kamja = new Player("kamja","44","캄자", "protected캄자");
kamja.protectedNickname // protectednickname이 private이기에 Error발생 Property 'protectedNickname' is protected and only accessible within class 'User' and its subclasses.
kamja.getNickName(); // protected 타입은 상속받은 객체(자식 클래스)에서는 접근이 가능하다.
```

필드가 외부로부터는 보호되지만 자식 클래스에서는 사용하고 싶다면 `private`를 사용하면 안된다.
`private` 대신 `protected`를 사용하면 필드가 외부로부터 보호되고, 자식 클래스에서 필드의 사용이 가능하다.

### public, protected, private 구분

|           | 선언한 클래스 내에서 접근 | 상속받은 클래스(자식 클래스) 내에서 접근 | 인스턴스 |
| :-------: | :-----------------------: | :--------------------------------------: | :------: |
|  public   |            ⭕             |                    ⭕                    |    ⭕    |
| protected |            ⭕             |                    ⭕                    |    ❌    |
|  private  |            ⭕             |                    ❌                    |    ❌    |

`public`

- 모든 클래스에서 접근 가능

`protected`

- 해당 클래스와 자식 클래스(상속받은 클래스)에서 접근 가능

`private`

- 해당 클래스 내에서만 접근 가능(자식 클래스(상속받은 클래스)에서도 접근 불가)

# 4.1

`hashMap 만들기`

- 해싱 알고리즘 이용
- 사전에 새 단어를 추가하고, 단어를 찾고, 단어를 삭제하는 메소드 만들기

```js
type Words = {
  [key : string] : string // Words 타입이 string만을 property로 가지는 오브젝트라고 명시한다.
  // 즉, 객체의 키는 string 타입, 값도 string 타입이여야한다.
  // [key : string] : string에서 key의 이름은 변경되어도 상관없다 [x : string] : string이라고 해도 위의 코드와 동일하게 동작한다.
  // 제한된 양의 property 혹은 key를 가지는 타입을 정의하는 방법이다.
  // 즉, object의 타입을 선언해야할 때 유용하다.
}
let EX : Words = {
  "kamja" : "kokuma", // string : string
  2 : "kokuma",  // number : string  Error 발생 키는 string 타입이여야 한다.
}
class Dict {
  private words : Words // property가 constructor(생성자)부터 바로 초기화되지 않는다.
  constructor(){ // words를 수동으로 초기화
    this.words = {}
  }
  add(word : Word){ // 클래스를 타입처럼 사용할 수 있다.
    if(this.words[word.term] === undefined){
      // 주어진 단어가 아직 사전에 존재하지 않음을 의미한다.
      this.words[word.term] = word.def;
    }
  }
  def(term : string){
    // 단어 출력
    return this.words[term];
  }
  update(word : Word){
    if(this.words[word.term] === undefined){
      this.add(word);
    }else{
      this.words[word.term] = word.def;
    }
  }
  del(term : string){
    if(this.words[term] !== undefined){
      delete this.words[term];
    }
  }
}
class Word {
  constructor(
    public term : string,
    public def : string
  ){}
}
const kamja = new Word("kamja","캄자");
const dict = new Dict();
dict.add(kamja);
dict.def("kamja");
dict.update(new Word("kamja","kamja44"));
console.log(`캄자 -> ${dict.def("kamja")}`);
dict.del("kamja");
console.log(`kamja is gone ${dict.def("kamja")}`);
```

# 4.2

`readonly`

- 타입을 지정할때 readonly를 사용하면 읽기전용이 된다.
  - 즉, 내용을 수정할 수 없다.
  - TS의 readonly는 JS에 존재하지 않는다.

```js
class Word {
  constructor(
    public readonly term : string,
    public readonly def : string
  ){}
}
const kamja = new Word("kamja","캄자");
kamja.term = "kokuma"; // term, def는 readonly이기에 수정이 불가능하다. Cannot assign to 'term' because it is a read-only property.
```

`concrete 타입의 특정 값 명시하기`

```js
type Team = "red" | "blue" | "yellow";
type Health = 1 | 5 | 10
type Player = {
  nickname : string,
  team : Team // team은 red, blue, yellow만 될 수 있다.
  health : Health // health는 1, 5, 10만 될 수 있다.
}
const kamja : Player = {
  nickname : "kamja",
  // team : "coral", // Error발생 team은 red, blue, yellow 3개 중 하나의 값만 가질 수 있다. Type '"coral"' is not assignable to type 'Team'.
  team : "red",
  // health : 9 // ERROR발생 health는 1, 5, 10 3개 중 하나의 값만 가질 수 있다. Type '9' is not assignable to type 'Health'.
  health : 10,
}
```

`interface`

- type과 동일한 역할을 한다.
  - 단, 오브젝트의 모양을 특정하기 위한 역할만 한다.
    - 즉, 인터페이스를 TS에게 오브젝트의 모양을 설명할 때만 사용할 수 있다.
- interface는 객체 지향 프로그ㅐ밍의 개념을 활용하여 개발되었다.

```js
interface Player {
  nickname: string;
  team: string;
  health: number;
}
type Player = {
  nickname: string,
  team: string,
  health: number,
};
// interface와 Player 코드는 동일한 역할을 수행한다.
```

`interface의 상속`

- interface는 클래스처럼 동작한다.

```js
interface User {
  name : string
  readonly read : string
}
interface Player extends User{
}
const kamja : Player = {
  name : "kamja"
}
```

`interface의 property 축적`

```js
interface User {
  name: string;
}
interface User {
  age: number;
}
interface User {
  location: string;
}
// TS가 동일한 인터페이스를 하나로 합쳐준다.
// 즉, 같은 인터페이스에 다른 이름을 가진 property 들을 쌓을 수 있다.
const kamja: User = {
  name: "kamja",
  age: 20,
  location: "KOR",
};
```

`type은 interface보다 유연하다 -> 사용할 수 있는 기능이 많다.`

# 4.3

`추상 클래스를 인터페이스로 변경`

`추상 클래스`

```js
abstract class User {
  constructor(
    protected firstname : string,
    protected lastName : string
  ){}
  abstract sayHi(name : string) : string
  abstract fullName() : string
}
class Player extends User {
  fullName(){
    return `${this.firstName} ${this.lastName}`
  }
  sayHi(name : string){
    return `Hello ${name}. My name is ${this.fullName()}`
  }
}
```

`인터페이스 상속`

- 인터페이스를 상속할 때에는 property를 private으로 만들지 못한다.(protected로도 만들지 못한다.)
  - 즉, 인터페이스를 상속할때 property는 public으로 생성해야 한다.
- 인터페이스 상속 시 implements 키워드를 사용하는데 이는 TS에서만 지원한다.
  - 인터페이스를 상속하면 추상 클래스를 추가로 사용하지 않기에 TS에서 JS로 변환 시 파일의 사이즈를 줄일 수 있다.

```js
interface User{
  firstName : string,
  lastName : string,
  sayHi(name : string) : string,
  fullName() : string,
}
class Player implements User{ // implements는 TS에만 존재하고 JS에는 존재하지 않는다.
  constructor(
    public firstName : string,
    public lastName : string,
  ){}
  fullName(){
    return `${this.firstName} ${this.lastName}`
  }
  sayHi(name : string){
    return `Hello ${name}. My name is ${this.fullName()}`
  }
}
```

`여러개의 인터페이스 동시에 상속`

- 하나의 클래스에서 여러개의 인터페이스를 동시에 상속이 가능하다.

```js
interface User{
  firstName : string,
  lastName : string,
  sayHi(name : string) : string,
  fullName() : string,
}
interface Human {
  health : number
}
class Player implements User, Human{ // 인터페이스 여러개 동시 상속
  constructor(
    public firstName : string,
    public lastName : string,
    public health : number,
  ){}
  fullName(){
    return `${this.firstName} ${this.lastName}`
  }
  sayHi(name : string){
    return `Hello ${name}. My name is ${this.fullName()}`
  }
}
```

# 4.4

Recap
`인터페이스는 개발자가 원하는 메소드와 property를 클래스가 가지도록 강제할 수 있다.`

<br>

`타입 키워드를 사용하여 타입 상속받기`

```js
type PlayerA = {
  name: string,
};
type PlayerAA = PlayerA & {
  lastName: string,
};
const playerA: PlayerA = {
  name: "kamja",
  lastName: "kokuma",
};
```

`interface를 사용하여 interface 상속받기`

```js
interface PlayerB {
  name: string;
}
interface PlayerBB extends PlayerB {
  lastName: string;
}
interface PlayerBB {
  health: number;
}
const playerB: PlayerB = {
  name: "kamja",
};
```

`타입과 인터페이스는 모두 동일하게 동작한다.`

```js
type PlayerA = {
  firstName : string
}
interface PlayerB{
  firstName : string
}
class User implements PlayerB{
  constructor(
    public firstName : string
  )
}
class User1 implements PlayerA{
  constructor(
    public firstName : string
  )
}
```

type과 interface의 차이점

`클래스나 오브젝트의 모양을 정의하고 싶으면 interface를 사용하고 나머지의 경우에는 type을 사용한다.`

<br>

`타입은 새 property를 추가하기 위해 다시 선언될 수 없지만 interface는 항상 다시 선언될 수 있다.`

# 4.5

`다형성, 제네릭, 클래스, 인터페이스 모두 합치기`
다형성

- 다른 모양의 코드를 가질 수 있게 한다.
  - 다형성을 이루기 위해서 제네릭을 사용한다.

제네릭

- concrete 타입이 아닌 placeholder 타입 즉, TS가 타입을 추론한다.
- 제네릭은 다른 타입에게 상속이 가능하다.

```js
// Storage interface
// TS에 의해 이미 선언된 JS의 웹 스토리지 API를 위한 인터페이스이다.
interface Storage{ // 즉, Storage에 새 property를 추가한다.(기존 Storage인터페이스를 상속받는다.)
}
interface SStorage<T>{
  [key : string] : T // key가 제한되지 않은 Object를 정의하게 해준다.
}
class LocalStorage<T> { // generic type인 T를 클래스로 보내고 클래스는 제네릭을 인터페이스로 보낸다.
  private storage : SStorage<T> = {}
  set(key : string, value : T){ // 스토로지 저장
    this.storage[key] = value;
  }
  remove(key : string){ // 스토로지 제거
    delete this.storage[key]
  }
  get(key : string) : T { // 스토로지 가져오기
    return this.storage[key]
  }
  clear(){ // 스토로지 비우기
    this.storage = {}
  }
}

const stringStorage = new LocalStorage<string>() // constructor LocalStorage<string>(): LocalStorage<string>
stringStorage.get("ket") // const stringStorage: LocalStorage<string>
stringStorage.set("hello", "how are you?")
const booleansStorage = new LocalStorage<boolean>() // constructor LocalStorage<boolean>(): LocalStorage<boolean>
booleansStorage.get("xxx") // const booleansStorage: LocalStorage<boolean>
booleansStorage.set("hello", true)
```
