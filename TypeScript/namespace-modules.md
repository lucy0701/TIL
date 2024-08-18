# Namespace and Modules

### 목록

- [namespace](#namespace)
- [네임스페이스의 주요 목적](#네임스페이스의-주요-목적)
- [Namespace vs Modules](#namespace-vs-modules)
  - [네임스페이스를 사용하는 경우](#네임스페이스를-사용하는-경우)
  - [네임스페이스 대신 모듈을 사용하는 경우](#네임스페이스-대신-모듈을-사용하는-경우)
  - [네임스페이스와 모듈의 차이점](#네임스페이스와-모듈의-차이점)
  - [import 경로에 위치한 모듈과 컴파일러](#import-경로에-위치한-모듈과-컴파일러)
  - [Needless Namespace](#needless-namespace)

## namespace

- 구 "internal modules"
  - internal modules -> namespace
- 네임스페이스는 주로 코드에서 이름 충돌을 방지하고, 코드를 논리적으로 그룹화 하기위해 사용됨
- 하지만 TS의 모듈 시스템을 사용하게 되면서, 네임스페이스의 필요성이 줄어들게 됨

## 네임스페이스의 주요 목적

1. 이름 충돌 방지:

- 큰 프로젝트에서 여러 파일에서 동일한 이름의 클래스, 함수, 변수 등의 정의할 수 있음
- 네임스페이스를 사용하여 동일한 이름을 네임스페이스 내부에서 격리할 수 있어 충돌을 방지할 수 있음

  ```ts
  namespace Geometry {
    export class Square {
      /* ... */
    }
  }

  namespace Art {
    export class Square {
      /* ... */
    }
  }

  let geomSquare = new Geometry.Square();
  let artSquare = new Art.Square();
  ```

  - Geometry.Square와 Art.Square는 각각 다른 네임스페이스에 정의된 동일한 이름의 클래스이지만, 서로 충돌하지 않고 독립적으로 사용될 수 있다.

2. 코드의 논리적 그룹화:

- 관련된 기능들을 한데 묶어서 관리할 수 있음. 예들들어 여러 유틸리티 함수나 관련된 클래스를 하나의 네임스페이스 안에 모아두면, 코드 구조를 더 명확하게 만들 수 있음

  ```ts
  namespace Utils {
    export function log(message: string) {
      console.log(message);
    }

    export function error(message: string) {
      console.error(message);
    }
  }

  Utils.log('This is a log message');
  Utils.error('This is an error message');
  ```

## Namespace vs Modules

### 네임스페이스를 사용하는 경우

1. 전역 스코프를 사용하는 라이브러리:

- 브라우저에서 실행되는 전역 스크립트나, 특정한 환경에서 다른 모듈 시스템을 사용하지 않은 경우에 유용
- 여러 스크립트 파일이 같은 전역 공간에서 작동할 때, 네임스페이스를 통해 전역 변수를 보호하고 이름 충돌을 방지할 수 있음

2. 모듈 시스템을 사용하지 않는 코드 베이스:

- 오래된 코드베이스나, CommonJS, AMD와 같은 모듈 시스템을 사용하지 않는 프로젝트에서는 네임스페이스가 유용할 수 있음

3. 큰 규모의 프로젝트에서 내부적으로 논리적 그룹이 필요한 경우:

- 대규모 프로젝트에서 내부적으로 관련된 코드들을 그룹화하고, 이 그룹을 외부에 노출시키지 않을 때, 네임스페이스를 사용
- 하지만 이 경우에도 모듈 시스템이 더 좋을 수 있음

### 네임스페이스 대신 모듈을 사용하는 경우

- 모듈 시스템을 사용하는 프로젝트

  - ES6 모듈이나 TS 모듈 시스템이 있는 경우, 네임스페이스 대신 모듈을 사용하는 것을 권장
  - 모듈 시스템에서는 파일 단위로 코드가 격리되기 때문에, 네임스페이스를 쓸 필요가 거의 없음

- 파일 간의 코드 공유
  - 모듈 시스템을 사용하면 `import`와 `export`를 통해 파일간에 필요한 코드만 공유할 수 있으므로, 네임스페이스의 역할을 대처할 수 있음

### 네임스페이스와 모듈의 차이점

1. 네임스페이스:

- 네임스페이스는 같은 파일 또는 여러 파일에서 코드를 논리적으로 그룹화하고, 이름을 충돌을 방지하기 위해 사용
- 여러 클래스나 함수가 같은 이름을 가질 수 없기 떄문에, 네임스페이스 안에 묶어서 사용

2. 모듈:

- 모듈은 파일단위로 코드를 그룹화 하고 내보내기(export) 및 가져오기(import)할 수 있게 해줌
- 모듈을 사용하면, 각 파일이 자체적으로 독립된 스코프를 가지기 때문에, 네임스페이스를 사용할 필요가 없음

[TOP](#)

### import 경로에 위치한 모듈과 컴파일러

- 컴퍼일러는 .ts, .tsx를 찾은 다음 적절한 경로에 위치한 .d.ts를 찾음
- 만약 특정한 파일을 찾지 못한다면, 컴파일러는 *앰비언트 모듈(ambient module) 선언*을 탐색함

- myModules.d.ts

  ```ts
  // 모듈이 아닌 .d.ts 파일 또는 .ts 파일:
  declare module 'SomeModule' {
    export function fn(): string;
  }
  ```

- myOtherModule.ts

  ```ts
  /// <reference path="myModules.d.ts" />
  import * as m from 'SomeModule';
  ```

  - 위의 reference 태그는 앰비언트 모듈(ambient module) 선언이 포함된 선언 파일의 위치를 지정하는 데 필요

[TOP](#)

### Needless Namespace

- 네임스페이스를 사용하던 프로그램을 모듈로 변경하면, 파일은 다음과 같은 모습이 되기 쉽다.
  - shapes.ts
    ```ts
    export namespace Shapes {
      export class Triangle {
        /* ... */
      }
      export class Square {
        /* ... */
      }
    }
    ```
    - 최상위 모듈 `Shapes`는 아무런 의미 없이 Triangle과 `Square`을 감쌈
  - shapeConsumer.ts
    ```js
    import * as shapes from './shapes';
    let t = new shapes.Shapes.Triangle(); // shapes.Shapes?
    ```
- TypeScript 모듈의 중요한 특징 중 하나는 서로 다른 두 개의 모듈이 절대 같은 스코프 안에 이름을 제공하지 않는다는 점이다. 모듈 사용자가 어떤이름을 할당할지를 결정하기 떄문에, 네임스페이스 내부에서 내보내는 심벌을 미리 감싸줄 필요가 없다.
- 네임스페이스를 설정하지 않아도 되는 이유는, 네임스페이스를 지정하는 일반적인 목적은 구조와 논리적 그룹을 제공하고 이름 충돌을 방지하기 위함인데, 모듈 파일이 이미 스스로 논리적 그룹을 형성하고 있기 때문에, 최상위 이름은 이를 가져오는 코드에 의해 정의되고, 내보내는 객체를 위한 추가적인 모듈 계층을 사용할 필요가 없다

* 수정된 예
  - shapes.ts
    ```ts
    export class Triangle {
      /* ... */
    }
    export class Square {
      /* ... */
    }
    ```
    - 최상위 모듈 `Shapes`는 아무런 의미 없이 Triangle과 `Square`을 감쌈
  - shapeConsumer.ts
    ```js
    import * as shapes from './shapes';
    let t = new shapes.Triangle();
    ```

[TOP](#)
