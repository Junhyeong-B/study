# 1. 기본 타입

- String

```tsx
const str: string = "hi";
```

- Number

```tsx
const num: number = 10;
```

- Boolean

```tsx
const show: boolean = true;
```

⇒ 의견! Boolean 값으로 `open` 이름을 사용하고 싶다면 변수: `open`, getter: `isOpen`, setter: `setIsOpen` 형태로 사용하는 편이다. 변수 자체를 `isOpen`으로 사용하면 getter, setter를 사용할 때 변수 이름이 복잡해질 수 있다.

- Array

```tsx
const arr: number[] = [1, 2, 3];
const arr2: Array<number> = [4, 5, 6];
```

- Tuple

```tsx
const tuple: [string, number] = ["hi", 10];
```

⇒ 짝수 index에는 string만, 홀수 index에는 number만 사용 가능

- Enum

```tsx
enum Avengers {
	Capt,
	IronMan,
	Thor = 4,
}

const Capt = Avengers.Capt;
const IronMan = Avengers[1]; // IronMan
const Thor = Avengers[4]; // Thor
```

- Any

```tsx
let variable: any = 1;
variable = true;
variable = "hi"; // Error 발생하지 않는다.
```

- void

변수에는 undefined, null만 할당하고, 함수에서는 반환 값을 설정할 수 없는 타입.

```tsx
const unUseful: void = undefined;
```

- Never

함수의 끝에 절대 도달하지 않는다는 의미

```tsx
function neverEnd(): never {
	while (true) {}
}
```