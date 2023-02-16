```ts
function logPerson(person: Person) {
  let additionalInformation: string = "";
  if (isAdmin(person)) {
    additionalInformation = person.role;
  }
  if (isUser(person)) {
    additionalInformation = person.occupation;
  }
  console.log(` - ${person.name}, ${person.age}, ${additionalInformation}`);
}
```

에러가 발생하는 구간이다. `.role`, `.occupation`에서 빨간 줄이 그어지는 데 해당 키를 찾을 수 없다고 한다. 앞 단계와 역시 조건으로 분류하고 있지만, 타입스크립트에서는 `role`을 `User`에서 찾고, `occupation`을 `Admin`에서 찾고 있다.

> 이걸 확실하게 알려주기 위해서는 `isAdmin`, `isUser` 함수를 수정해주면 된다.

### 변경 전

```ts
export function isAdmin(person: Person) {
  return person.type === "admin";
}

export function isUser(person: Person) {
  return person.type === "user";
}
```

### 변경 후

```ts
export function isAdmin(person: Person): person is Admin {
  return person.type === "admin";
}

export function isUser(person: Person): person is User {
  return person.type === "user";
}
```

각 함수에 인자로 들어오는 값을 어떤 타입에서 찾아야 하는 지 알려주면 에러는 사라진다.

> 그런데 이럴 거면 함수 인자의 타입을 각각 Admin, User로 바꾸면 되지 않을까?...하는 생각이 있다.
