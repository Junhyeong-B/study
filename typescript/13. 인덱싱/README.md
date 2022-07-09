# 13. 인덱싱

# 1. 배열 요소 접근(indexing signatures)

- 인터페이스를 사용할 때 인덱싱 타입을 정의할 수 있다.

```tsx
interface StringArray {
  [index: number]: string;
}
 
const myArray: StringArray = getStringArray();
const secondItem = myArray[1];
```

- 이렇게 작성하면 `StringArray` 타입을 갖는 배열은 `Array[number]` 형태로 접근했을 때 값이 string이라는 것을 알 수 있다.

# 2. 배열 변경 제한하기

```tsx
interface ReadonlyStringArray {
  readonly [index: number]: string;
}

const arr: ReadonlyStringArray = ['Thor', 'Hulk'];
arr[2] = 'Capt'; // Error!
```

- readonly 키워드를 이용해서 배열의 값이 변경되는 것을 막을 수 있다.

# 3. 타입 별칭에서의 인덱스

- 타입 별칭에서는 indexed access type을 통해 값을 사용할 수 있다.

```tsx
type Person = { age: number; name: string; alive: boolean };
type Age = Person["age"];
// type Age = number;
```