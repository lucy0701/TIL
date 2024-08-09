## Redirect Error

<img src="https://github.com/user-attachments/assets/b0b80a5a-300a-472a-ba7c-98329bee4d61">

### 원인

- 다른 페이지로 리디렉션되는 비표준 URL

  ```ts
  {
  url: 'https://simpang.kr/about/',
  lastModified: new Date(),
  },
  {
  url: 'https://simpang.kr/about/dev',
  lastModified: new Date(),
  },
  ```

  - `https://simpang.kr/about/`의 끝에 `/`가 있어 색인이 제대로 생성되지 않았음

### 해결

- URL 끝의 `/`를 제외한 후 색인 요청

### 배운점

- URL 끝의 슬래시 처리: 일관성을 유지하는 것이 중요
  - url 끝에 슬래시(`/`)를 **일관되게 사용하거나 사용하지 않도록 설정**
    - URL 끝에 슬래시가 있거나 없을 경우 검색엔진은 두 URL을 별개의 페이지로 인식하여 중복 컨텐츠가 발생할 수 있음
