# TypeScript

**TypeScript란 무엇인가**

<br />

Typed JavaScript at any Scale
 - 타입스크립트는 타입을 추가해서 자바스크립트를 확장시킨다.
 - 코드를 실행하기 전에 에러를 잡거나 고치는 데 쓰는 시간을 절약해준다.
 - 어떠한 자바스크립트 실행환경에서 가능하다. 완전히 오픈소스이다.

TypeSciprt = Language
- 자바스크립트의 기능을 강화시키고 타입이란 개념까지 추가
- 컴파일의 과정을 통해서 순수한 자바스크립트로 변경
- 정리
    * 타입스크립트는 'Programming Language' 
    * 타입스크립트는 'Compiled Language'
    * 전통적인 Compiled Language랑 다른 점이 많아 'Transpile'라는 용어를 사용하기도 함
    * 자바스크립트는 Interpreted Language

Compile과 Interpreted의 차이
- 코드를 바로 컴파일해서 런타임에 오류를 발견하는 Interpreted Language와 달리 Compiled Language는 컴파일을 하는 시점(컴파일 타임)에 오류를 발견하여 실행하기 전에 오류를 수정할 수 있다.

* TS는 런타임 환경에서 바로 읽어서 사용할 수 없다<br /> 
-> TypeScript Compile를 이용해 plain JavaScript로 변경해서 사용해야한다.

<br />

**TypeScript 설치 및 사용**
<br />

설치
```js
// typescript는 런타임에서 사용하지 않기때문에 일반적으로 devDependency로 설치
npm i typescript -D
```

Compiler 사용법
```js
// typescript가 global로 설치되었을 때
tsc [파일명] 

// 프로젝트 안에서 사용할 때
{tsc가 있는 경로}/tsc

// 단, scripts 안에서는 경로 생략하고 tsc로 사용가능
```

Compiler 설정파일 생성 (tsconfig.json)
```js
tsc --init
```

<br />

**Type Annotation**
```ts
let a = "Mark";

a = 39; // Error
```
변수를 처음에 string 타입으로 초기화했기때문에 number 타입의 값을 넣으려고 할 때 에러발생
```ts
let a: number;

a = "Mark"; // Error

a = 39; 
```
변수를 정의할때 type annotation으로 number라고 타입을 지정해줬기때문에 string 타입의 값을 넣을때 에러 발생