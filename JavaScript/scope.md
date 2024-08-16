# Scope

### 목록

- [Scope란?](#scope란)
- [Scope chain](#scope-chain)
- [For문에서 비동기 함수 사용](#for문에서-비동기-함수-사용)

## Scope란?

변수에 접근할 수있는 범위. 현재 실행되는 컨텍스트내에서 변수에 접근할 수 있는지 결정

- 변수 또는 함수가 스코프 내부에 있지 않다면 사용할 수 없음
- 스코프는 계층구조를 가졌기 때문에 하위 스코프는 상위 스코프에 접근할 수 있지만 반대는 불가능
  > 외부 함수가 내부함수의 변수를 참조하기 위해서는 [클로저](./closures.md)를 활용 한다

### JavaScript의 스코프 종류

- 전역 범위: 스크립트 모드에서 실행되는 모든 코드의 기본 범위
- 모듈 범위: 모듈 모드에서 실행되는 코드 범위
- 함수 범위: funtion으로 생성된 범위

> JS는 기본적으로 함수 레벨의 스코프이며, ES6부터 `let`, `const`의 [변수 선언](./variables.md)을 통해 블록 레벨의 스코프 범위가 가능해짐
>
> - 블록범위 : `{}`

[TOP](#)

## Scope chain

- 함수 실행 시 변수의 유효 범위를 결정하는 메커니즘
- 코프 체인은 현재 함수의 스코프(범위)와 그 함수가 호출된 위치의 상위 스코프를 모두 포함하여, 변수를 찾는 계층적 구조를 형성 함

> [실행 컨텍스트](./execution-context.md)를 참조

### Lexical scoping

- 자바 스크립트는 렉시컬 스코프(Lexical Scope) 원칙을 따름
- 정적 스코프(Static Scope)라고도 함
- 렉시컬 스코프란 함수를 호출한 곳이 아닌 **선언한 곳을 기준**으로 스코프를 결정하는 원칙을 말함
  - 함수 호출 시점이 아니라 함수 정의 시점의 스코프가 적용 된다.

```js
function init() {
  var name = 'Mozilla';
  function displayName() {
    console.log(name);
  }
  displayName();
}
init();
```

- name는 init의 지역 변수`name`와 함수`displayName()`를 생성
- `displayName()`은 init의 내부 변수인 `name`에 접근 가능

### 스코프 체인의 동작

1. 현재 스코프:
   함수가 실행될 때, 자바스크립트 엔진은 현재 함수의 스코프를 확인하여 그 범위에서 변수를 찾음

2. 상위 스코프 탐색:
   현재 스코프에서 변수를 찾을 수 없으면, 엔진은 **상위 스코프를 탐색** 함
   이 과정은 스코프 체인을 통해 진행되며, 최상위 스코프인 **전역 스코프까지 계속**된다

3. 변수의 접근:
   변수는 자신이 선언된 스코프와 그 상위 스코프에서 접근할 수 있음
   최상위 스코프까지 가더라도 변수나 함수가 존재하지 않으면, ReferenceError가 발생

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
