```ts
function logPerson(person: Person) {
  let additionalInformation: string;
  if (person.role) {
    additionalInformation = person.role;
  } else {
    additionalInformation = person.occupation;
  }
  console.log(` - ${person.name}, ${person.age}, ${additionalInformation}`);
}
```

에러가 발생하는 구간이다. if문의 `.role`에서 빨간 줄이 그어진다.
에러가 생기는 이유는 들어오는 인자로 들어오는 `person`에 `.role`이 `User`에 없기 때문이다. 인자의 타입은 `User | Admin`인데 `role`은 `Admin`에 있다.

따라서 조건을 바꿔주면 에러가 사라진다. 들어오는 타입에 `role`이 있는지 확인하면 된다.

```ts
if ("role" in person) {
  additionalInformation = person.role;
} else {
  additionalInformation = person.occupation;
}
```
