# 15. 맵드 타입(Mapped Type)

- 맵드 타입이란 기존에 정의되어 있는 타입을 새로운 타입으로 변환해 주는 문법을 의미한다.

```tsx
type OnlyBoolsAndHorses = {
  [key: string]: boolean | Horse;
};
 
const conforms: OnlyBoolsAndHorses = {
  del: true,
  rodney: false,
};
```

# 1. 자바스크립트의 map 함수

```tsx
var arr = [{ id: 1, title: '함수'}, { id: 2, title: '변수'}, { id: 3, title: '인자'}];
var result = arr.map(function(item) {
  return item.title;
});
console.log(result); // ['함수', '변수', '인자'];
```

- map 함수는 자바스크립트 내장 API로 key, value 값을 갖는다.

# 2. ****맵드 타입의 기본 문법****

- mapped type은 자바스크립트 map 함수와 유사하다.

```tsx
{ [ P in K ] : T }
{ [ P in K ] ? : T }
{ readonly [ P in K ] : T }
{ readonly [ P in K ] ? : T }
```

# 3. 맵드 타입 기본 예제

```tsx
type Heroes = 'Hulk' | 'Thor' | 'Capt';

type HeroProfiles = { [K in Heroes]: number };
const heroInfo: HeroProfiles = {
  Hulk: 54,
  Thor: 1000,
  Capt: 33,
}
```

- HeroProfiles 타입을 정의할 때 K 타입은 Heroes를 순회하면서 해당 값을 number 타입으로 정의한다.

```tsx
type HeroProfiles = {
  Hulk: number; // 첫번째 순회
  Thor: number; // 두번째 순회
  Capt: number; // 세번째 순회
}
```

# 4. 실용 예제

```tsx
type Subset<T> = {
  [K in keyof T]?: T[K];
}
```

```tsx
interface Person {
  age: number;
  name: string;
}

type SubsetPerson = Subset<Person>;
// Person {
//  age?: number;
//  name?: string;
// }
```

- 이러한 형태는 Partial 타입이 정의된 것과 동일하다.
- 실제로 유틸리티 타입에서 맵드 타입이 사용되고 있다.