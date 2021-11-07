# TypeScript의 기본 타입

TypeScript : static type (개발 중간에 오류 발견)<br />
JavaScript : dynamic type (런타입에 돌입해야 오류 발견)

```ts
 // JavaScript
function add(n1, n2) {
    if (typeof n1 !== 'number' || typeof n2 !== 'number') {
        throw new Error('Incorrect input!');
    }
    return n1 + n2;
}

// TypeScript
function add(n1 : number, n2: number) {
    return n1+n2;
}
```

프로그램이 유용하려면 가장 간단한 데이터 단위로 작업할 수 있어야한다. (numbers, strings 등등)<br />
TypeScript에서는 JavaScript와 동일한 타입을 지원하며, 추가적으로 열거 타입이 제공되었다.

<br />

**TypeScript 에서 프로그램 작성을 위해 기본 제공하는 데이터 타입**
- 사용자가 만든 타입은 결국 하위의 기본 자료형들로 쪼개진다.
- JavaScript 기본 자료형을 포함 (superset)
- ECMAScript 표준에 따른 기본 자료형은 6가지
    - Boolean
    - number
    - String
    - Null Undefined
    - Symbol (ECMAScript 6에 추가)
    - Array: obejct 형
- 프로그래밍을 돕기 위해 추가적으로 제공된 자료형
    - Any
    - Vode
    - Never
    - Unknown
    - Enum
    - Tuple: object형

<br />

**Primitive Type**
<br />
- 오브젝트와 레퍼런스 형태가 아닌 실제 값을 저장하는 자료형
- primitive type의 내장 함수를 사용 가능한 것은 자바스크립트 처리 방식 덕분
```ts
let name = 'mark';

name.toString();
```

- ES2015 기준 6가지
    - boolean
    - number
    - string
    - symbol(ES2015)
    - null
    - undefined

- literal 값으로 Primitive 타입의 서브 타입을 나타낼 수 있다.
```ts
true; // primitive 타입인 boolean의 서브타입
```
- warpper 객체 생성 가능 (권장 X)
    - new Boolean(false); // object 타입
    - new String('world'); // object 타입
    - new Number(42); // object 타입

<br />

**Type Casing**
- TypeSciript의 핵심 primitive types은 모두 소문자
- Number, String Boolean, Symbol 또는 Object 유형과 소문자 버전은 동일하지 않다.
- 위의 유형은 primitives를 나타내지 않으며, 타입으로 사용해서는 안된다.
    - 대신 number, string, boolean, object and symbol 타입을 사용한다

<br />

**Symbol**
- ECMAScript 2015의 symbol
- new Symbol 사용은 사용할 수 없다.
- Symbol을 함수로 사용해서 symbol 타입읆 만들어낼 수 있다.
```ts
console.log(Symbol('foo') === Symbol('foo'));
```
- 프리미티브 타입의 값을 담아서 사용한다.
- 고유하고 수정불가능한 값으로 만들어준다.
- 주로 접근을 제어하는데 쓰는 경우가 많다.

```ts
let sym = Symbol();
let obj = {
    [sym]:"value";
};

console.log(obj[sym]); // "value"
```
<br />

**Undefined & Null**
- TypeScript에서 undefined 와 null은 실제로 각각 undefined 및 null 이라는 타입을 가진다
- void와 마찬가지로, 그 자체로는 그다지 유용하지 않다.
- 타입과 값 모두 소문자로만 존재한다.
- 따로 설정을 하지 않으면 모든 타입의 서브타입으로 사용할 수 있다
- tsconfig 파일에서 strictNullChecks 옵션을 활성화하면, null과 undefined는 void나 자기 자신들에게만 할당할 수 있다.
    - 이 경우, null과 undefined를 할당할 수 있게 하려면, union Type(|)을 이용해야 한다.
- null in JavaScript
    - 무언가가 있지만 사용할 준비가 덜 된 상태 
    - null 타입은 null 값만 가질 수 있다 
    - 런타임에서 typeof 연산자를 이용해서 null 타입의 값을 찍어내면 object 타입이 출력된다
