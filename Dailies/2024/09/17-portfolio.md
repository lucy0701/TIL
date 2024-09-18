### 2024.09.17

## Portfolio

#### TODO

- main 내부 컴포넌트 분리
  - info, interview, skills, project
  - 컵포넌트의 유지보수와 복잡한 css를 분리시키기 위함
- Data Type 분리

  - 컴포넌트별 사용 용도에 따라 분리

- 기존 FlatStep 컴포넌트에서 step을 index로 처리했으나, category.id를 사용하여 직접적으로 접근하도록 변경

  - 기존

    ```tsx
    const [step, setStep] = useState(1);

    const handleStepClick = (targetStep: number) => {
      setStep(targetStep);
      const sectionElement = document.getElementById(`section-${targetStep}`);

      if (sectionElement) {
        sectionElement.scrollIntoView({ behavior: 'smooth' });
      }
    };

    // ~~ 생략 ~~

    if ('title' in section) {
      return (
        <button
          key={i}
          className={step === i + 1 ? styles.current : ''}
          onClick={() => handleStepClick(i + 1)}>
          {section.title}
        </button>
      );
    }
    ```

  - 변경

    ```tsx
    const [step, setStep] = useState(info.id);

    const handleStepClick = (targetStep: string) => {
      setStep(targetStep);
      const sectionElement = document.getElementById(targetStep);

      if (sectionElement) {
        sectionElement.scrollIntoView({ behavior: 'smooth' });
      }
    };

    // ~~ 생략 ~~

    if ('category' in section) {
      return (
        <button
          key={i}
          className={step === datas[category].id ? styles.current : ''}
          onClick={() => handleStepClick(datas[category].id)}>
          {section.category}
        </button>
      );
    }
    ```

### 문제 발생

페이지의 뷰가 `section-1`에 있을 때, `section-4`를 클릭하면, 네비게이션 버튼이 제대로 작동하지 않음
![image](https://github.com/user-attachments/assets/da584c3e-c04a-44f8-965b-9d3b2ca13e53)

- Info(section-1) -> Project(section-4)

![image](https://github.com/user-attachments/assets/64dca76d-e45f-4616-b8a9-904cca91b6a6)

> 로그를 찍어보니, 1 -> 4 -> 3 -> 4 이렇게 상태가 업데이트 됨. 난 분명 4번을 눌렸을 뿐인데!!

### 원인

- 버튼과 스크롤바 이벤트가 같은 상태를 업데이트하고 있었다.

  ```ts
  const handleStepClick = (targetStep: string) => {
    setStep(targetStep);
    // ~~ 생략 ~~
  };

  const handleScroll = useCallback(() => {
    // ~~ 생략 ~~
    if (section1Top < viewHeight / 2 && section1Top >= 0) {
      setStep(info.id);
    }
  }, []);
  ```

- section-4번 버튼을 클릭하면 상태는 section-4로 업데이트 되고, 스크롤바가 이동하며 section-3과 section-4를 다시 업데이트 했던 것.

### 해결

`handleStepClick`에서 `setStep(info.id);`삭제

- 왜? 삭제만으로 해결 됨?
  ```ts
  const sectionElement = document.getElementById(targetStep);
  if (sectionElement) {
    sectionElement.scrollIntoView({ behavior: 'smooth' });
  }
  ```
  - handleStepClick 함수는 해당 엘리먼트의 아이디를 찾아가기 때문에 상태 업데이트가 필요없음...

> 헷갈리니까 step을 section으로 통일시키고 자야겠다.
