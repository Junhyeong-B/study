# 9. 타입 호환

### 1. 타입 호환?

- 타입 호환이란 타입스크립트에서 특정 타입이 다른 타입에 잘 맞는지를 의미한다.

```tsx
interface Named {
    name: string;
}

class Person {
    name: string;
}

let p: Named;
// OK, 타입 호환이 되기 때문.
p = new Person();
```

### 2. 구조적 타이핑

- 오직 멤버만으로 타입을 관계시키는 방식이다.

```tsx
interface Named {
    name: string;
}

let x: Named;

let y = { name: "Alice", location: "Seattle" };
// y의 추론된 타입은 { name: string; location: string; }

x = y;
```

- `x`에 `y`를 할당하려고 할 때 `x`의 각 프로퍼티를 검사하여 `y`에 상응하는 호환 가능한 프로퍼티를 찾는다.
- 여기서 `y`는 `name: string`이라는 타입을 갖고 있기 때문에 할당이 가능해진다.

### 3. Enum 타입 호환 주의

- `Enum` 타입은 `number` 타입과 호환되지만 `Enum` 타입 끼리는 호환되지 않는다.

```tsx
enum Status { Ready, Waiting };
enum Color { Red, Blue, Green };

let customStatus = Status.Ready;
customStatus = Color.Green;  // 'Color.Green' 형식은 'Status' 형식에 할당할 수 없습니다.ts(2322)
```

### 4. Class 타입 호환 주의

- 클래스 타입끼리 비교할 때 Static member(스태틱 멤버)와 Constructor(생성자)를 제외한 속성만 비교한다.

```tsx
class Animal {
  feet: number;
  constructor(name: string, numFeet: number) {}
}
class Size {
  feet: number;
  constructor(numFeet: null) {}
}

let a: Animal = new Animal("", 0);
let s: Size = new Size(null);
a = s; // 성공
s = a; // 성공
```

### 5. 제네릭

- 제네릭 타입 간의 호환 여부는 타입 인자`<Type>`이 속성에 할당 됐는지를 기준으로 한다.

```tsx
interface Empty<T> {}
let x: Empty<number>;
let y: Empty<string>;

x = y; // 성공, y는 x의 구조와 대응하기 때문
```

- 그러나 인터페이스를 통해 제네릭을 할당하는 것은 다르게 동작한다.

```tsx
interface NotEmpty<T> {
  data: T;
}

let x: NotEmpty<number>;
let y: NotEmpty<string>;
x = y; // 오류, x와 y 는 호환되지 않음
```