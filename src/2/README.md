```ts

type Person = ...;

```

타입 `Person`을 선언하여 해결하는 문제이다.

```ts
const persons: Person[] /* <- Person[] */ = [
  {
    name: "Max Mustermann",
    age: 25,
    occupation: "Chimney sweep",
  },
  {
    name: "Jane Doe",
    age: 32,
    role: "Administrator",
  },
  {
    name: "Kate Müller",
    age: 23,
    occupation: "Astronaut",
  },
  {
    name: "Bruce Willis",
    age: 64,
    role: "World saver",
  },
];
```

배열 값을 확인하면 키값이 두 종류가 들어있다. `role`, `occupation` 두 종류인데 이때문에 에러가 발생한다.

```ts
interface User {
  name: string;
  age: number;
  occupation: string;
}

interface Admin {
  name: string;
  age: number;
  role: string;
}
```

`interface User` 에는 `role`이 없기 때문이다. 해결해줄 수 있는 방법은 두 가지인데, 여기서 `interface Admin`이 이미 선언되어 있기 때문에 이를 이용해 해결해주겠다.

```ts
type Person = User | Admin;
```

`Person`에 `User`와 `Admin`을 추가해주면 된다. 즉, `User`에 값이 없으면 `Admin`에서 찾으라고 하는 소리와 같다.

<hr>

### 다른 방법

```ts
interface Person {
  name: string;
  age: number;
  occupation?: string;
  role?: string;
}
```

`?` 를 사용해 `option`으로 지정해주면 하나의 인터페이스로도 사용할 수 있다.
