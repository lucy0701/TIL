# Declaration

> **declare** : ① 선언하다 ② 말하다 ③ 주장 ④ 선고하다

- TypeScript에는 타입을 정의할 때 사용하는 주요 방법 중 **.d.ts 파일**을 이용하는 방법과 **declare 키워드**을 사용하는 방법 두 가지가 있다.
- 이 둘은 유사한 목적을 가지고 있지만, 사용되는 상황과 용도가 다르다. 차이점을 알아보자!

### 목록

- [declare 키워드](#declare-키워드)
- [.d.ts 파일](#dts-파일)
  - [.d.ts 파일 구조](#dts-파일-구조)
  - [.d.ts 파일 내부에서 declare사용하기](#dts-파일-내부에서-declare사용하기)
  - [@type/\*](#type)
- [declare vs .d.ts file](#declare-vs-dts-file)
  - [declare](#declare)
  - [.d.ts](#dts)
  - [요약](#요약)

## declare 키워드

- TS 파일 내에서 **타입**이나 **변수**를 **전역으로 선언**할 때 사용
- TS 컴파일러에게 "이 선언은 실제로 존재하는 것처럼 취급해라"라고 알려주는 역할

- 예시

  ```ts
  declare global {
    interface Window {
      Kakao: any;
    }
  }
  ```

- 용도:
  - TypeScript 파일 내에서 타입을 정의하거나 *외부에서 제공된 **전역 변수, 함수, 모듈, 클래스 등**을 정의*할 때 사용
- 특징:
  - TS 파일 내부에서 특정 변수를 전역으로 정의하거나, 외부 JS 코드에서 제공하는 전역 객체를 선언할 때 사용
  - 실제 구현은 존재하지 않지만, 타입스크립트가 타입 검사를 할 때는 존재한다고 간주
  - 타입 추론 보조: declare를 사용하여 제공된 타입 정보는 *IDE에서 타입 추론을 보조*하고, 코드 작성 시 **타입 안전성을 보장**

[TOP](#)

## .d.ts 파일

'declare typescript'의 약자

- TypeScript에서 **타입 정의 선언만 포함된 파일**
- JS 기반의 서브 파티 모듈을 TS 개발 환경에서 사용하기 위해 JS 모듈의 타입을 정의한 파일
- 이 파일은 TS 컴파일러가 필요한 타입 정보를 읽을 수 있도록 하여, **코드 작성 시 타입 검사(타입 체킹)를 지원**

- 용도:
  - 외부 라이브러리(예 : JS기반의 라이브러리)나 모듈에 대한 타입을 정의를 분리하여 관리할 때 사용
- 특징:
  - 순수한 타입 정의만 포함, _실행 코드는 포함되지 않음_
  - 자동으로 프로젝트 내에서 사용될 수 있으며, 이를 통해 IDE에서 타입 완성을 지원받을 수 있음
  - 모듈 외부에서 제공되는 타입을 정의하는 경우 (Ex: Node.js 모듈이나 외부 라이브러리)를 위해 사용

### .d.ts 파일 구조

- .d.ts 파일은 실행 코드 없이 타입 선언만 포함
  > 구현을 정의하지 않은 선언을"ambient"라고 함. 일반적으로 .d.ts 파일 내부에 정의 되어 있음
- TypeScript 컴파일러는 이 파일에서 제공하는 타입 정보를 사용하여 타입 체크 및 자동 완성 등을 지원

  - 모듈 타입 정의: **외부 라이브러리**나 **모듈**의 타입을 정의할 때 사용
  - 전역 타입 정의: **전역 스코프**에서 사용되는 타입을 정의할 때 사용

### .d.ts 파일 내부에서 declare사용하기

1. `.d.ts`파일에서 모듈을 선언할 때 `declare module`을 사용

- TypeScript 컴파일러가 해당 모듈의 타입 정보를 알 수 있도록 함

  ```ts
  // myLibrary.d.ts
  declare module 'myLibrary' {
    export function myFunction(param: string): number;
    export interface MyInterface {
      name: string;
      age: number;
    }
  }
  ```

2. 전역 변수 및 함수 선언

- 전역 변수나 전역 함수가 존재할 때, .d.ts 파일 내에서 declare를 사용하여 이를 선언
  자
  ```ts
  // globals.d.ts
  declare const API_BASE_URL: string;
  declare function fetchData(url: string): Promise<any>;
  ```

3. 전역 타입 선언

- 전역 공간에 있는 타입(예: 기본 타입, 전역 인터페이스)을 선언할 때도 declare를 사용

  ```ts
  // globalTypes.d.ts
  declare global {
    interface Window {
      myCustomProperty: string;
    }
  }
  ```

### @type/\*

- TS로 개발할 때, npm으로 `@type/`을 함께 설치할 때가 있다.

  ```
  npm i -D @types/*
  ```

  > Type 검사는 개발 환경에서만 지원하기 때문에(런타임은 JS기반) `-D`로 설치 함!

- 해당 모듈들은 JS 기반으로 만들어진 라이브러리의 Type 선언을 모아 둔 `d.ts` 모듈 파일이다.
- 만약 해당 라이브러리의 타입 일부만 사용하거나, 커스텀 된 타입을 선언해서 사용하고 싶다면 직접 `d.ts`파일을 생성하면 됨

  1. 프로젝트 내부에 @type 폴더 생성
  2. 모듈 명 폴더 추가
  3. index.d.ts 파일 생성
  4. 타입 선언
  5. tsconfig.json파일 내부에 compilerOptions에 typeRoots 속성을 추가

  - typeRoots를 설정하지 않으면 기본적으로 node_modules/@types 디렉토리를 검색
  - typeRoots를 설정하게 되면, 해당 파일 내부만 타입 검사를 실행하기 때문에 typeRoots 설정에 커스텀 파일 경로와 `node_modules/@types` 디렉토리도 추가해 줘야 함

    ```json
    {
      "compilerOptions": {
        "typeRoots": ["./types", "./node_modules/@types"]
      }
    }
    ```

> tsconfig에서 `declaration` 옵션을 `true`로 설정하면 TS 파일을 JS로 컴파일할 때, 자동으로 JS 파일과 d.ts 파일을 생성해 줌

[TOP](#)

## declare vs .d.ts file

### declare

- 파일 내부 선언이 필요한 곳에 직접 `declare`를 사용할 수 있다.
- 사용 예시
  - **일시적 사용** : 라이브러리를 잠깐 사용하거나, 간단한 테스트가 필요해 타입 선언이 필요할 때
  - **간단한 전역 변수/함수**: 전역적으로 사용될, 변수나 함수가 간단하거나 몇 개 안 될 때 (소규모 프로젝트)

### .d.ts

- 외부 라이브러리나 모듈을 사용할 경우, 타입 선언을 별도의 `.d.ts`파일을 분리하여 사용
- 사용 예시
  - **외부 라이브러리 사용**: JS로 작성된 외부 라이브러리의 타입을 선언하거나, 타입의 정의가 제공되지 않은 라이브러리의 타입들을 정의할 때 `.d.ts`파일 작성
  - **재사용성과 유지보수**: 여러 파일에서 반복적으로 사용되는 타입이나 모듈의 경우, `d.ts` 파일로 분리하여 사용
  - **타입 패키지 작성**: npm에 배포할 라이브러리의 타입을 정의하거나, 프로젝트 여러 곳에서 사용될 수 있는 패키지를 작성할 때

### 요약

- **`declare`**: TS에서 타입 정보를 제공할 때 사용하는 키워드. 타입만을 정의하고, 존재하는 전역 변수, 함수, 클래스, 모듈 등을 타입스크립에 알림
- **`.d.ts`**: TS 프로젝트에서 타입 정보를 정의하는 데 사용. 실제 실행 코드를 포함 X, 타입 정의만 포함하여 TS 타입 검사를 지원함

> 즉, declare은 해당 타입이 있음을 TS 컴파일러에 알리고, .d.ts는 프로젝트에서 사용될 라이브러리나 전역 타입에 대한 정의들을 모아둔 파일을 말함

[TOP](#)
