### 에러 발생 지역

```ts
type PowerUser = unknown;
```

다구간에서 에러를 발생하고 있으므로 생략한다. 이번 문제에서는 타입이 하나 추가되었기 때문에 기존 타입을 이용하여 해당 타입을 확장하는 문제이다.

### 변경 전

```ts
type PowerUser = unknown;
```

### 변경 후

```ts
type PowerUser = Omit<User, "type"> &
  Omit<Admin, "type"> & { type: "powerUser" };
```

새로 추가된 타입은 `PowerUser`이다. 해당 타입은 `User`, `Admin`의 요소를 모두 갖지만 `type`의 값이 다르다. 따라서 `Omit`을 이용해 `type`을 제거한 뒤 `powerUser`를 추가해준다.

여기서 `type`의 확장 문법인 `&`을 사용한다.
