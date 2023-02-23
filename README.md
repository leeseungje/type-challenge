# type-challenge

- https://github.com/type-challenges/type-challenges 해당 깃에서 하는 챌린지 해보기

## 하기전 공부해야할 내용

### 정적 vs 동적

- 정적은 내가 생성한게 아닌 이미 생성되어 있는것(static)
- 동적은 내가 그때그때 생성하는것

### Generic

- 어떤 클래스 또는 함수에서 사용할 타입을 그 함수나 클래스를 사용할 때 결정하는 프로그래밍 기법
- 기본적으로 특정 타입을 위해 만들어진 클래스나 함수를 다른 타임을 위해 재사용을 할 수가 없다.
- 그래서 제네릭을 통해 범용적인 사용을 가능케 한다.

### Generic 문법

- 클래스

```javascript
class Stack<T> {
  private data: T[] = [];

  constructor() {}

  push(item: T): void {
    this.data.push(item);
  }

  pop(): T {
    return this.data.pop();
  }
}

const numberStack = new Stack<number>();
const stringStack = new Stack<string>();
numberStack.push(1);
stringStack.push('a');
```

- '<T>': Type의 약자로 제네릭을 선언할때 많이 사용 무엇이든 들어간다.

---

-함수

```javascript
function first(arr: any[]): any {
  return arr[0]
}
first < number > [1, 2, 3] // 1
////////////////////////////////////////////////////////////
function toPair(a: any, b: any): [any, any] {
  return [a, b]
}
toPair < string, number > ('1', 1) // [ '1', 1 ]
toPair < number, number > (1, 1) // [ 1, 1 ]
```

- 해당방식을 제네릭을 사용 시

```javascript
function first<T>(arr: T[]): T {
  return arr[0]
}
first < number > [1, 2, 3] // 1
////////////////////////////////////////////////////////////////
function toPair<T, U>(a: T, b: U): [T, U] {
  return [a, b]
}
toPair < string, number > ('1', 1) // [ '1', 1 ]
toPair < number, number > (1, 1) // [ 1, 1 ]
```

-`T`,`U` 두 가지 타입 변수는 T의 알파벳 순서대로 사용하면 된다.

---

- 상속된 타입 변수
  타입 변수는 기존에 사용하고 있는 타입을 상속할 수도 있다.
  이 점을 이용하면 입력 받을 변수의 타입을 제한할 수 있다.

```javascript
function getFirst<T extends Stack<U>, U>(container: T): U {
  const item = container.pop();
  container.push(item);
  return item;
}

```

---

- `extends`[확장]: 키워드는 새로운 클래스의 '상속'을 위해 사용한다. 상위 클래스의 모든 프로퍼티와 메서드들을 갖고 있으므로 일일이 정의하지 않아도 된다.
  상위 클래스의 프로퍼티를 지정하지 않으면, 초기값으로 선언되며 에러는 반환하지 않는다.
  ```javascript
  interface BasicAddress {
    name?: string;
    street: string;
    city: string;
    country: string;
    postalCode: string;
  }
  interface AddressWithUnit extends BasicAddress {
    unit: string;
  }
  //{
  //  name?: string;
  //  street: string;
  //  city: string;
  //  country: string;
  //  postalCode: string;
  //  unit: string
  //}
  ```

---

- `keyof`연산자: object에 사용하면 object가 가지고 있는 모든 key값을 union type으로 합쳐서 내보내 준다.

```javascript
interface Car {
	name: string;
    price: number;
}

type CarKeys = keyof Car;
let carPrice: CarKeys = 'price';
let carName: CarKeys = 'name';
let carName2: Carkeys = 'namename'; // 'price' or 'name' 만 가능하다.
```
