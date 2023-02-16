unknown 타입 타입스크립트에게 추론을 부탁하는 것이다.

```ts
type User = unknown;
const users: unknown[] = [...]
function logPerson(user: unknown) {...}
```

index.js 파일을 보면, 타입 `User`,`users` 배열, `logPerson` 함수에 `unknown` 타입이다.
에러가 나는 곳은 함수의 인자 부분인데, 어떤 인자가 들어올 것인지 타입스크립트가 추론할 수 없어서 생긴다. `unknown`을 지우고 정확한 타입 값을 작성해주면 에러 표시는 사라진다.

```ts
type User = { name: string; age: number; occupation: string };
const users: User[] = [...]
function logPerson(user: User) {...}
```

`users` 배열 내부의 키와 값을 확인하고 해당 타입을 작성하여 타입을 선언하면 해결된다.
