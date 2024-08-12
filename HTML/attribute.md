# HTML 전역 속성

### 전역 특성

전역 특성 (Global attributes)은 모든 HTML에서 공통으로 사용할 수 있는 특성

- 모든 HTML 요소에 지정 가능 ( 표준에 명시되지 않은 요소에도 지정 가능)

### 목록

> - [data-\*](#data-attributes)
> - [lang](#lang)
> - [style](#style)

## data-attributes

- 사용자 지정 데이터 특성 (custom data attributes)이라는 특성 클래스를 형성하여 임의의 데이터를 스크립트로 HTML과 DOM 사이에서 교환할 수 있는 방법

- 목적
  - 적절한 표준 속성이나 요소가 없을 때 사용하는 HTML의 표준화된 속성
- 사용

  - 자바스크립트와 CSS에서 속성을 활용하여 동적인 기능을 추가하거나 스타일을 지정하는데 사용

- `data-*`의 `*`은 *XML 이름 생성규칙*을 따르는 모든 이름으로 대체 가능

  > **XML 이름 생성규칙**
  >
  > - 대소문자 여부에 상관없이 `xml`로 시작하면 안됨
  > - 세미콜론(`u+003A`)을 포함해서는 안됨
  > - 대문자를 포함해서는 안됨

- HTML 속성이기 때문에 CSS의 선택자로 접근 가능

  ```html
  <div
    id="product-12345"
    data-name="운동화"
    data-price="10000"
    data-stock="5"
    data-color="red">
    운동화
  </div>
  ```

  ```css
  [data-name] {
    /*  CSS의 선택자로 접근 */
    padding: 1em;
    background-color: yellowgreen;
  }
  [data-color='red'] {
    /*  CSS의 선택자로 접근 */
    color: red;
  }
  ```

[TOP](#)

## lang

- 요소 컨텐츠와 텍스트가 포함된 모든 요소 속성에 대한 기본언어를 지정
- 스크린 리더, 브라우저, 검색 엔진등에서 사용해야하는 언어 정보를 제공

> 참고 : `lang`의 기본값은 "알 수 없음"이므로 항상 값을 지정하는 것이 좋음

### 장점

- 스크린 리더가 웹페이지의 언어를 인식, 해당 언어에 맞는 음성 제공
- 크롬에서 자동 번역 기능을 사용할 때, 언어를 자동으로 감지
- 검색 엔인지 웹페이지의 언어를 인식, 해당 언어로 검색 결과를 제공

### 예시

1. 최상단 태그에 사용 (기본 언어)

```html
<html lang="ko"></html>
```

2. 페이지에서 사용

```html
<p>안녕하세요. <span lang="en">Sim Pang</span>입니다</p>
```

### 주의

- lang 속성을 사용하게 될 경우, 콘텐츠뿐 아니라 `title`속성의 텍스트도 해당 언어로 인식됨
- 잘못 된 예
  ```html
  <a lang="es" title="스페인어" href="">Español</a>
  ```
- 올바른 예
  ```html
  <a lang="es" title="스페인어" href="">
    <span lang="es">Español</span>
  </a>
  ```

### Memo

- 크롬에서 한국어인데 자동 번역이 실행됨.
- 문제의 원인은 lang의 초깃값이 en으로 설정되어 있었다. 주의하자!

[TOP](#)

## style

- 직접 요소에 스타일을 적용하는 **인라인 스타일**
  - 인라인 스타일은 다른 선택자보다 구체성 값이 가장 높기 때문에 다른 스타일 규칙을 무시할 수 있음. 가급적 사용을 권장하지 않음
- 주로 테스트등 빠른 스타일링을 위한 목적으로 사용
- 인라인 스타일을 사용해야 할 경우
  - 이메일 발송을 HTML 템플릿으로 할 경우 해당 HTML 템플릿에 인라인 스타일만 적용 가능
  - 이메일에서 중요한 보안정책의 이유로 외부 CSS 적용 방법을 지원하지 않음

[TOP](#)

## 참고

- codingeverybody : https://codingeverybody.kr/category/html/
- MDN : https://developer.mozilla.org/ko/docs/Web/HTML
