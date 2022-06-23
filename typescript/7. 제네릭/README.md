# 7. 제네릭

### 1. 제네릭

```tsx
const identity = (arg: number): number => {
	return arg;
}
```

- 제네릭이 없다면 `identity` 함수는 지정한 타입으로만 사용해야 한다.
    - 여기선 `number`로만 사용 가능
- 제네릭 없이 함수의 파라미터 타입을 지정하고 싶지 않고 동적으로 변동되는 값을 받고 싶다면 `any`를 사용할 수도 있다.
    - 그러나 실제로 함수가 값을 반환할 때 `any` 타입이 된다는 정보만 얻을 뿐 타입스크립트를 사용하는 이점이 없게된다.
- 제네릭을 사용하면 유저가 준 파라미터의 타입을 캡처하고 이 정보를 나중에 사용할 수 있게 해준다.

```tsx
const identity = <Type>(arg: Type): Type => {
	return arg;
}
```

- 여기선 `arg`의 타입을 `any`를 사용한 것과 유사하게 사용할 수 있게 되지만 반환 타입에 `number` 등 파라미터로 전달받은 타입을 캡처하여 정보를 나타내준다.
- 제네릭은 아래와 같이 동작한다.

```tsx
// 1. 함수를 호출할 때 제네릭 타입을 전달한다.
identity<number>();

// 2. 함수의 인자를 전달한다.
identity<number>(1);

// 3. 해당 함수 호출은 아래 함수를 호출한 것과 같다.
function identity<number>(arg: number): number {
	return arg;
}
```

### 2. 제네릭 사용 이유

- 제네릭을 사용하면 매개변수들은 실제로 any와 같이 모든 타입이 될 수 있는 것처럼 취급할 수 있게 해준다.
    - 모든 경우에 유효하지는 않다.
    - 특정 타입에 대한 메서드를 사용하려 할때 아래와 같이 에러가 발생하지만 any는 에러가 발생하지 않는다.
    
    ```tsx
    const identity = <Type>(arg: Type): Type => {
      console.log(arg.length);
    	// 'Type' 형식에 'length' 속성이 없습니다.ts(2339)
    	return arg;
    };
    
    // 아래와 같이 선언하면 arg는 항상 배열형태이고, length를 사용할 수 있게된다.
    const identity = <Type>(arg: Type[]): Type[] => {
      console.log(arg.length);
    	return arg;
    }
    ```
    
- any를 사용하면 더 이상 타입 검사를 하지 않기에 any 타입에 대한 정보 외엔 아무것도 얻을 수 없다.
- 그래서, 함수의 인자와 반환 값에 대한 타입 정보를 얻으면서 any와 유사하게 모든 타입에 대한 타입을 사용할 수 있도록 설정하기 위해 제네릭을 사용한다.

### 3. 제네릭 타입

- 제네릭 타입을 객체 리터럴 타입의 함수 호출 시그니처로 작성할 수 있다.

```tsx
function identity<Type>(arg: Type): Type {
  return arg;
}
 
let myIdentity: { <Type>(arg: Type): Type } = identity;
```

- 객체 리터럴 타입 대신 인터페이스로 작성할 수도 있다.

```tsx
interface GenericIdentityFn {
  <Type>(arg: Type): Type;
}
 
function identity<Type>(arg: Type): Type {
  return arg;
}
 
let myIdentity: GenericIdentityFn = identity;
```

- 여기서 매개변수의 타입을 명시할 수도 있다.

```tsx
let myIdentity: GenericIdentityFn<number> = identity;
```

### 4. 제네릭 클래스

- 클래스에서도 제네릭을 사용할 수 있고, 인터페이스를 작성하는 것과 유사하다.

```tsx
class GenericMath<T> {
  pi: T;
  sum: (x: T, y: T) => T;
}

let math = new GenericMath<number>();
```

### 5. 제네릭 제약 조건

- 제네릭으로 모든 타입에 대해 사용할 수 있지만, `length` 메서드와 같이 특정 타입에 대해서만 사용할 수 있는 메서드를 사용할 때 에러가 발생한다.
- `length` 메서드를 `any`처럼 사용할 수 있도록 하려면 제네릭 제약 조건을 명시하는 인터페이스를 작성하여 해결할 수 있다.

```tsx
interface Lengthwise {
  length: number;
}
 
function loggingIdentity<Type extends Lengthwise>(arg: Type): Type {
  console.log(arg.length);
	// Type 타입은 Lengthwise를 상속받아 length 메서드를 갖도록 돼있기에 에러가 발생하지 않는다.
	// 다만 모든 타입에 대해 동작하지는 않는다.
  return arg;
}

loggingIdentity(10); // Error, 숫자 타입에는 `length`가 존재하지 않으므로 오류 발생
loggingIdentity({ length: 0, value: 'hi' }); // `text.length` 코드는 객체의 속성 접근과 같이 동작하므로 오류 없음
```

### 6. 객체의 속성을 제약하는 방법

```tsx
function getProperty<Type, Key extends keyof Type>(obj: Type, key: Key) {
  return obj[key];
}
 
let x = { a: 1, b: 2, c: 3, d: 4 };
 
getProperty(x, "a");
getProperty(x, "m"); // '"m"' 형식의 인수는 '"a" | "b" | "c" | "d"' 형식의 매개 변수에 할당될 수 없습니다.ts(2345)
```

- `key extends keyof Type` 의 의미는 `Type`의 `key` 를 유니온 타입으로 갖는 것처럼 동작한다.
    - 여기선 `type Key = "a" | "b" | "c" | "d"` 처럼 선언한 것과 동일.
    - `obj[key]` 처럼 작성해도 keyof Type의 타입을 상속받으므로 에러가 발생하지 않는다.