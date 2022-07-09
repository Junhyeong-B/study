# 11. 모듈

# 1. Export

- ES6의 모듈 export, import 키워드를 Type에 대해서도 사용할 수 있다.
- Export는 타입 별칭(Type Alias), 인터페이스 앞에 붙여서 내보내기할 수 있다.

```tsx
export interface StringValidator {
  isAcceptable(s: string): boolean;
}

export type StringValidator2 = {
	isValidate(s: string): boolean;
}
```

# 2. Import

Import는 일반적인 변수, 함수, 클래스 등을 import하는 것과 동일하게 작성한다.

```tsx
import { StringValidator } from "./StringValidator";
```

## 2-1) Importing Types

- 타입스크립트 3.8 버전 이전까지는 `import` 구문만으로 타입을 가져오기 했지만, 타입스크립트 3.8버전부터 `import` 구문, `import type` 구문 둘 다 사용할 수 있다.

```tsx
// Re-using the same import
import { APIResponseType } from "./api";

// Explicitly use import type
import type { APIResponseType } from "./api";
```

- 두 개의 구문에는 동작하는데 있어 차이는 없지만 가져오려는 모듈이 타입인지 아닌지 명시적으로 알 수 있기 때문에 필요에 따라 구분해서 사용할 수 있다.