# Type System

**작성자와 사용자의 관점으로 코드 바라보기**

타입 시스템
- 컴파일러에게 사용하는 타입을 명시적으로 지정하는 시스템
- 컴파일러가 자동으로 타입을 추론하는 시스템

타입스크립트의 타입 시스템
- 타입을 명시적으로 지정할 수 있다
- 타입을 명시적으로 지정하지 않으면, 타입스크립트 컴파일러가 자동으로 타입을 추론

타입
- 해당 변수가 할 수 있는 일을 결정

<br />

**함수 사용법에 대한 오해를 야기하는 JS**
```js
// JavaScript

// (f2 실행의 결과가 NaN을 의도한 게 아니라면)
// 이 함수의 작성자는 매개변수 a가 number라는 가정하에 함수를 작성
function f2(a) {
return a * 38;
}

// 사용법을 숙지하지 않은 사용자가 문자열을 사용하여 함수를 실행
console.log(f2(10)); // 380
console.log(f2('Mark')); // NaN
```
**타입스크립트의 추론에 의지하는 경우**
```tsx
// a의 타입을 명시적으로 지정하지 않은 경우이기 때문에 a는 any로 추론
// 함수의 리턴 타입은 number로 추론. (NaN도 number의 하나)
function f3(a) {
return a * 38;
}

// 사용자는 a가 any이기 때문에 사용법에 맞게 문자열을 사용
console.log(f2(10)); // 380
console.log(f2('Mark') + 5); // NaN
```

> noImplicityAny
> - 타입을 명시적으로 지정하지 않은경우, 타입스크립트가 추론 중 'any'라고 판단되면 컴파일 에러를 발생시켜 명시적으로 지정하도록 유도

```tsx
// noImplicityAny 옵션 추가
// Error TS7006: Parameter 'a' implicitly has an 'any' type.
function f3(a) {
return a * 38;
}

// 사용자의 코드를 실행할 수 없다 컴파일이 정상적으로 마무리 될 수 있도록 수정해야 함
console.log(f3(10)); // 380
console.log(f3('Mark') + 5); // NaN
```

**number 타입으로 추론된 리턴타입**
```tsx
// 매개변수의 타입은 명시적으로 지정
// 명시적으로 지정되지 않은 함수의 리턴타입은 number로 추론
function f4(a:number) {
 if (a>0) {
return a*38;
}
}

// 해당 함수의 리턴 타입은 number이기 때문에, 타입에 따르면 이어진 연산 바로 가능
// 하지만 실제론 undefined + 5 가 실행되어 NaN이 출력
console.log(f4(5)); // 190
console.log(f4(-5)); // NaN
```

> strictNullChecks
> - 모든 타입에 자동으로 포함되어 있는 'null'과 'undefined'를 제거

**number | undefined 타입으로 추론된 리턴타입**
```tsx
// 매개변수의 타입은 명시적으로 지정
// 명시적으로 지정되지 않은 함수의 리턴타입은 number | undefined 로 추론
function f4(a:number) {
 if (a>0) {
return a*38;
}
}

// 해당 함수의 리턴 타입은 number | undefined 이기 때문에, 타입에 따르면 이어진 연산 바로 할 수 없음
console.log(f4(5)); // 190
console.log(f4(-5)); // Error TS2532 : Object is possibly 'undefined'.
```

**명시적으로 리턴 타입을 지정해줘야하는 이유**
- 함수 구현부의 리턴 타입과 명시적으로 지정한 타입이 일치하지 않으면 TypeScipt가 컴파일 에러를 발생시키기 때문에 코드를 검토하면서 개발할 수 있음

> noImplicitReturns
>  - 함수 내에서 모든 코드가 값을 리턴하지 않으면 에러 발생

<br />

**매개변수에 object가 들어오는 경우**
```js
// JavaScript
function f6(a) {
    return `이름은 ${a.name} 이고, 연령대는 ${Math.floor(a.age / 10) * 10}대 입니다.`';
}

console.log(f6({name : 'Mark', age : 38})); // 이름은 Mark이고, 연령대는 30대 입니다.
console.log(f6('Mark')); // 이름은 undefined이고, 연령대는 NaN입니다.
```

**Object literal type**
```tsx
function f7(a:{name:string; age: number }): string {
    return `이름은 ${a.name} 이고, 연령대는 ${Math.floor(a.age / 10) * 10}대 입니다.`;
}

console.log(f7({name:'Mark', age: 38})); // 이름은 Mark이고, 연령대는 30대 입니다.
console.log(f7('Mark')); // Error TS2345: Argument of type 'string' is not assignable to parameter of type '{name: string; age: number; }'.
```

**나만의 타입을 만드는 방법**
```tsx
interface PersonInterface {
    name : string;
    age: number;
}

type PersonTypeAlias = {
    name:string;
    age:number;
}

function f8(a: PersonInterface): string {
    return `이름은 ${a.name} 이고, 연령대는 ${Math.floor(a.age / 10) * 10}대 입니다.`;
}

console.log(f8({name:'Mark', age: 38})); // 이름은 Mark이고, 연령대는 30대 입니다.
console.log(f8('Mark')); // Error TS2345: Argument of type 'string' is not assignable to parameter of type 'PersonInterface'.
```
<br />

### Structural Type System vs Nominal Type System
<br/>

