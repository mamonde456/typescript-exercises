### 에러 발생 구간

```ts
export type ApiResponse<T> = unknown;
```

다구간에서 에러가 발생하므로 생략한다. 이번 문제는 앞단계와 같이 새로운 타입을 선언하고 기존 타입을 삭제한 뒤, 해당 타입을 사용하여 해결하는 것이다.

`AdminsApiResponse`, `UsersApiResponse` 두 개의 타입을 삭제하고 `ApiResponse`를 작성해 교체해주면 된다.

들어오는 데이터 타입이 그때 그때 다르기 때문에 제네릭을 사용해준다.

### 변경 전

```ts
export type ApiResponse<T> = unknown;
```

### 변경 후

```ts
export type ApiResponse<T> = {
      status: "success";
      data: T;
    }
  | {
      status: "error";
      error: string;
    };
```

제네릭을 통해 들어오는 타입을 지정해주면 된다. 


```ts

export function requestUsers(callback: (response: ApiResponse<User[]>) => void) {
    callback({
        status: 'success',
        data: users
    });
}

export function requestCurrentServerTime(callback: (response: ApiResponse<number>) => void) {
    callback({
        status: 'success',
        data: Date.now()
    });
}

export function requestCoffeeMachineQueueLength(callback: (response: ApiResponse<number>) => void) {
    callback({
        status: 'error',
        error: 'Numeric value has exceeded Number.MAX_SAFE_INTEGER.'
    });
}

```

원래 타입은 배열이었으나 배열을 사용하지 않는 함수도 있기 때문에 `ApiResponse` 타입을 사용할 때, `Admin[]`, `User[]` 과 같이 작성해준다.

data 타입을 보고 `<number>`로 작성해주면 해당 문제는 해결되는 것을 볼 수 있다.