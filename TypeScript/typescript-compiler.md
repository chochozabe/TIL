# TypeScript Compiler

**Compilation Context**
 - 컴파일 할 파일이나 컴포넌트의 모음
 - 어떠한 컴파일러 옵션이 적용되고 있는지에 대한 정보를 포함
 - 이러한 logical grouping을 정의하는 것이 바로 tsconfig.json 파일

<br />

**tsconfig schema**
 - 최상위 프로퍼티
  - compileOnSave
  - extends
  - compileOptions
  - file
  - include
  - exculde
  - references

<br />

**compileOnSave**
  - true / false (default false)
  - 컴파일을 다시 시켜주는 
  -  Visul Studio 2015 with TypeScript 1.8.4 이상
  - atom-typescript 플러그인
  - schema
```tsx
{
  ...,
  "compileOnSaveDefinition" : {
    "properties" : {
      "compileOnSave" : {
        "description": "Enable Compile-on-Save for this project.",
        "type": "boolean"
      }
    }
  },
  ...,
}
```
- 사용법
```tsx
/// tsconfig
{
  "compileOnSave" : true,
}
```

<br />

**extends**
 - 상속 옵션
 - 파일(상대) 경로명 : string
 - TypeScript 2.1 New Options
 - schema
```tsx
{
  ...,
  "extendsDefinition" : {
    "properties" : {
      "extends" : {
        "description": "Path to base configuration file to inherit from/ Requires TypeScript version 2.1 or later.",
        "type": "string"
      }
    }
  },
  ...,
}
```
- 사용법
```tsx
{
  "extends" : "./base.json" // 상속할 파일이 존재해야함
}

// base.json : 같은 구조
{
  "compileOnSave" : true,
}
```

> tsconfig 공식에서 제공하는 tsconfig 설정 파일이 많아 extends해서 사용하면 유용<br/>
> npm install --save-dev @tsconfig/deno<br/>
> { "extends": "@tsconfig/deno/tsconfig.json" }

<br />

***files, include, exculde***
 - 셋 다 설정이 없으면 모든 파일을 컴파일
- files
  - 상대 혹은 절대 경로의 리스트 배열
  - exclude보다 쎔
- include, exclude
  - glob 패턴 (ex .gitignore)
  - include
    - exclude 보다 약함
    - `*` 같은 걸 사용하면, .ts/.tsx/.d.ts만 include (allowJS)
  - exclude
    - 설정 안하면 4가지(node_modules, bower_components, jspm_packages, `<outDir>`) 를 default로 제외
    - `<outDir>`은 include에 있어도 항상 제외
    - 어떤 디렉토리 안에 어떤 파일을 컴파일할지 정할 수 있음
- schema
```tsx
{
  ...,
  "filesDefinition": {
      "properties": {
        "files": {
          "description": "If no 'files' or 'include' property is present in a tsconfig.json, the compiler defaults to including all files in the containing directory and subdirectories except those specified by 'exclude'. When a 'files' property is specified, only those files and those specified by 'include' are included.",
          "type": "array",
          "uniqueItems": true,
          "items": {
            "type": "string"
          }
        }
      }
    },
    "excludeDefinition": {
      "properties": {
        "exclude": {
          "description": "Specifies a list of files to be excluded from compilation. The 'exclude' property only affects the files included via the 'include' property and not the 'files' property. Glob patterns require TypeScript version 2.0 or later.",
          "type": "array",
          "uniqueItems": true,
          "items": {
            "type": "string"
          }
        }
      }
    },
    "includeDefinition": {
      "properties": {
        "include": {
          "description": "Specifies a list of glob patterns that match files to be included in compilation. If no 'files' or 'include' property is present in a tsconfig.json, the compiler defaults to including all files in the containing directory and subdirectories except those specified by 'exclude'. Requires TypeScript version 2.0 or later.",
          "type": "array",
          "uniqueItems": true,
          "items": {
            "type": "string"
          }
        }
      }
    },
...,
}
```