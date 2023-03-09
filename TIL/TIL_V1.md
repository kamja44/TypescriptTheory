# TS 기본 타입

- 타입은 항상 변수명 뒤에 콜론을 작성한 후 작성한다.

배열 : 자료형[]
숫자 : number
문자열: string
논리: boolean

Object의 타입 정의 1

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

Object의 타입 정의 2
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

함수의 return 타입 지정

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

Arrow Function 사용 시 타입 지정 방법

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
