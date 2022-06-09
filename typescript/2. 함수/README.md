# 2. 함수

### 1. 타입 스크립트에서의 함수

- 함수의 파라미터 타입
- 함수의 반환 타입
- 함수의 구조 타입

### 2. 함수의 기본적인 타입 선언

```tsx
function sum(a: number, b: number): number {
	return a + b;
}
```

### 3. 함수의 인자

```tsx
sum(10, 20); // 30
sum(10, 20, 30); // Error
sum(10); // Error
```

- 타입스크립트에서는 함수의 인자를 모두 필수 값으로 간주하고, 매개변수를 넘겨주지 않거나 추가로 넘겨주게 되면 에러가 발생한다.
- 자바스크립트에서는 arguments 내부 객체로 인해 해당 에러는 발생하지 않는다.
    - 적게 들어오면 남는 파라미터는 undefined 값이 되고, 많이 들어오면 arguments 내부 객체에 저장된다.

### 4. Rest 문법이 적용된 매개변수

```tsx
function sum(a: number, ...nums: number[]): number {
	let totalSum = 0;

	for (const num of nums) {
		totalSum += num;
	}

	return a + totalSum;
}
```

### 5. this

- 타입스크립트에서 this는 `this: type` 형태로 명시한다.

```tsx
interface Vue {
	el: string;
	count: number;
	init(this: Vue): () => {};
}

const vm: Vue = {
	el: "#app",
	count: 10,
	init: function(this: Vue) {
		return () => {
			return this.count;
		}
	}
}

const getCount = vm.init();
const count = getCount();
console.log(count); // 10
```

### 6. 콜백에서의 this

- 콜백으로 함수가 전달되었을 때 this는 아래와 같이 선언할 수 있다.
    - `this: void`는  this 타입을 선언할 필요가 없다는 뜻이다.

```tsx
interface UIElement {
	addClickListener(onClick: (this: void, e: Event) => void): void;
}

class Handler {
    info: string;
    onClick(this: void, e: Event) {
        // `this`의 타입이 void이기 때문에 여기서 `this`를 사용할 수 없습니다.
        console.log('clicked!');
    }
}
let handler = new Handler();
uiElement.addClickListener(handler.onClick);
```