```ts
filterUsers(persons, {
  age: 23,
}).forEach(logPerson);
```

에러가 발생하는 지점이다. `age`에서 에러가 발생하는데, 인터페이스 `User`에서 설명한 객체의 모양과 다르기 때문이다.

```ts
interface User {
  type: "user";
  name: string;
  age: number;
  occupation: string;
}
```

`age`만 존재하기 때문에 에러가 발생한다. 그렇다고 우리는 나머지 값을 넣고 싶지 않다. 이를 위해 선택사항으로 타입스크립트에게 전달해주어야 한다.

### 변경 전

```ts
function filterUsers(persons: Person[], criteria: User): User[] {
  return persons.filter(isUser).filter((user) => {
    const criteriaKeys = Object.keys(criteria) as (keyof User)[];
    return criteriaKeys.every((fieldName) => {
      return user[fieldName] === criteria[fieldName];
    });
  });
}
```

### 변경 후

```ts
function filterUsers(persons: Person[], criteria: Partial<User>): User[] {
  return persons.filter(isUser).filter((user) => {
    const criteriaKeys = Object.keys(criteria) as (keyof User)[];
    return criteriaKeys.every((fieldName) => {
      return user[fieldName] === criteria[fieldName];
    });
  });
}
```

`criteria`의 타입을 `Partial<User>`로 설정하면 에러는 사라진다. `Partial`은 `User`의 모든 값을 선택사항으로 만들어준다.

> 보너스 문제

보너스 문제에서 `criteria` 에서 `type` 값을 삭제하라고 되어있다. 인터페이스는 건드리지 말고 삭제하는 방법이 있는데, 다음과 같이 해결하면 된다.

```ts
const criteriaKeys = Object.keys(criteria) as (keyof User)[];
```

들어오는 인자의 키를 찾는 부분이다. 해당 부분의 타입을 변경해주면 된다.

```ts
const criteriaKeys = Object.keys(criteria) as (keyof Omit<User, "type">)[];
```

위와 같이 작성해주면 `User`의 `type`값은 삭제된다. `Omit`은 해당 타입(`User`)의 해당 값(`type`)을 삭제해준다. 단, 키는 문자열이므로 문자열로 작성해주자.
