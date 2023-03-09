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