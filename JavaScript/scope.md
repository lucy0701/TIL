# Scope

### 목록

- [Scope란?](#scope란)
- [For문에서 비동기 함수 사용](#for문에서-비동기-함수-사용)

## Scope란?

값과 표현식이 표현되고나 참조 될 수있는 현재 실행되는 컨텍스트를 의미

- 변수 또는 함수가 스코프 내부에 있지 않다면 사용할 수 없음
- 스코프는 계층구조를 가졌기 때문에 하위 스코프는 상위 스코프에 접근할 수 있지만 반대는 불가능

### JavaScript의 스코프 종류

- 전역 범위: 스크립트 모드에서 실행되는 모든 코드의 기본 범위
- 모듈 범위: 모듈 모드에서 실행되는 코드 범위
- 함수 범위: funtion으로 생성된 범위

> JS는 기본적으로 함수 레벨의 스코프이며, ES6부터 `let`, `const`의 변수 선언을 통해 블록 레벨의 스코프 범위가 가능해짐
>
> - [참고 : 변수](./variables.md)
> - 블록범위 : `{}`

[TOP](#)

## For문에서 비동기 함수 사용

> for문에서 비동기 함수 사용시 `var`,`let`의 차이점

### var를 사용

- 실행문

  ```js
  const functionScope = () => {
    console.log('Start');

    for (var i = 0; i < 5; i++) {
      setTimeout(() => {
        console.log(i);
      }, 100);
    }

    console.log('End');
  };

  functionScope();
  ```

- 결과

  ```
  Start
  End
  5
  5
  5
  5
  5
  ```

- 이유
  - `var`로 선언된 변수는 **함수 스코프**를 가지며, 함수 내에서는 어떤 블록 안에서도 **동일한 변수를 참조**한다. for문 내에서 var로 선언된 i는 블록안에서 변수를 공유함
  - i는 for문이 반복 될 때, 새로운(업데이트 되는) 값으로 덮어쓰게 된다.
  - for문이 *모두 완료된 시점에서 setTimeout가 실행되기 때문에 setTimeout는 최종값을 참조*하게 됨

### 클로저를 사용

- 실행문

  ```js
  const closerFunction = () => {
    console.log('Start');

    for (var i = 0; i < 5; i++) {
      ((x) => {
        setTimeout(() => {
          console.log(x);
        }, 100);
      })(i);
    }

    console.log('End');
  };

  closerFunction();
  ```

- 결과

  ```
  Start
  End
  0
  1
  2
  3
  4
  ```

* 이유
  - 클로저 : 즉시 호출 함수 표현식(IIFE)을 활용하여 'x'라는 인수에 'i'값을 전달
  - 클로저는 'x'값으로 실행 시점의 'i'값을 전달 받고, 내부 함수는 'x'의 값을 참조하게 됨
  - 'x'는 실행 시점의 'i'를 캡쳐하고 있기 때문에, *독립적인 값*을 가지게 됨

> **즉시 호출 함수 표현식(IIFE)**
>
> - Immediately Invoked Function Expression
> - 정의함과 동시에 즉시 실행시키는 패턴
> - 코드의 범위를 관리하고, 변수의 스코프를 제한하며, 전역 네임스페이스를 오염시키지 않고도 특정 기능을 모듈화할 수 있음
>
> ```js
> (function () {
>   console.log('안녕하세요');
> })();
> ```

### let을 사용

- 실행문

  ```js
  const blockScope = () => {
    console.log('Start');

    for (let i = 0; i < 5; i++) {
      setTimeout(() => {
        console.log(i);
      }, 100);
    }

    console.log('End');
  };

  blockScope();
  ```

- 결과

  ```
  Start
  End
  0
  1
  2
  3
  4
  ```

- 이유
  - `let`는 블록 스코프를 가짐
  - for문이 반복 될 때마다 새로운 'i' 변수를 생성 ( let i는 블록마다 새로 선언된 것으로 간주 )
  - 각 반복에서 생성된 'i'는 `setTimeout` 콜백이 실행 될 때까지 그값을 유지함
  - 각 `setTimeout`는 해당 블럭안의 'i'를 참조하게 되므로, 순서대로 출력이 가능

[TOP](#)
