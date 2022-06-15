# 5. 연산자를 이용한 타입 정의

### 1. Union Type

- Javascript에서 비교 곱 연산자(`||`)와 같이 A 이거나 B 라는 의미이다.

```tsx
type Union = string | number;
```

### 2. Union Type의 장점

```tsx
function getAge(age: number | string) {
  if (typeof age === 'number') {
    age.toFixed(); // 정상 동작, age의 타입이 `number`로 추론되기 때문에 숫자 관련된 API를 쉽게 자동완성 할 수 있다.
    return age;
  }
  if (typeof age === 'string') {
    return age;
  }
  return new TypeError('age must be number or string');
}
```

- 특정 타입에 따라 분기하는 경우 자동완성이 가능하고, 특정 상황에 대한 에러를 컴파일 단계에서 확인할 수 있다.

### 3. ****Intersection Type****

- 여러 타입을 모두 만족하는 하나의 타입.
- 인터페이스에서 extends 키워드를 통해 확장된 인터페이스와 유사하다.

```tsx
interface Person {
  name: string;
  age: number;
}

interface Developer {
  name: string;
  skill: number;
}

type Human = Person & Developer;

/*
 * {
 * 	  name: string;
 *	  age: number;
 * 	  skill: string;
 * }
 */
```

### 4. ****Union Type을 쓸 때 주의할 점****

```tsx
interface Person {
  name: string;
  age: number;
}
interface Developer {
  name: string;
  skill: string;
}
function introduce(someone: Person | Developer) {
  someone.name; // O 정상 동작
  someone.age; // X 타입 오류
  someone.skill; // X 타입 오류
}
```

- 여기서 someone은 Person도 될 수 있고, Developer도 될 수 있다는 의미보다는 introduce를 호출하는 시점에 someone이 Person인지 Developer인지 추론할 수 없게 된다.
- 따라서 위 경우 Person일 때 동작, Developer일 때 동작을 분기하여 작성해 주어야 한다.
- 그러나 유니온 타입에 겹치는 속성이 존재한다면 해당 속성은 사용할 수 있다.(여기선 `someone.name`)