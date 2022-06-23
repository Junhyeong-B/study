# 8. 타입 추론

### 1. 타입 추론

- 타입 추론이란 타입스크립트가 명시적인 타입 표기가 없을 때 변수나 함수 반환 값에 대한 타입 정보를 제공해주는 것을 말한다.

### 2. 가장 적절한 타입

- 여러 표현 식에서 타입을 추론할 때 표현식들의 타입을 이용해 가장 적절한 타입을 계산한다.

```tsx
let x = [0, 1, null];
   // let x: (number | null)[]
```

- 이 경우 0, 1이 일반적인 number 타입이므로 `number | null` 로 타입이 추론된다.
- 가장 적절한 타입으로 계산할 때 항상 공통 타입을 선택하려고 하지만 모든 타입이 공통되지 않을 때는 각각의 타입이 추론되어 계산된다.
    - 이 경우 모든 추론되어야 할 변수, 값에 대해 타입을 명시하면 원하는 대로 타입을 추론하게 만들 수 있다.

```tsx
// zoo의 타입을 명시하지 않을 때
let zoo = [new Rhino(), new Elephant(), new Snake()];
    // let zoo: (Rhino | Elephant | Snake)[]

// zoo의 타입을 명시했을 때
let zoo: Animal[] = [new Rhino(), new Elephant(), new Snake()];
    // let zoo: Animal[]
```

### 3. 문맥상 타이핑

```tsx
window.onmousedown = function (mouseEvent) {
  console.log(mouseEvent.button); //<- OK
  console.log(mouseEvent.kangaroo); //<- Error!
};
```

- 위 경우 `onmousedown` 함수의 타입을 사용해 함수를 할당하기에 `mouseEvent` 매개변수는 `mouse Event`에 존재하는 매개변수만 추론되어 사용할 수 있게 된다.
- 따라서, `mouse event`에 존재하는 `button`은 사용할 수 있지만 존재하지 않는 `kangaroo`는 에러를 생성한다.