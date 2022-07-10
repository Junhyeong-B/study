# 14. 유틸리티 타입

- 유틸리티 타입은 이미 정의해 놓은 타입을 변환할 때 사용하기 좋은 타입 문법이다.

# 1. **`Partial<Type>`**

- 특정 타입의 부분 집합을 만족하는 타입.
- 모든 타입이 필수인 인터페이스에 Partial 타입을 사용하면 모든 타입이 있어도 되고 없어도 되는(Optional) 타입이라고 생각할 수 있다.

```tsx
interface Address {
  email: string;
  address: string;
}

type MayHaveEmail = Partial<Address>;
const me: MayHaveEmail = {}; // 가능
const you: MayHaveEmail = { email: 'test@abc.com' }; // 가능
const all: MayHaveEmail = { email: 'capt@hero.com', address: 'Pangyo' }; // 가능
```

## 1-1) Partial 정의

```tsx
type Partial<T> = {
    [P in keyof T]?: T[P];
};
```

- T 타입의 키 값인 P를 이용해서 index signatures로 `T[P]` 타입으로 지정하는데 모든 값을 옵셔널로 만들어주고 있다.

# 2. **`Required<Type>`**

- Type의 모든 속성이 필수로 설정되어 구성된 타입을 생성한다.

```tsx
interface Props {
  a?: number;
  b?: string;
}
 
const obj: Props = { a: 5 };
 
const obj2: Required<Props> = { a: 5 }; // b 프로퍼티가 없어서 에러 발생
// Property 'b' is missing in type '{ a: number; }' but required in type 'Required<Props>'.
```

## 2-1) Required 정의

```tsx
type Required<T> = {
    [P in keyof T]-?: T[P];
};
```

- 인덱스 시그니처로 정의되어 있는데, 여기서 `-?` 는 필수(?를 제거(?))를 의미한다.

# 3. **`Record<Keys, Type>`**

- 객체나 맵 같은 유형의 변수를 정의할 때 key값은 Keys 타입, value 값은 Type 타입이 된다.

```tsx
interface CatInfo {
  age: number;
  breed: string;
}

type CatName = "miffy" | "boris" | "mordred";

const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};
```

## 3-1) Record 정의

```tsx
type Record<K extends keyof any, T> = {
    [P in K]: T;
};
```

# 4. **`Pick<Type, Keys>`**

- Type에 존재하는 타입 중 Keys만으로 이루어진 타입으로 구성된다.

```tsx
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}
 
type TodoPreview = Pick<Todo, "title" | "completed">;
 
const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```

- todo 변수에 Todo 타입을 사용했다면 description 타입이 존재하지 않아 에러가 발생할 것이다.

## 4-1) Pick 정의

```tsx
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};
```

# 5. **`Omit<Type, Keys>`**

- Type에 존재하는 타입 중 Keys를 제외한 나머지 타입으로 이루어진 타입으로 구성된다.

```tsx
interface Todo {
  title: string;
  description: string;
  completed: boolean;
  createdAt: number;
}
 
type TodoPreview = Omit<Todo, "description">;
 
const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
  createdAt: 1615544252770,
};
```

## 5-1) Omit 정의

```tsx
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;
```

## 5-2) Exclude

```tsx
type Exclude<T, U> = T extends U ? never : T;

type Type = Exclude<"a" | "b" | "c", "a">;  
// type Type = "b" | "c"
```

- Exclude는 T의 타입 중 U의 타입을 제외한 나머지들로 타입을 구성한다.