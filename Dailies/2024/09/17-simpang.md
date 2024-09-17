### 2024.09.17

### Simpang

#### comment

- 모바일 화면에서 인풋 클릭시 화면 확대

  - 모바일 환경에서 font-size가 16px 미만일 경우, 사용자의 편의를 위해 기본으로 줌으로 처리됨
  - 해결 방법
    - head의 mata설정
      ```html
      <meta
        name="viewport"
        content="width=device-width, initial-scale=1.0, user-scalable=no" />
      ```
      - width=device-width: 뷰포트의 너비를 디바이스의 화면 너비에 맞추도록 설정
      - initial-scale=1.0: 페이지가 처음 로드될 때의 확대/축소 비율을 설정. 1.0은 100%로 기본값
      - user-scalable=no: 사용자가 핀치 줌이나 더블 탭을 통해 페이지를 확대하거나 축소할 수 없도록 설정. no의 경우 사용자가 브라우저를 통해 확대/축소 하는것을 방지시킴.
    - font-size 16px이상으로 변경
  - 선택
    - font-size 16px로 지정. `user-scalable=no`를 사용할 경우 다른곳에서도 영향을 끼칠 수 있으며, 화면에 글자크기를 높여 사용자에게도 더 잘 보이게 하는게 맞다고 생각

- comment의 input가 빈텍스트일 때 처리
  - 조건문 추가  
    `if (text) postComment({ id: contentId, text });`
