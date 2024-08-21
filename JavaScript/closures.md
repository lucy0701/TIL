# Closure

### 목록

- [Closure란?](#closure란)
- [클로저 활용](#클로저-활용)
  - [private method (캡슐화)](#private-method-캡슐화)
  - [커링 (Currying)](#커링-currying)
- [클로저 스코프 체인](#클로저-스코프-체인)
- [루프에서 클로저 생성하기](#루프에서-클로저-생성하기)

## Closure란?

- 생명주기가 끝난 외부함수의 변수를 참조하고 있는 내부 함수
- 외부 함수의 생명주기가 끝나더라도 내부함수는 스코프 체이닝을 통해 외부함수의 스코프를 참조 할수 있다
- 내부 변수는 내부함수가 참조하고 있기 때문에 가비지 컬렉션에서 제외 됨

```js
function makeFunc() {
  const name = 'Mozilla';
  function displayName() {
    console.log(name);
  }
  return displayName;
}

const myFunc = makeFunc();
myFunc();
```

- `myFunc`은 `makeFunc`이 실행 될 때 생성된 `displayName` 함수의 인스턴스에 대한 참조
- `displayName`의 인스턴스는 `makeFunc`의 변수 `name`을 참조하게 됨
- `myFunc`가 호출될 때 `makeFunc`는 name를 참조고 있기 때문에,"Mozilla"가 `console.log`에 전달 됨

[TOP](#)

## 클로저 활용

### private method (캡슐화)

- 캡슐화는 객체의 내부 상태(변수)와 동작(메서드)를 외부로부터 숨기고, 특정한 메서드를 통해서만 접근할 수 있게 하는 객체지향 프로그래밍의 원칙이다.
- 클로저를 이용하여 변수와 함수의 접근 범위를 제한하는 것이 가능하며, 이를 통해 정보 은닉이 가능함

```js
const makeCounter = function () {
  let privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment() {
      changeBy(1);
    },

    decrement() {
      changeBy(-1);
    },

    value() {
      return privateCounter;
    },
  };
};

const counter1 = makeCounter();
const counter2 = makeCounter();

console.log(counter1.value()); // 0.

counter1.increment();
counter1.increment();
console.log(counter1.value()); // 2.

counter1.decrement();
console.log(counter1.value()); // 1.
console.log(counter2.value()); // 0.
```

- 비공개 함수와 변수에 접근하는 함수를 정의하기 위해 클로저를 사용하는 방법이다.
- 이렇게 클로저를 사용하는 것을 **모듈 디자인 패턴**이라고 함

  > **모듈**
  >
  > - 전체 애플리케이션 중 일부를 독립된 코드로 분리해서 만들어 놓은 것
  >
  > **모듈 디자인 패턴**
  >
  > - 모듈 패턴은 관련된 변수와 함수를 모아 **즉시 실행 함수(IIFE)**로 감싸 하나의 모듈을 만들어 사용하는 방법

- `makeCounter` 함수는 `privateCounter`라는 변수와 `changeBy`라는 함수를 내부에 정의
- **`privateCounter`와 `changeBy`**는 함수 내부에서만 접근할 수 있는 비공개(private) 상태로 남게 됨
- `increment`, `decrement`, `value` 메서드는 클로저로서 `privateCounter`에 접근할 수 있으며, 외부에서 직접적으로 `privateCounter`를 수정하거나 접근할 방법은 없다

### 커링 (Currying)

**커링(Currying)** 은 여러 인수를 한 번에 받는 함수를 여러 단계로 나누어 각 단계에서 하나의 인수만을 받도록 변환하는 과정이다.

즉, 커링된 함수는 한 번에 하나의 인수만 받으며, 나머지 인수를 받기 위해 새 함수를 반환한다.

> 커링은 함수를 호출하지 않는다. 변환만 함

```js
function curry(f) {
  // 커링 변환을 하는 curry(f) 함수
  return function (a) {
    return function (b) {
      return f(a, b);
    };
  };
}

// usage
function sum(a, b) {
  return a + b;
}

let curriedSum = curry(sum);
console.log(curriedSum(1)(2)); // 3

let addFive = curriedSum(5);
console.log(addFive(1)); // 6
```

- `curry(f)` 함수:

  - `curry(f)`는 원래 함수 f를 커링된 형태로 변환함
    변환된 함수는 여러 인수를 받아야 할 때, 각 인수를 별도의 함수 호출로 나눠서 받을 수 있도록 한다

- `curriedSum(1)` 호출:

  - 이 호출은 첫 번째 인수 1을 받아 저장하고, 두 번째 인수를 기다리는 새로운 함수를 반환함

  - 이 함수는 여전히 sum 함수에 대한 접근 권한을 가지고 있으며, 첫 번째 인수를 기억한다.

- `curriedSum(1)(2)` 실행:

  - 두 번째 인수 2를 받은 후, sum(1, 2)를 실행하여 결과를 반환

- `addFive` 함수:

  - `addFive`는 `curriedSum(5)`의 결과로, 첫 번째 인수를 5로 고정한 새로운 함수다

  - 이 함수는 이제 두 번째 인수만 받으면 되며, 호출 시 고정된 5와 새로운 인수를 합산한 결과를 반환함

[TOP](#)

## 클로저 스코프 체인

- 클로저의 세가지 스코프(범위):

  - 지역 범위 (Local Scope, Own scope)
  - 포함하고 있는 범위 (블록, 함수 또는 모듈 범위일 수 있음)
  - 전역 범위 (Global Scope)

[TOP](#)

## 루프에서 클로저 생성하기

```js
function showHelp(help) {
  document.getElementById('help').textContent = help;
}

function setupHelp() {
  var helpText = [
    { id: 'email', help: 'Your e-mail address' },
    { id: 'name', help: 'Your full name' },
    { id: 'age', help: 'Your age (you must be over 16)' },
  ];

  for (var i = 0; i < helpText.length; i++) {
    // 범인은 이 줄에서 `var`를 사용하는 것입니다.
    var item = helpText[i];
    document.getElementById(item.id).onfocus = function () {
      showHelp(item.help);
    };
  }
}

setupHelp();
```

- 이 코드를 사용하면 제대로 동작하지 않는다. 어떤 필드에 포커스를 주더라도 나이에 관한 도움말이 표시됨
- 그 이유는 `onfocus` 이벤트에 연결된 함수가 클로저이기 때문
- 반복문에서 3개의 클로저가 만들어졌지만, 각 클로저가 사용하는 변수인 `item`이 `var`로 선언 되었기 때문에 같은 값을 공유한다.
  - 변수 `item`이 `var`로 선언되어 [호이스팅(hoisting)](hoisting.md)으로 인해 함수 범위 스코프이기 때문
  - `item.help`의 값은 `onfocus` 콜백이 실행 될때 결정이 되므로, 반복문은 모두 실행이 완료 되어 `item` 변수 객체는 마지막 항목을 가르키고 있음

### 해결 방법

1. 함수 팩토리 사용

   ```js
   function showHelp(help) {
     document.getElementById('help').innerHTML = help;
   }

   function makeHelpCallback(help) {
     return function () {
       showHelp(help);
     };
   }

   function setupHelp() {
     var helpText = [
       { id: 'email', help: 'Your e-mail address' },
       { id: 'name', help: 'Your full name' },
       { id: 'age', help: 'Your age (you must be over 16)' },
     ];

     for (var i = 0; i < helpText.length; i++) {
       var item = helpText[i];
       document.getElementById(item.id).onfocus = makeHelpCallback(item.help);
     }
   }

   setupHelp();
   ```

- `makeHelpCallback`는 각 콜백에 새로운 환경을 생성한다

2. 익명 클로저를 활용

   ```js
   function showHelp(help) {
     document.getElementById('help').textContent = help;
   }

   function setupHelp() {
     var helpText = [
       { id: 'email', help: 'Your e-mail address' },
       { id: 'name', help: 'Your full name' },
       { id: 'age', help: 'Your age (you must be over 16)' },
     ];

     for (var i = 0; i < helpText.length; i++) {
       (function () {
         document.getElementById(item.id).onfocus = function () {
           showHelp(item.help);
         };
       });
     }
   }

   setupHelp();
   ```

> [Scope 참조](scope.md)

### 클로저를 사용하지 않는 방법

1. `let`, `const` 활용

   ```js
   function showHelp(help) {
     document.getElementById('help').textContent = help;
   }

   function setupHelp() {
     const helpText = [
       { id: 'email', help: 'Your email address' },
       { id: 'name', help: 'Your full name' },
       { id: 'age', help: 'Your age (you must be over 16)' },
     ];

     for (let i = 0; i < helpText.length; i++) {
       const item = helpText[i];
       document.getElementById(item.id).onfocus = () => {
         showHelp(item.help);
       };
     }
   }

   setupHelp();
   ```

- 위의 경우 `var` 대신 `const`을 사용하여 모든 클로저가 블록 범위 변수를 바인딩할 것이므로 추가적인 클로저가 필요하지 않음

2. `forEach()` 사용

   ```js
   function showHelp(help) {
     document.getElementById('help').textContent = help;
   }

   function setupHelp() {
     var helpText = [
       { id: 'email', help: 'Your e-mail address' },
       { id: 'name', help: 'Your full name' },
       { id: 'age', help: 'Your age (you must be over 16)' },
     ];

     helpText.forEach(function (text) {
       document.getElementById(text.id).onfocus = function () {
         showHelp(text.help);
       };
     });
   }

   setupHelp();
   ```

- forEach()는 for문과 비슷하지만, 내부적으로 콜백 함수를 각 배열요소에 실행할 때, 콜백 함수는 독립적인 실행 컨텍스트(스코프)를 가짐

[TOP](#)
