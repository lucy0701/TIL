# JavaScript

> 간단한 기초 설명

### 목록

> - [JavaScript란?](#javascript란)
> - [script 태그 위치](#script-태그-위치)
> - [동등 연산자와 일치 연산자](#동등-연산자와-일치-연산자)
> - [Function](#function)
> - [Event](#event)
> - [API](#api)
> - [Interpreted versus compiled code](#interpreted-versus-compiled-code)
> - [Server side versus client side code](#server-side-versus-client-side-code)
> - [Types of error](#types-of-error)
> - [Variables](#variables)

[TOP](#)

## JavaScript란?

- HTML 문서(정적 페이지)에 동적 상호 작용을 제공할 수 있게 해주는 완전한 동적 프로그래밍 언어(dynamic programming language)
- 인터프리터 컴파일 프로그래밍 언어
- 일급 함수 지원
- 프로토타입 기반, 다중 패러다임, 단일 스레드, 동적 언어로 객체지향형, 명령형, 선언형(함수형 프로그래밍 등) 스타일 지원

## script 태그 위치

1. #### 속성이 미사용

- `<head>` 태그 사이에 위치

  ```html
  <html>
    <head>
      <script src="./index.ts"></script>
    </head>
    <body>
      <h1>Head 사이에 위치</h1>
    </body>
  </html>
  ```

  - HTML이 파싱완료 되기전 JS가 먼저 실행 됨
  - JS 실행문이 Document 속성이 필요할 경우, 오류 발생 (아직 렌더트리가 없기 때문)

- `<body>` 마지막에 위치

  ```html
  <html>
    <head></head>
    <body>
      <h1>Body 마지막에 위치</h1>
      <script src="./index.ts"></script>
    </body>
  </html>
  ```

  - HTML 파싱이 완료 된 후 실행

> - HTML파싱 도중 JS를 만나게 되면 JS가 실행 됨
> - HTML은 파싱을 멈추고 JS의 작업이 완료되고 다시 파싱을 이어 감
> - 이유는 자바스크립트는 **싱글스레스 언어**이기 때문 (한번에 하나의 작업을 처리)

2. #### 속성 사용 (보통 속성을 사용 함)
   > 두 속성 모두 브라우저의 백그라운드에서 다운로드 됨
   > 다운로드 중에도 HTML 파싱은 멈추지 않음
   > 차이점은 실행 시점

- **defer**
  - HTML 파싱이 모두 완료 된 뒤 JS파일 실행 (위치 상관 X)
  - HTML 파싱 / JS 파일 다운 -> DOM 노드트리 -> JS 실행 -> DOMContentLoaded
- **async**
  - JS의 파일의 다운로드가 완료되면 바로 실행
  - async 스크립트가 실행중에는 HTML 파싱 멈추지 않음
  - 실행은 다운로드가 끝난 스크립트 순서
  - 주로 서드 파티 스크립트(외부 스크립트)를 사용할 때 유용

[TOP](#)

## 동등 연산자와 일치 연산자

- `=` : 할당
  - 값을 변수에 할당 함
- `==` : 일치 연산자
  - 타입만 확인 함. 피연산자의 타입이 다를경우 타입을 강제 변환
  - ex
    - `0.1 + 0.3 == 0.3` => `true`
    - `undefind == null` => `true`
- `===` : 동등 연산자
  - 값의 내용 모두 확인
    - `0.1 + 0.3 == 0.3` => `false`
    - `undefind == null` => `false`

[TOP](#)

## Function

- 재사용을 원하는 기능을 담는 방법
- `()`를 사용하여 실행
- arguments : 함수가 작동하는데 필요한 데이터. 하나 이상의 인수는 콤마`,`로 구분
  > return문은 브라우저에게 함수로부터 나오는 result 변수를 반환하게 하여 그 변수를 사용할 수 있게 함
  > 함수 안에서 정의된 변수는 그 함수 내부에서만 사용 가능하기 때문(변수 scoping)에 return을 통해 외부로 전달

[TOP](#)

## Event

웹사이트의 상호작용에 필요 함

```js
document.querySelector('html').onclick = function () {
  alert('안녕!');
};
```

- HTML 요소를 선택하고 onclick 핸들러 프로퍼티에 클릭 이벤트가 실행할 익명함수 할당

### 이벤트 할당

```js
function createParagraph() {
  const para = document.createElement('p');
  para.textContent = 'You clicked the button!';
  document.body.appendChild(para);
}
```

#### 인라인

- html안에 JS코드를 직접 삽입

  ```html
  <button onclick="createParagraph()">Click me!</button>
  ```

  - 코드의 재사용성이 떨어지고, HTML 코드가 더러워지기 때문에 추천하지 않음

#### addEventListener

- 순수한 JavaScript 구문을 사용
- querySelectorAll()를 사용하여 내부 Button에 이벤트를 추가

  ```js
  const buttons = document.querySelectorAll('button');
  for (const button of buttons) {
    button.addEventListener('click', createParagraph);
  }
  ```

  - 재사용성을 높이고, 분리된 코드로 유지/보수에 효율적

[TOP](#)

## API

- 브라우저 API

  - 웹브라우 내장 API

  * DOM
    - HTML, CSS를 조작하여 HTML 요소를 생성, 제거, 스타일링, 이벤트등을 다룰 수 있게 해줌
  * Geolocation API
    - 지리 정보를 가져 옴. Google 지도가 사용자의 위치를 찾아 지도에 표시하는 방법
  * Canvas와 WebGL API
    - 애니메이션 2D 및 3D 그래픽을 만들 수 있음
  * HTMLMediaElement와 WebRTC를 포함하는 오디오와 비디오 API
    - 웹 페이지에서 바로 오디오 및 비디오를 재생하거나 웹 카메라에서 비디오를 가져와 다른 사람의 컴퓨터에 표시하는 등 멀티미디어 작업 수행 가능

- 서드파티 API
  - 외부에서 코드와 정보를 가져옴
    - Google 지도 API와 OpenStreetMap API로 웹 사이트에 지도를 삽입등

[TOP](#)

## Interpreted versus compiled code

### 인터프리터

- 코드가 위에서 아래로 실행되며, 실행 결과는 바로 반환 됨
- 브라우저에서 코드가 실행 되기 전, 다른 형태로 변환할 필요없음
- 코드는 텍스트 형식으로 수신되에 처리 바로 됨

### 컴파일러

- 프로그램이 실행되기 전 다른 형태로 변환(컴파일) 됨
- C/C++에서는 코드를 기계어로 변환하고 변환된 결과를 컴퓨터가 실행 함
- 프로그램은 프로그램의 원본 소스 코드에서 실행된 이진 형식(바이너리)으로부터 실행 됨

### JavaScript는 인터프리터를 사용하는 프로그래밍 언어

- 웹브라우저는 JavaScript 코드를 텍스트형식으로 입력 받아 처리함
- 대부분의 모던 JavaScript 인터프리터들은 성능 향상을 위해 JIT컴파일을 사용
  - **JIT**(just-in-time)
  - 스크립트의 *실행과 동시*에 *소스 코드를 이진 형태로 변환*하여 최대한 높은 실행 속도를 얻는 방법
  - 컴파일처럼 미리 처리되는 것이 아닌, **런타임에 처리되기 때문에 JS는 인터프리터 언어로 분류**

[TOP](#)

## Server side versus client side code

### 서버 사이드 코드

- 서버에서 코드가 실행되고, 결과가 브라우저에 표시 됨
- 인기 있는 서버 사이드 웹 언어로는 PHP, Python, Ruby, ASP.NET, 그리고 JavaScript가 있음
  -JavaScript는 브라우저 뿐만 아니라, Node.js 환경처럼 서버 사이드 언어로도 사용할 수 있음

### 클라이언트 코드

- 사용자의 컴퓨터에서 실행되는 코드
- 웹페이지를 볼 때, 페이지의 코드가 다운로드 되고 실행되어 화면에 표시 됨

[TOP](#)

## Types of error

- 구문 오류
  - 코드에 오타가 있을 경우 오류
  - 코드가 작동하지 않거나, 중간에 멈춤
- 논리 오류
  - 구문은 올바르지만 의도한 동작과 실제 코드에 차이가 있을 때
  - 프로그램은 성공적으로 돌아가지만 결과가 잘못 됨

[TOP](#)

## Variables

- 값을 저장하는 컨테이너

### 1.변수 선언

```js
let name;
let age;
```

- 값이 없이 선언만 하게 되면 값은 undefined가 할당 됨

[TOP](#)

### 1.변수 초기화

- 선언 후 초기화

  ```js
  name = 'lucy';
  age = 20;
  ```

- 값을 넣어 초기화 함

  ```js
  let name = 'lucy';
  let age = 20;
  ```

- 선언과 초기화를 동시에 할 수 있음 (주로 이렇게 함)

> - 선언 키워드(const, let)를 사용하지 않고 초기화를 하게 될 경우, 해당 변수는 전역으로 처리됨
> - 상수(const)의 경우 한 번 초기화가 되고나면 값을 변경 할 수없기 때문에, 선언과 초기화를 함께 해야 함

[TOP](#)

### var

- 재선언 가능
- 함수 스코프 레벨이므로, if, for등 에서 선언해도 외부에서 호출 가능함

### 변수 이름 규칙

- 라틴 문자(0-9, a-z, A-Z)와 언더바(`_`) 문자를 사용
- 변수 이름의 맨 앞에 언더바(`_`), 숫자 사용 X
- lower camel case: (ex) `const userAge`
- JavaScript 예약어 사용 X : (ex) `var`, `function`, `let`, `for`

### 동적 타입

- JavaScript는 동적 타입이므로, 변수 선언시 변수의 타입을 따로 지정하지 않아도 됨
- JS에서 변수는 런타임 때, 값을 읽어 타입을 추론 하기 때문

[TOP](#)
