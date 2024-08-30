# JavaScript Module

## Module

프로그램 시스템을 기능 단위로 분리한 것.
단순히 분리시키는 것이 아닌, 각 기능이 분리되어 재사용 및 독립적인 컴파일이 가능해야 함.
모듈의 형태는 객체, 함수, 메서드, 추상화된 자료등 다양하다.

### 장점

- 유지보수
  - 각 기능별 모듈화를 통해, 코드의 의존성이 낮아지고 해당 기능에 대한 유지보수가 가능하다.
- 네임스페이스 화
  - 모듈은 내부에서 각 모듈만의 영역을 갖기 때문에 중복 변수명으로 부터 자유로워 진다.
- 재사용성
  - 독립된 기능을 통해, 해당 기능을 재사용하기 용이 함.

## 모듈 시스템

JS 프로그램을 모듈로 개발하고 배포할 수 있음

### CJS (CommonJS)

'Common'이름 답게 범용적으로 사용하기 위해 만들어짐
JS를 브라우저 외에서도 사용 가능하도록 하는 워킹 그룹

#### 특징

- 동기적 작동
- `exports` 내보내기와 `require` 가져오기 사용

```js
// add.js
function add(a, b) {
  return a + b;
}
module.exports = add;

// index.js
const add = require('./add');
add(1, 3);
```

### AMD (Asynchronous Module Definition)

CommonJS를 클라이언트 사이드에서 사용하기 위함(서버 사이드에서도 가능)

#### 특징

- 비동기적 작동
- 브라우저에서 실행될 독립적인 그룹 형성 (모듈)
- `define`, `require` 사용

> JS는 파일 스코프가 존재 X, define()함수로 모듈 내부에서 사용하는 변수와 전역 변수를 분리
> define()함수는 네임스페이스 역할을 함

`define(id?, dependencies?, factory);`

- id -모듈 식별자. id가 존재하지 않을 경우 로더가 요청하는 `<script>` 태그의 src값을 기본 id로 설정
- dependencies
  - 기본값 : ["require", "exports", "module"]
  - 모듈의 의존석을 나타내는 배열, 반드시 먼저 로드 되어야하는 모듈
  - 콜백 함수인 factory로 전달
- factory
  - 모듈이나 객체를 인스턴스화 하기위해 실행해야하는 콜백 함수
  - return의 값이 있을 경우 exports 객체의 속성값으로 할당
  - factory가 객체인 경우 exports 객체의 속성값으로 할당

```js
define('alpha', ['require', 'exports', 'beta'], function (
  require,
  exports,
  beta,
) {
  // id가 'alpha'인 모듈이 beta라는 모듈 사용한다고 정의
  exports.verb = function () {
    return beta.verb();
    //Or:
    return require('beta').verb();
  };
});

// 객체 반환하는 익명 모듈
// 모듈이 정의된 파일의 경로가 자동으로 식별자로 지정됨
define(['alpha'], function (alpha) {
  return {
    verb: function () {
      return alpha.verb() + 2;
    },
  };
});

// 의존성 없는 익명 모듈
define({
  add: function (x, y) {
    return x + y;
  },
});

// commonJS 래핑을 사용한 모듈
define(function (require, exports, module) {
  var a = require('a'),
    b = require('b');

  exports.action = function () {};
});
```

### UMD(Universal Module Definition)

- AMD, CommonJS 호환을 위한 패턴
- AMD와 CommonJS, Window에 추가하는 방식까지 가능한 구현 패턴

```js
(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
    // AMD
    define(['exports', 'b'], factory);
  } else if (
    typeof exports === 'object' &&
    typeof exports.nodeName !== 'string'
  ) {
    // CommonJS
    factory(exports, require('b'));
  } else {
    // Browser globals (root is window)
    factory((root.commonJsStrict = {}), root.b);
  }
})(this, function (exports, b) {
  exports.action = function () {};
});
```

1. `exports`와 `module`이 존재하면 **CommonJS** 방식
2. `define`이 함수고 `define.amd`가 존재할 경우 **AMD** 방식
3. 둘 다 아니라면 **window** 객체에 모듈을 내보냄

### ESM

ES6에서 추가 된 JS 모듈 기능

### 특징

- 비동기적
- `import`, `export`를 사용하여 모듈 불러오기 및 내보내기 구현
- 브라우저에서 import와 export를 지원하지 않아 번들러를 함께 사용해야 함 (webpack, rollup 등)
  - `<script type="module" src="index.mjs">`
- Node.js 에서는 package.json에 `"type": "module"`추가
