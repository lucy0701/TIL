# Proxy와 Reflect

## Proxy

- 특정 객체를 감싸 객체의 기본동작(읽기, 쓰기 등)을 가로채어 제어할 수 있게 해주는 기능
- 객체의 동작을 제어하거나 디버깅, 유효성 검사, 로깅 등의 작업을 수행할 때 유용
- Proxy는 JS 내장 객체로, '**특수 객체(exotic object)**'이며, 프로퍼티가 없는 것이 특징
- **Handelr**: `Proxy`를 생성할 때, 이 객체의 동작을 정의하는 'handler' 객체 제공, handler객체는 다양한 **트랩(trap)** 을 포함하며, 각 트랩은 객체의 기본 동작을 가로채는 메서드이다.

### 기본 문법

```js
let proxy = new Proxy(target, handler);
```

- Proxy는 객체를 생성할 때 두 가지 인자를 전달

  - targt: 감싸게 될 객체로, 함수를 포함한 모든 객체가 가능
  - handler: 타겟의 기본 동작을 정의하는 '**트랩(trap)**'이 담긴 객체

- Proxy가 작업이 시작되면, Handler에 트랩이 있을 경우 트랩이 실행
- Handler에 트랩이 없을 경우, taget으로 바로 전달되어 target이 작업을 직접 수행

```js
let target = {};
let proxy = new Proxy(target, {});

proxy.test = 5; // 프록시 값을 씀 -- 1
alert(target.test); // 5, target의 새로운 프로퍼티
alert(proxy.test); // 5, 프록시를 사용해 값을 읽을 수 있음 -- 2
for (let key in proxy) alert(key); // text, 반복 작동 -- 3
```

1. proxy.test= 를 이용해 target에 새로운 값 설정
2. proxy.test 를 이용해 값을 읽으면 target에서 값을 읽어옴  
   ![alt text](https://github.com/user-attachments/assets/a938b1e8-87a1-4766-86d9-35d938765d19)
3. proxy를 대상으로 반복 작업을 하면 target을 둘러싸는 투명한 래퍼가 됨
   ![image](https://github.com/user-attachments/assets/05741cfc-ce75-4c4b-a986-06f70ddd3ad7)
   > 출처 : [모던 JavaScript](https://ko.javascript.info/proxy)

### 트랩을 사용해 가로챌 수 있는 작업

- 객체에서 어떤 작업을할 땐 내부 메서드(interal method)를 사용한다. 예를 들어 프로퍼티를 읽을 땐 [[Get]], 작성할 땐 [[Set]] 등의 내부 메서드가 관여하는데, 이런 메서드는 개발자가 직접 사용해 호출 할 수는 없다 (명세서에만 정의된 메서드)

- 프록시의 내부 트랩은 이 내부 메서드의 호출을 가로챈다.

  - new Proxy의 handler에 매개변수로 추가할 수 있는 메서드
    ![alt text](https://github.com/user-attachments/assets/8ebd21db-d02c-44d1-a6f2-67c12ada30a9)

  - **주요 트랩(Trap) 메서드**
    - `get(target, property, receiver)`: 프로퍼티 값을 읽을 때 호출
    - `set(target, property, value, receiver)`: 프로퍼티 값을 설정할 때 호출
    - `has(target, property)`: in 연산자를 사용할 때 호출
    - `deleteProperty(target, property)`: delete 연산자를 사용할 때 호출
    - `apply(target, thisArg, argumentsList)`: 함수를 호출할 때 호출
    - `construct(target, argumentsList, newTarget)`: 생성자 함수 호출 시 호출
    - `ownKeys(target)`: Object.keys, Object.getOwnPropertyNames, Object.getOwnPropertySymbols 등의 메서드 호출 시 호출

- **규칙**
  - 값을 쓰는 게 성공적으로 처리되면 [[Set]]은 반드시 true를 반환. 실패할 경우 false 반환
  - 값을 지우는 게 성공적으로 처리되면 [[Delete]]는 반드시 true를 반환. 실패할 경우 false 반환
    등등...
  - 프락시 객체를 대상으로 [[GetPrototypeOf]]가 적용되면 프락시 객체의 타깃 객체에 [[GetPrototypeOf]]를 적용한 것과 동일한 값이 반환되어야 한다. 프록시의 프로토타입을 읽는 것은 타깃 객체의 프로토타입을 읽는 것과 동일해야함

> 주의  
> `dictionary = new Proxy(dictionary, ...);`  
> 타겟 객체의 위치와 상관없이 객체는 타깃 객체를 덮어야한다.  
> 객체를 프록시로 감싼 이후엔 절대로 타겟 객체를 참조하는 코드가 없어야 함
