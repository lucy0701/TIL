### 2024.09.18

## Portfolio

### window.innerHeight

브라우저 창 세로 높이

### getBoundingClientRect()

Dom요소의 크기와 위치를 반환하는 메서드  
top, bottom, left, right 을 사용하여 해당 뷰에서 얼마나 떨어졌는지를 알 수있음  
뷰에서 거리기준은 top과 left임 그래서 bottom과 right의 경우 해당 요소의 너비와 높이만큼 출력 됨

> 브라우저 뷰의 기준이므로 요소의 크기가 `%`, `vh`등 일 경우나 스크롤바로 요소가 움직일 경우, 거리는 변경 됨

width, height는 요소의 크기를 말함

#### 사용

- `window.innerHeight`를 사용하여 요소가 일정 높이를 만났을 때 이벤트를 실행하게 할 수있음

#### 예시

- 사용 예시

  ```ts
  const handleScroll = useCallback(() => {
    const viewHeight = window.innerHeight;
    const section = document.getElementById(info.id);

    if (section1) {
      const sectionTop = section1.getBoundingClientRect().top;
      if (sectionTop < viewHeight / 2 && section1Bottom >= 0) {
        setTargetSection(info.id);
      }
    }
  }, []);

  useEffect(() => {
    window.addEventListener('scroll', handleScroll);

    return () => {
      window.removeEventListener('scroll', handleScroll);
    };
  }, [handleScroll]);
  ```

- top과 bottom

  ```ts
  const handleScroll = useCallback(() => {
    const viewHeight = window.innerHeight;

    const section1 = document.getElementById(info.id);

    if (section1) {
      const section1Top = section1.getBoundingClientRect().top;
      const section1Bottom = section1.getBoundingClientRect().bottom;

      if (section1Top < viewHeight / 2 && section1Bottom >= viewHeight / 2) {
        setTargetSection(info.id);
      }
    }
  }, []);
  ```

  - 첫 예시의 경우 스크롤바가 올라갈 때, 요소의 top부분이 뷰포트를 만나야 했기 때문에 원하는대로 구현이 안됨. bottom을 추가하여 bottom의 경계가 뷰포트를 만났을 때에도 이벤트가 발생할 수 있도록 함

### CSS background-image

- img태그와 차이점
  - 배경적 디자인요소로 활용 됨. 패턴의 반복이나 배경 고정 등
  - DOM 요소로 인식 안됨. 검색 불가
  - ALT 속성 없음
  - CSS요소로 인식 됨
  - 즉, 컨텐츠 요소는 img, 디자인은 background-image를 사용 함

#### background-attachment

- scroll : 기본 값
- fixed : 고정. 스크롤바가 움직여도 이미지는 고정되어 보임
- local : 스크롤바의 영향을 받지만, 해당 요소의 영역내부에서 받음. overflow를 사용하여 해당 요소 내부의 스크롤바의 영향을 받게 함

#### 내가 구현 하고 싶었던 것!

이미지는 고정되어 있고, 스크롤바가 내려가면 해당이미지가 점점 확대 되는 것!

> 이미지는 왜 고정? 스크롤바가 내려가는 것이 아니라 다음 요소가 올라가는 듯한 느낌을 하고 싶었음

- CSS

  ```css
  .wrap {
    width: 100%;
    height: 100vh;

    background-image: url('/images/gradients_3.png');
    background-size: 100%; /* 기본사이즈 100% */
    background-position: center; /* 가운데 기준으로 확대 */
    background-attachment: fixed; /* 고정 */
    transition: background-size 0.3s ease; /* 부드럽게~ */
  }
  ```

  - `background-size: cover;` : 처음 이미지의 비율을 위해 커버를 사용했으나, 사이즈 조절 불가함을 알게 됨
  - `background-size: 100% 100%;` : 첫번째는 가로, 두번째는 세로지만 두개다 사용 할 경우 이미지가 깨짐. 그래서 뺌! (미작성시 auto)

* JS

  ```ts
  useEffect(() => {
    const background = document.querySelector(
      `.${styles.wrap}`,
    ) as HTMLElement | null;

    if (background) {
      const handleScroll = () => {
        const scrollTop = window.scrollY;
        const backgroundSize = 100 + scrollTop * 0.1;

        background.style.backgroundSize = `${backgroundSize}%`;
      };

      window.addEventListener('scroll', handleScroll);

      return () => {
        window.removeEventListener('scroll', handleScroll);
      };
    }
  }, []);
  ```

  - background : 해당 요소 클래스 명
  - scrollTop : 수직 스크롤 위치
  - backgroundSize
    - 100 : `background-size: 100%`; 베이스 크기를 맞춰 줌
    - scrollTop \* 0.1 : 크기 조절
  - `background.style.backgroundSiz` : size 냅다 꼽기

> 대충 요론 느낌으로, `innerHeight`, `scrollY`, `getBoundingClientRect()`등을 활용하여 스크롤바 이벤트를 적용 시켜 봤다.

#### 응용편 `background-clip`

- 텍스트 내부에 이미지 넣기! 그리고 이미지 고정하기!

```css
.mainTitle {
  background-image: url('/images/gradients_1.png');
  background-size: cover;
  background-attachment: fixed;
  background-clip: text;
  -webkit-background-clip: text;

  .title,
  .name {
    color: transparent;
    transition: all 1s ease;
  }
}
```

- background-clip

  - border-box: 기본값. border 영역까지 배경 표시
  - padding-box: padding까지 확장
  - content-box: content까지 확장
  - text: 텍스트 생상, 배경색이 배경에 따라 다르게 보임

- color은 꼭 투명하게 해야 배경이 보임!

이렇게 하면 스크롤바가 움직일 때, 글자색이 화려해짐! (딱히 쓸 일은 없을듯)