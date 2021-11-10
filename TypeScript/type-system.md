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