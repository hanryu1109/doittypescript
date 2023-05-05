# 01. 타입스크립트와 개발 환경 만들기

## 01-1. 타입스크립트란 무엇인가?

- ESNext에 타입(type) 기능을 추가한 타입스크립트
- 자바스크립트 코드는 바벨(Babel)이라는 트랜스파일러(transpiler)를 거치면서 ES5 자바스크립트 코드로 변환됩니다. (트랜스파일러란 어떤 프로그래밍 언어로 작성된 소스코드를 또 다른 프로그래밍 언어로 된 소스코드로 바꿔주는 프로그램을 말합니다. 트랜스파일러는 텍스트로 된 소스코드를 바이너리 코드로 바꿔주는 컴파일러와 구분하기 위해 생긴 용어입니다.)
- 타입스크립트 소스코드는 TSC(TypeScript compiler)라는 트랜스 파일러를 통해 ES5 자바스크립트 코드로 변환됩니다.

## 01-2 타입스크립트 주요 문법 살펴보기

### ESNext의 주요 문법

1. 비구조화 할당(destructuring assignment)

- 객체와 배열에 적용 가능

```js
let person = { name: "Jane", age: 22 };
let { name, age } = person; // name = "Jane", age = 22

let array = [1, 2, 3, 4];
let [head, ...rest] = array; // head = 1, rest = [2, 3, 4]

let a = 1,
  b = ((2)[(a, b)] = [b, a]); // a = 2, b = 1
```

2. 화살표 함수

- function 키워드 외에도 화살표(=>)로 함수 선언 가능

```js
function add(a, b) {
  return a + b;
}
const add2 = (a, b) => a + b;
```

3. 클래스

- 객체지향 프로그래밍을 가능케 합니다.
- 객체지향 프로그래밍은 프로그래밍 언어가 '캡슐화', '상속', '다형성'이라는 세 가지 요소를 지원합니다.

```js
abstract class Animal {
    constructor(public name?: string, public age?: number) { }
    abstract say(): string
}

class Cat extends Animal {
    say() {return '야옹'}
}

class Dog extends Animal {
    say() {return '멍멍'}
}

let animals: Animal[] = [new Cat('야옹이', 2), new Dog('멍멍이', 1)]
let sounds = animals.map((animal) => animal.say())
```

4. 모듈

- 모듈을 사용하면 코드를 여러 개 파일로 분할해서 작성할 수 있습니다.
- export, import

```js
import * as fs from 'fs
export function writeFile(filepath: string, content: any) {
    fs.writeFile(filepath, content, (err) => {
        err && console.log('error', err)
    })
}
```

5. 생성기

- `yield` 문은 반복자를 의미하는 반복기(iterator)를 생성할 때 사용합니다.
- 반복기는 독립적으로 존재하지 않고 반복기 제공자(iterable)를 통해 얻습니다.
- 이 처럼 `yield` 문을 이용해 반복기를 만들어내는 반복기 제공자를 생성기(generator) 라고 부릅니다.
- 타입스크립트에서 `yield`는 반드시 `function*`으로 만들어진 함수 내부에서만 사용할 수 있습니다.

```js
function* gen() {
  yield* [1, 2];
}

for (let value of gen()) {
  console.log(value); // 1, 2
}
```

6. Promise와 async/await 구문

- Promise는 비동기 콜백 함수를 상대적으로 쉽게 구현할 목적으로 만들어졌습니다.
- async/await 구문을 빌려서 여러 개의 Promise 호출을 결합한 좀 더 복잡한 형태의 코드를 간결하게 구현할 수 있습니다.

```js
async function get() {
  let values = [];
  values.push(await Promise.resolve(1));
  values.push(await Promise.resolve(2));
  values.push(await Promise.resolve(3));

  return values;
}

get().then((values) => console.log(values)); // [1. 2. 3]
```

### 타입스크립트 고유의 문법 살펴보기

1. 타입 주석과 타입 추론

- 다음 코드에서 변수 n 뒤에는 콜론(:)과 타입 이름이 있습니다. 이것을 `타입 주석(type annotation)`이라고 합니다.
- 변수 m 처럼 타입 부분을 생략할 수도 있습니다. 타입 부분이 생략되면 오른쪽 값을 분석해 왼쪽 변수의 타입을 결정합니다. 이를 `타입 추론(type inference)` 이라고 합니다.

```js
let n: number = 1;
let m = 2;
```

2. 인터페이스

```js
interface Person {
    name: string;
    age?: number;
}

let person: Person { name: "Jane" }
```

3. 튜플

- 물리적으로는 배열과 같습니다.
- 배열에 저장되는 아이템의 데이터 타입이 모두 같으면 배열, 다르면 튜플입니다.

```js
let numberArray: number[] = [1, 2, 3]; // 배열
let tuple: [boolean, number, string] = [true, 1, "Ok"]; // 튜플
```

4. 제네릭 타입

- 제네릭 타입은 다양한 타입을 한꺼번에 취급할 수 있게 헤줍니다.
- 다음 코드에서 Container 클래스는 value 속성을 포함합니다.
- 이 클래스는 `Container<number>`, `Container<string>`, `Container<number[]>`, `Container<boolean>` 처럼 여러 가지 타입을 대상으로 동작할 수 있는데 이를 '제네릭 타입(generic type)'이라고 합니다.

```js
class Container<T> {
    constructor(public value: T) {}
}

let numberContainer: Container<number> = new Container<number>(1)
let stringContainer: Container<string> = new Container<string>("this is string")
```

5. 대수 타입

- ADT란, 추상 데이터 타입(abstract data type)을 의미하기도 하지만 대수 타입(algebraic data type)이라는 의미로도 사용됩니다.
- 대수 타입이란, 다른 자료형의 값ㅇ르 가지는 자료형을 의미합니다. ❓❓❓❓
- 대수 타입에는 크게 합집합 타입(union 또는 sum type)과 교집합 타입(intersection 또는 product type) 두 가지가 있습니다.
- 합집합 타입은 '|' 기호를, 교집합 타입은 '&' 기호를 사용해 여러 타입을 결합해서 만들 수 있습니다.

```js
type NumberOrString = number | string;
type AnimalAndPerson = Animal & Person;
```
