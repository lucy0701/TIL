# Async-Await

### 목록

- [async와 await](#async와-await)
- [Error handling](#error-handling)

## async와 await

### Async functions

`async` keyword는 함수 앞에 작성

- `async`를 작성하면, 해당 함수는 비동기로 작업 됨을 의미
- 반드시 Promise를 반환하며, 프로미스가 아닌것은 프로미스로 감싸 반환

  ```js
  async function f() {
    return 1;
  }
  ```

### Await

```js
const value = await promise;
```

- `async` 키워드를 작성한 함수 내부에서 `await` 키워드 사용 가능 (`await`은 `async` 함수 안에서만 동작 함)

  ```js
  async function f() {
  let promise = new Promise((resolve, reject) = {
      setTimeout(() = resolve('완료!'), 1000);
  });

  let result = await promise; // 프라미스가 이행될 때까지 기다림 (*)

  alert(result); // "완료!"
  }

  f();
  ```

- 코드가 실행 되고 `await`키워드를 만나면 실행이 잠시 '중단'되었다가 프로미스가 처리되면 실행이 재개 됨

* #### `async`는 최상위 레벨 코드에서 작동하지 않음

  - 익명 `async` 함수로 코드를 감싸면 최상위 레벨 코드에도 `await` 사용 가능

    ```js
    (async () = {
    let response = await fetch('/article/promise-chaining/user.json');
    let user = await response.json();
    ...
    })();
    ```

* #### `await`는 'thenable' 객체를 받음

  - promise.then처럼 await에도 thenable 객체(then 메서드가 있는 호출 가능한 객체)를 사용할 수 있음
  - thenable 객체는 서드파티 객체가 프라미스가 아니지만 프라미스와 호환 가능한 객체를 제공할 수 있다는 점에서 생긴 기능
  - 서드파티에서 받은 객체가 .then을 지원하면 이 객체를 await와 함께 사용할 수 있다
  - _await는 데모용 클래스 Thenable의 인스턴스를 받을 수 있다._

    ```js
    class Thenable {
    constructor(num) {
        this.num = num;
    }
    then(resolve, reject) {
        alert(resolve);
        // 1000밀리초 후에 이행됨(result는 this.num*2)
        setTimeout(() = resolve(this.num * 2), 1000); // (*)
    }
    }

    async function f() {
    // 1초 후, 변수 result는 2가 됨
    let result = await new Thenable(1);
    alert(result);
    }

    f();
    ```

    1.  `await`은 `.then`이 구현되어있으면서 프라미스가 아닌 객체를 받으면, 내장 함수 resolve와 reject를 인수로 제공하는 메서드인 `.then`을 호출 함
    2.  `await`는 **resolve**와 **reject** 중 하나가 호출되길 기다렸다가((\*)로 표시한 줄) 호출 결과를 가지고 다음 일을 진행

[TOP](#)

## Error handling

promise가 정삭적으로 해결되면 결과값을 반환하고, 거부될 경우 throw문을 작성한 것처럼 error를 반환 한다

```js
async function f() {
  await Promise.reject(new Error('Whoops!'));
}
```

```js
async function f() {
  throw new Error('Whoops!');
}
```

- 프로미스가 거부 되기 전에 약간의 시간이 지체되는 경우, await가 에러를 던지기 전 지연이 발생
- await가 던진 에러는 throw가 던진 에러를 잡을 때처럼 try...catch를 사용해 잡을 수 있다

  ```js
  async function f() {
    try {
      let response = await fetch('/no-user-here');
      let user = await response.json();
    } catch (err) {
      // catches errors both in fetch and response.json
      alert(err);
    }
  }

  f();
  ```

  - try...catch를 사용하지 않을경우, .catch를 추가하여 거부된 프로미스를 처리할 수 있음

  ```js
  async function f() {
    let response = await fetch('http://no-such-url');
  }

  // f() becomes a rejected promise
  f().catch(alert); // TypeError: failed to fetch // (*)
  ```

[TOP](#)
