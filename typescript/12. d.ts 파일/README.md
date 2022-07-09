# 12. d.ts 파일

# 1. 타입스크립트 선언 파일

- TypeScript 3.7에서 JSDoc 구문을 사용하여 `d.ts` 파일을 지원하고 있다.
- d.ts 파일은 타입스크립트 코드의 타입 추론을 돕는 파일이다.

```tsx
declare global {
  interface Window {
    myNewProperty: string;
  }

	interface Document {
		webkithidden: boolean;
	}
}
```

- 위 코드는 Window 객체에 myNewProperty라는 프로퍼티의 타입을 추가하였고, 원래는 없어서 오류가 발생했을 `window.myNewProperty`에 접근할 수 있게 된다.

# 2. 전역 변수와 전역 함수에 대한 타입 선언

- 해당 타입스크립트 파일에서 사용할 순 있지만 선언되어 있지 않은 전역 변수나 전역 함수는 아래와 같이 타입을 선언할 수 있다.

```tsx
// 전역 변수
declare const pi = 3.14;

// 전역 함수
declare namespace myLib {
  function greet(person: string): string;
  let name: string;
}
myLib.greet('캡틴');
myLib.name = '타노스';
```