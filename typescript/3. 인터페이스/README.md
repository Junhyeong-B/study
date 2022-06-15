# 3. 인터페이스

### 1. 인터페이스 정의

- 객체의 스펙
- 함수의 파라미터, 스펙
- 배열과 객체를 접근하는 방식
- 클래스

### 2. 인터페이스를 통한 추론

- 인터페이스를 함수의 파라미터로 타입 정의하여 사용하면 항상 인터페이스의 속성 개수와 인자로 받는 객체의 속성 개수를 일치시키지 않아도 된다.
- 인터페이스에 정의된 속성 개수만 일치한다면 실제 객체의 속성 개수는 더 많아도 상관 없다.
- 인터페이스에 정의된 속성의 순서는 상관 없다.

```tsx
interface Name {
  firstName: string;
}

function hello(name: Name) {
  console.log(name.firstName);
}

const person = {
	firstName: "First",
	lastName: "Last"
};

hello(person); // First
```

- 여기서 객체의 속성은 firstName, lastName 2개 이지만 interface 속성의 개수는 firstName 1개이다. 이 경우 에러가 발생하지 않고 사용 가능하다.

### 3. 옵션 속성

- 인터페이스의 속성을 사용할 수도 있고 사용하지 않을 때도 있을 땐 옵션 속성으로 지정할 수 있다.

```tsx
interface Name {
	firstName: string;
	fullName?: string;
}
```

- Name 인터페이스에서는 firstName은 필수로 필요하지만, fullName은 있어도 되고 없어도 된다.
- 여기서 fullName은 `fullName: string | undefined;` 라고 작성하는 것과 동일하다.

### 4. 옵션 속성의 장점

- 옵션 속성을 선택적으로 사용할 수 있게 해놓으면 인터페이스에 정의되어 있지 않은 속성에 대해 Error를 발생시키므로 코드를 작성하는 사람이 이를 인지할 수 있게 해준다.

### 5. 읽기 전용 속성

- 인터페이스 객체를 생성할 때만 값을 할당하고 그 이후에는 변경할 수 없도록 하는 키워드로 속성 앞에 readonly를 붙여서 사용한다.

```tsx
interface Human {
	readonly gender: string;
}

const me: Human = {
	gender: "male"
};
me.gender = "female"; // Cannot assign to 'gender' because it is a read-only property.
```

### 6. 읽기 전용 배열

- 배열을 선언할 때 읽기전용으로 만들고 싶으면 `ReadonlyArray<T>` 형태로 선언한다.

```tsx
const Arr: ReadonlyArray<number> = [1, 2, 3];
arr.push(4); // Property 'push' does not exist on type 'readonly number[]'.
```

### 7. 함수 타입

```tsx
interface SumFunc {
	(num1: number, num2: number): number;
}

const sum: SumFunc = (num1: number, num2: number) => {
	return num1 + num2;
}
```

### 8. 클래스 타입

```tsx
interface CraftBeer {
  beerName: string;
  nameBeer(beer: string): void;
}

class myBeer implements CraftBeer {
  beerName: string = 'Baby Guinness';
  nameBeer(b: string) {
    this.beerName = b;
  }
  constructor() {}
}
```

### 9. 인터페이스 확장

```tsx
interface Name {
	firstName: string;
	lastName: string;
}

interface Human extends Name {
	gender: string;
}

const me: Human = {
	firstName: "first",
	lastName: "last",
	gender: "male",
};
```