# Debounce and Throttle

> 연속적으로 함수가 반복 실행될 때, 성능을 최적화를 위해 이벤트를 제어하는 두 가지 주요 기술

- **차이점**

  - **Debounce**
    - 연속된 호출이 멈춘 후 마지막 호출만 실행
    - 일정 시간 동안 이벤트가 발생하지 않으면 함수 실행
  - **Throttle**
    - 일정 시간 간격으로 한 번만 함수 호출
    - 주어진 시간동안에 함수 호출 발생 X, 최초 호출 이후에 정해진 주기마다 호출 발생

- **라이브러리**
  - Lodash : `debounce`와 `throttle` 기능을 제공하는 유틸리티 라이브러리
  - RxJS: `debounceTime`과 `throttleTime` 연산자를 통해 비슷한 기능을 제공

### Debouncing

- **동작 방식**

  1. 함수가 호출될 때 타이머 실행
  2. 타이머가 완료되기 전에 함수가 재호출 되면, 이전 타이머를 취소하고 새 타이머를 설정
  3. 타이머가 만료되면, 타이머가 설정된 함수가 호출 됨

- ex. 자동완성 : 사용자가 입력을 멈춘 후 마지막 입력만 처리

```js
function debounce(func, delay) {
  let timer;
  return function (...args) {
    if (timer) {
      clearTimeout(timer);
    }
    timer = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}
```

### Throttling

- **동작 방식**

  1. 함수가 호출되면 즉시 실행
  2. 일정 시간동안 추가 호출 무시
  3. 설정된 시간이 지나면 함수가 다시 호출 될 수 있음

- ex. 스크롤 이벤트 : 페이지 스크롤 시, 일정 시간 간격으로 스크롤 위치를 기록하거나 화면을 업데이트

```js
function throttle(func, delay) {
  let lastCall = 0;

  return function (...args) {
    const now = Date.now();
    if (now - lastCall < delay) return;

    lastCall = now;
    return func.apply(this, args);
  };
}
```

<br>

---

### 프로젝트 적용

- 좋아요 버튼 연속 클릭 막기

* 첫 시도 : Debounce

  ```ts
  const useDebounce = <T>(
    callback: (args: T) => void,
    timeout = 300,
  ): ((args: T) => void) => {
    const timerRef = useRef<NodeJS.Timeout | null>(null);

    const debouncedCallback = (args: T) => {
      if (timerRef.current) {
        clearTimeout(timerRef.current);
      }
      timerRef.current = setTimeout(() => {
        callback(args);
      }, timeout);
    };

    return debouncedCallback;
  };

  export default useDebounce;
  ```

  - 디바운스의 경우, 이벤트가 실행되고 일정 시간이 지나야하기 때문에 부적합 했다.

* 두번 째 시도: Throttling

  ```ts
  const useThrottle = <T>(
    callback: (args: T) => void,
    limit = 300,
  ): ((args: T) => void) => {
    const inThrottle = useRef(false);

    const throttledCallback = (args: T) => {
      if (!inThrottle.current) {
        callback(args);
        inThrottle.current = true;
        setTimeout(() => {
          inThrottle.current = false;
        }, limit);
      }
    };

    return throttledCallback;
  };

  export default useThrottle;
  ```

  - 디바운스와 쓰로틀링을 공부하고 보니, 이건 쓰로틀링을 쓰는게 맞는것 같았다.
