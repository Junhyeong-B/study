# 10. 타입 별칭

### 1. 타입 별칭

- 타입 별칭이란 특정 타입이나 인터페이스를 참조할 수 있는 타입 변수를 의미한다.

```tsx
// string 타입을 사용할 때
const name: string = 'Jack';

// 타입 별칭을 사용할 때
type NameType = string;
const name: MyName = 'Jack';
```

- 타입 별칭도 제네릭을 사용할 수 있다.

```tsx
type User<T> = {
  name: T
}
```

### 2. 유니온 타입

- 타입 별칭을 사용하면 참조할 수 있는 타입 변수뿐만 아니라 유니온 타입으로 부여할 수 있다.

```tsx
type ID = number | string;
```

### 3. 타입 별칭과 인터페이스

- 타입 별칭과 인터페이스는 매우 유사하고, 동작도 비슷하다.
- 타입 별칭과 인터페이스는 확장, 새로운 필드 추가 등에서 차이가 존재한다.

1) 타입 확장

```tsx
// 인터페이스는 extends 키워드를 사용한다.
interface Animal {
  name: string
}

interface Bear extends Animal {
  honey: boolean
}

const bear = getBear() 
bear.name
bear.honey

// 타입 별칭은 & 키워드를 사용한다.
type Animal = {
  name: string
}

type Bear = Animal & { 
  honey: Boolean 
}

const bear = getBear();
bear.name;
bear.honey;
```

2) 타입 필드 추가

```tsx
// 인터페이스는 동일 식별자로 타입을 작성하면 덮어 씌워진다.
interface Window {
  title: string
}

interface Window {
  ts: TypeScriptAPI
}

const src = 'const a = "Hello World"';
window.ts.transpileModule(src, {})

// 타입 별칭은 필드 추가가 불가능하다.
type Window = {
  title: string
}

type Window = {
  ts: TypeScriptAPI
}

 // Error: Duplicate identifier 'Window'.
```