- undefined in JavaScript
    - 무언가가 아예 준비가 안된 상태
    - 값을 할당하지 않은 변수는 undefined의 값을 가진
    - obejct의 property가 없을때도 undefined
    - 런타입에서 typeof 연산자를 이용해서 undefined 타입의 값을 찍어내면 undefined 타입이 출력된다
    
<br />

**Object**
 - 직접 값을 가지지않고, 주소값을 가짐
 - primitive type이 아닌것을 나타내고 싶을 때 사용하는 타입

```tsx
// create by object literal
const person1 = {name : 'Mark', age : 39};
// type {name : string, age: number}

// create by Object.create -> 전역 내장객체
const person2 = Object.create({name:'Mark', age:39});
// type (o: object | null)
```
<br />

**Array**
 - 같은 타입의 요소들을 모아둔 자료형
 - 사용법
    - Array<타입>
    - 타입[]

```tsx
let list1: number[] = [1,2,3];
let list2: Array<number> = [1,2,3]; // 의미는 동일하나 jsx나 tsx에서 충돌할 가능성이 있기때문에 위의 표현법을 추천
let list3:(number | string)[] = [1,2,3,"4"]; // 이런 방법으로도 사용 가능
```
<br />

**Tuple**
 - 길이와 각 자리의 타입이 정확히 정해져있음

```tsx
let x: [string, number];
x = ["Mark", 39];
x = [24, "Hi"]; // Error
x = ["hello", 2022, "year"]; // Error
x[2] = "world"; // Error
```

```tsx
const person: [string, number] = ["Mark", 39];
const [first, second] = person;
const [first, second, third] = person; // Error
```
<br />

**Any**
 - 어떤 타입이든 될 수 있음
 - 최대한 쓰지 않는다
- 컴파일 타임에 타입 체크가 정상적으로 이뤄지지 않기 때문
 - 컴파일 옵션 중에 any를 써야하는데 쓰지안흔ㄴ 경우 오류를 내는 옵션이 있음
- noImplicitAny
 - log로 표현되는 역할만 하는 경우 any 가능
- any는 개체를 통해 전파
```tsx
let looselyTyped: any = {};
const d = looselyTyped.a.b.c.d;  // Not Error
// d의 타입은 any
```
 - 편의를 위해 쓰는 순간 타입 안전성을 잃는다
```tsx
function leakingany(obj : any) {
const a: number = obj.num; // any로 넘어온 객체의 값에 타입을 지정해줌으로써 누수를 막음
const b = a+1;
return b;
}

const c = leakingAny({num:0})
c.indexOf("0"); // Error : Number에는 indexOf라는 내장함수가 없음
``` 
<br />

**Unknown**
 - Typescript 3.0 버전부터 지원
 - any와 짝으로 any보다 Type-safe한 타입
    - any와 같이 아무거나 할당할 수 있다.
     - unknown타입으로 선언된 변수를 사용하려면 컴파일러가 타입을 추론할 수 있게끔 타입의 유형을 좁히거나 타입을 확정해주어야한다. 

```tsx
declare const maybe: unknown;

const aNumber: number = maybe;

if(maybe === true) {
    const aBoolean: boolean = maybe;

    const aString: string = maybe; // Error
}

if(typeof maybe === 'string') {
    const aString: string = maybe;

    const aBoolean: boolean = maybe; // Error
}
```
<br />

**Never**
 - 모든 타입의 서브타입, 모든 타입에 할당 할 ㅅ ㅜ있다
 - 하지만 never는 어느것도 할당할 수 없음(any 포함)
 - 잘못된 타입을 넣는 실수를 막고자 할 때 사용하기도 함
 - 일반적으로 return에 사용

```tsx
let a: string = "hello";
if(typeof a !== 'string') {
    a; // type never
}

declare const b: string | number;
if(typeof b !== 'string') {
    b; // type number
}

type Indexable<T> = T extends string ? T & {[index: string]: any} : never;
const b: Indexable<{}> == ''; // Error
```
<br />

**Void**
 - 어떤 타입도 가지지 않는 빈 상태
 - undefind와 동일
 ```tsx
function returnVoid(message: string): void {
console.log(message);

return; // void
// return undefined; // 가능
}

const r: undefined = returnVoid("리턴이 없다."); // Error
```
 