```ts
...

function filterPersons(persons: Person[], personType: string, criteria: unknown): unknown[] {
    return persons
        .filter((person) => person.type === personType)
        .filter((person) => {
            let criteriaKeys = Object.keys(criteria) as (keyof Person)[];
            return criteriaKeys.every((fieldName) => {
                return person[fieldName] === criteria[fieldName];
            });
        });
}

...

console.log('Users of age 23:');
usersOfAge23.forEach(logPerson);


console.log('Admins of age 23:');
adminsOfAge23.forEach(logPerson);

```

에러가 발생하는 부분이다.

```ts
let criteriaKeys = Object.keys(criteria) as (keyof Person)[];
return person[fieldName] === criteria[fieldName];
```

`criteria`에 에러가 발생한다. 또한 `logPerson`에서도 에러가 발생하는데 이유는 앞 단계와 같다.

```ts
export const usersOfAge23 = filterPersons(persons, "user", { age: 23 });
export const adminsOfAge23 = filterPersons(persons, "admin", { age: 23 });
```

들어가는 값이 일부분이기 때문이다. 앞과 동일하게 선택사항으로 타입스크립트에게 알려줄 필요가 있다.

```ts
function filterPersons(persons: Person[], personType: string, criteria: unknown): unknown[] {...}
```

해당 부분은 `unknown`으로 처리되어 있어 에러가 발생한다. 또한 이번에도 역시 `type`값을 삭제해달라 요구되어 있다. 이를 고쳐보자.

### 변경 전

```ts
function filterPersons(
  persons: Person[],
  personType: string,
  criteria: unknown
): unknown[] {
  return persons
    .filter((person) => person.type === personType)
    .filter((person) => {
      let criteriaKeys = Object.keys(criteria) as (keyof Person)[];
      return criteriaKeys.every((fieldName) => {
        return person[fieldName] === criteria[fieldName];
      });
    });
}
```

### 변경 후

```ts
function filterPersons(
  persons: Person[],
  personType: string,
  criteria: Partial<Person>
): Person[] {
  return persons
    .filter((person) => person.type === personType)
    .filter((person) => {
      let criteriaKeys = Object.keys(criteria) as (keyof Person)[];
      return criteriaKeys.every((fieldName) => {
        return person[fieldName] === criteria[fieldName];
      });
    });
}
```

`criteria` 로 들어오는 인자를 선택사항으로 만들어준다. 또한 함수가 반환하는 값은 `Person[]`이라고 지정해준다. 이렇게만 해줘도 오류는 사라지지만 `typescript-exercises` 사이트에서는 에러를 뱉어낸다. 테스트에 실패했기 때문이다. `User`와 `Admin`의 타입을 지정해줘야 한다. 또 해당 타입의 `type`값을 삭제해주자.

```ts
function filterPersons(
  persons: Person[],
  personType: "user",
  criteria: Partial<Omit<User, "type">>
): User[];
function filterPersons(
  persons: Person[],
  personType: "admin",
  criteria: Partial<Omit<Admin, "type">>
): Admin[];
function filterPersons(
  persons: Person[],
  personType: string,
  criteria: Partial<Person>
): Person[] {
  return persons
    .filter((person) => person.type === personType)
    .filter((person) => {
      let criteriaKeys = Object.keys(criteria) as (keyof Person)[];
      return criteriaKeys.every((fieldName) => {
        return person[fieldName] === criteria[fieldName];
      });
    });
}
```

위와 같이 바꿔주면 테스트에 통과할 수 있다. 함수를 두 개 이상 선언하게 되면 다양한 방식으로 호출할 수 있다.

> 보너스 문제

1번

```ts
let criteriaKeys = Object.keys(criteria) as (keyof User)[];
```

2번

```ts
let criteriaKeys = getObjectKeys(criteria);
```

1번 코드를 2번 코드로 교체하여 작성하는 것이다. 즉, 별도의 함수로 분리하여 관리하는 것인데, 여기서 제네릭을 사용할 수 있다.

```ts
const getObjectKeys = <T>(obj:T) => Object.keys(obj) as (keyof T)[];

...
let criteriaKeys = getObjectKeys(criteria);
```

위처럼 사용할 수 있다. 들어오는 인자가 다르기 때문에 제네릭을 사용하여 타입을 결정할 수 있다.
