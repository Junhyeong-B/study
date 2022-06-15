# 4. 이넘

### 1. 이넘

- 특정 값들의 집합을 의미하는 자료형

### 2. 숫자형 이넘

```tsx
enum Direction {
  Up,   // 0
  Down, // 1
  Left, // 2
  Right // 3
}
```

```tsx
enum Direction {
  Up = 1,  // 1
  Down,    // 2
  Left,    // 3
  Right    // 4
}v
```

### 3. 숫자형 이넘의 사용

```tsx
enum UserResponse {
  No = 0,
  Yes = 1,
}
 
function respond(recipient: string, message: UserResponse): void {
  // ...
}
 
respond("Princess Caroline", UserResponse.Yes);
```

### 4. 문자형 이넘

- 숫자형 이넘은 속성 중 앞의 속성 하나만 초기화해도 뒤의 속성 값이 정해지지만, 문자형 이넘은 전부 다 특정 문자로 초기화 해줘야 한다.

```tsx
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}
```

### 5. 복합 이넘

```tsx
enum BooleanLikeHeterogeneousEnum {
  No = 0,
  Yes = "YES",
}
```

- 위와 같이 복합적으로 사용할 수는 있지만 권장하지는 않는다.

### 6. 런타임 시점에서의 이넘 특징

```tsx
enum E {
  X,
  Y,
  Z,
}
 
function f(obj: { X: number }) {
  return obj.X;
}
 
// Works, since 'E' has a property named 'X' which is a number.
f(E);
```

- 이넘은 런타임 시점에 실제 존재하는 실제 객체로 속성을 사용할 수 있다.

### 7. 컴파일 시점에서의 이넘 특징

```tsx
enum LogLevel {
  ERROR,
  WARN,
  INFO,
  DEBUG,
}
 
type LogLevelStrings = LogLevel;
 
function printImportant(key: LogLevelStrings, message: string) {
	// ...
}
printImportant("ERROR", "This is a message");
// Argument of type '"ERROR"' is not assignable to parameter of type 'LogLevel'.
```

- 컴파일 시점에서는 실제 존재하는 객체가 아니므로 에러가 발생한다.
- 이를 사용하고 싶으면 `keyof typeof` 키워드와 함께 사용해야 한다.

```tsx
/**
 * This is equivalent to:
 * type LogLevelStrings = 'ERROR' | 'WARN' | 'INFO' | 'DEBUG';
 */
type LogLevelStrings = keyof typeof LogLevel;

printImportant("ERROR", "This is a message");
```

### 8. 리버스 매핑

```tsx
enum Enum {
  A,
}
 
let a = Enum.A;        // 0
let nameOfA = Enum[a]; // "A"
```