**Structual type system**
 - 구조가 같으면 같은 타입이다

 ```ts
interface IPersen {
    name : string;
    age : number;
    speak() : string;
}

type PersonType = {
    name : string;
    age : number;
    speak() : string;
}

let personInterface: IPerson = {} as any;
let personType: PersonType = {} as any;

personInterface = personType;
personType = personInterface;
 ```

 **Nominal type system**
  - 구조가 같아도 이름이 다르면 다른 타입
  - ex : C, Java

```tsx
  // 타입 스크립트에서 nominal type system을 쓰는 법
  type PersonID = string & {readonly brand : unique symbol};

  funciton PersonID(id:string) : PersonID {
      return id as PersonID;
  }

  function getPersonById(id : PersonID) {}

  getPersonById(PersonID('id-aaaaaa'));
  getPersonById('id-aaaaaaa'); // Error
  ```

  **Duck typing** 
   - 사람이 오리처럼 행동하면 오리로 봐도 무관하다라는 방식
   - 객체가 어떤 타입에 걸맞은 변수와 메소드를 지니면 객체를 해당 타입에 속하는 것으로 간주
   - 런타임에 타입 체크해서 에러 발생
   - ex : Python
 
  > 정적 타이핑을 지원하는 타입스크립트가 동적 타이핑인 Duck Typing의 특성이 있는 이유
  > - **타입 검사** 측면에서 해석하면 정적타이핑으로 해석
  > - **다형성의 측면**에서 해석하면 동적타이핑으로 해석

<br />

### 타입호환성
<br />

**서브타입**
```tsx
// sub1 타입은 sup1의 서브타입이다.
let sub1: 1 = 1;
let sup1: number = sub1;
sub1 = sup1; // Error! Type 'number' is not assignable to type '1'

// sub2 타입은 sup2의 서브타입이다.
let sub2: number[] = [1];
let sup2: object = sub2;
sub2 = sup2; // Error! Type '{}' is not assignable to type 'number[]'

// sub3 타입은 sup3의 서브타입이다.
let sub3: [number, number] = [1, 2];
let sup3: number[] = sub3;
sub3 = sup3; // Error! Type 'number[]' is not assignable to type '[number, number]'

// sub4 타입은 sup4의 서브타입이다.
let sub4: number = 1;
let sup4: any = sub4;
sub4 = sup4;

// sub5 타입은 sup5의 서브타입이다.
let sub5: nerver = 0 as never;
let sup5: number = sub5;
sub5 = sup5; // Error! Type 'number' is not assignable to type 'never'

class Animal() {}
class Dog extends Animal {
    eat() {}
}

// sub6 타입은 sup6의 서브타입이다.
let sub6: Dog = new Dog();
let sup6: Animal = sub6;
sub6 = sup6; // Error! Property 'eat' is missing in type 'Animal' but required in type 'Dog'.
```

1. 같거나 서브 타입인 경우 할당 가능 -> 공변
```tsx
// primitive type
let sub7: string = '';
let sup7: string | number = sub7;

// object - 각각의 프로퍼티가 대응하는 프로퍼티와 같거나 서브타입이어야 한다.
let sub8: {a : string; b: number} = {a:'', b:1};
let sup8: {a : string | number; b: number} = sub8;

// array - object와 동일
let sub8: Array<{a : string; b: number}> = [{a:'', b:1}];
let sup8: Array<{a : string | number; b: number}> = sub8;
```

2. 함수의 매개변수 타입만 같거나 슈퍼타입인 경우, 할당 가능 -> 반병
```tsx
class Person {}
class Developer extends Person {
    coding() {}
}

class StartupDeveloper extends Developer {
    burning() {}
}

function tellme(f:d(Developer) => Developer) {}

// Developer => Devoloper 에다가 Developer => Developer를 할당하는 경우
tellme(function dToD(d: Developer) : Developer {
return new Developer();
}

// Developer => Devoloper 에다가 Person => Developer를 할당하는 경우
tellme(function pToD(d: Person) : Developer {
return new Developer();
}

// Developer => Devoloper 에다가 StartupDeveloper => Developer를 할당하는 경우
tellme(function sToD(d: StartupDeveloper) : Developer {
return new Developer();
}
```


> strictFunctionTypes 옵션
>  - 함수를 할당할 시에 함수의 매개변수 타입이 같거나 슈퍼타입이 아닌 경우, 에러
>  - 안켜면 에러가 나지않음

<br />

### 타입 별칭
<br />

**타입 별칭(별명)**
 - interface와 비슷
- primitive, union type, tuple, function 
- 기타로 직접 작성해야하는 타입은 다른 이름 지정 
 - 만들어진 타입의 refer로 사용하는 것이지 타입을 만드는것은 아님

<br />

**Aliasing Primitive**
```tsx
type MyStringTpe = string;

const str = 'world';

let myStr: MyStringType = 'hello';
myStr = str;
```

**Aliasing Union Type**
```tsx
let person : string | number - ;
person = 'Mark';

type StringOrNumber = string | number;

let another: StringOrNumber = 0;
another = 'Anna';

/*
1. 유니온 타입은 A도 가능하고 B도 가능
2. 길게 쓰는걸 짧게 쓸수 있음
*/
```

**Aliasing Tuple**
```tsx
let person: [string, number] = ['Mark', 35];

type PersonTuple = [string, number];
let another: PersonTuple = ['Anna', 24];

/*
1. 튜플 타입에 별칭을 줘서 재활용 가능
*/
```
**Aliasing Function**
```tsx
type EatType = (food: string} => void;
```

**type vs aliasing**
 - 타입이 타입으로서의 목적이나 존재가치가 명확 : type / interface
 - 타입을 단순히 가르치거나 별칭을 주는 경우 : aliasing
