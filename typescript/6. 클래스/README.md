# 6. 클래스

### 1. readonly

- 클래스 속성에 readonly를 사용하면 수정은 안되고 접근만 가능하다.

```tsx
class Developer {
    readonly name: string;
    constructor(theName: string) {
        this.name = theName;
    }
}
const john = new Developer("John");
john.name = "John"; // Cannot assign to 'name' because it is a read-only property.
```

### 2. Accessor

```tsx
class Developer {
  private fullName: string;

  constructor(fullName?: string) {
	  this.fullName = fullName ?? "John";
  }
  
  get name(): string {
    return this.fullName;
  }

  set name(newValue: string) {
    if (newValue && newValue.length > 5) {
      throw new Error('이름이 너무 깁니다');
    }
    this.fullName = newValue;
  }
}
const josh = new Developer();
josh.name = 'Josh Bolton'; // Error 이름이 너무 깁니다

josh.name = 'Josh';
console.log(josh.name); // Josh
```

- name이라는 속성에 제약 사항을 추가하고 싶을 때 get, set 키워드를 활용할 수 있다.

### 3. ****Abstract Class****

```tsx
abstract class Developer {
  abstract coding(): void; // 'abstract'가 붙으면 상속 받은 클래스에서 무조건 구현해야 함
  drink(): void {
    console.log('drink sth');
  }
}

class FrontEndDeveloper extends Developer {
  coding(): void {
    // Developer 클래스를 상속 받은 클래스에서 무조건 정의해야 하는 메서드
    console.log('develop web');
  }
  design(): void {
    console.log('design web');
  }
}
const dev = new Developer(); // error: cannot create an instance of an abstract class
const josh = new FrontEndDeveloper();
josh.coding(); // develop web
josh.drink(); // drink sth
josh.design(); // design web
```