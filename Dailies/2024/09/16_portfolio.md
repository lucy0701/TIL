### 2024.09.16

### Portfolio

## Project Data Type 변경

- 기존
  ```JSON
  "projects": {
      "id": "section-4",
      "title": "Project",
      "contents": [
      {
          "id": "projects-1",
          "title": "Project",
          "name": "심팡 (Sim Pang)",
          "content": [
          "프로젝트 소개 및 팀원 및 역할",,
          "사용 기술",
          "특징",
          ],
          "skills": ["JavaScript"],
          "images": ["/images/1.png", "/images/2.png", "/images/3.png"]
      }
      ]
  }
  ```
- 변경

  ```JSON
  "projects": {
      "id": "section-4",
      "title": "Project",
      "contents": [
      {
          "id": "projects-1",
          "title": "Project",
          "name": "심팡 (Sim Pang)",
          "content": [
            { "info": "프로젝트 소개 및 팀원 및 역할", "image": "/images/seo.png"},
            { "skills": "사용 기술", "image": "/images/seo.png" },
            { "summary": "특징", "image": "/images/seo.png" }
          ]
      }
      ]
  ```

- 설명
  - 기존 content, skills, images 프로퍼티를 하나의 객체인 content로 결합
  - image의 경우 content와 함께 처리하는 것이 맞다고 생각
  - 해당 컨텐츠별 구성에 따른 컴포넌트를 다르게 사용할 수있기 때문에 인덱스 보다는 키를 사용하는 것이 좋다고 생각

#### Code

- 변경 전

  ```tsx
  {
    project.content.map((content, j) => (
      <div key={j} className={styles.projectBox}>
        <div className={styles.projectLeftBox}>
          <div className={styles.textContainer}>
            <p
              ref={(el) => {
                boxRefs.current[i * project.content.length + j] = el!;
              }}
              className={`${styles.content} ${styles.visible}`}>
              {content}
            </p>
            {j === 0 &&
              project.skills.map((skill) => <p key={skill}>{skill}</p>)}
          </div>
        </div>

        <div className={styles.projectRightBox}>
          <div>{project.images[j]}</div>
        </div>
      </div>
    ));
  }
  ```

- 변경 후

  ```tsx
  {
    project.content.map((content, j) => (
      <div key={j} className={styles.projectBox}>
        <div className={styles.projectLeftBox}>
          <div className={styles.textContainer}>
            <p
              key={j}
              ref={(el) => {
                boxRefs.current[i * project.content.length + j] = el!;
              }}
              className={`${styles.content} ${styles.visible}`}>
              {j === 0 && content.info}
              {j === 1 && content.skills}
              {j === 2 && content.summary}
            </p>
          </div>
        </div>

        <div className={styles.projectRightBox}>
          <Image
            src={content.image}
            alt='project image'
            width={700}
            height={900}
          />
        </div>
      </div>
    ));
  }
  ```

- 수정이 필요한 부분

  ```tsx
  <p
    key={j}
    ref={(el) => {
      boxRefs.current[i * project.content.length + j] = el!;
    }}
    className={`${styles.content} ${styles.visible}`}>
    {j === 0 && content.info}
    {j === 1 && content.skills}
    {j === 2 && content.summary}
  </p>
  ```

  - 각 content 별 컴포넌트 구성 후, 적용 예정
  - 이미지의 경우 한 개의 설명에 여러 개의 이미지가 들어올 수 있다는 점도 고려

---

#### Type

```ts
type ContentItem = {
  info?: string;
  skills?: string;
  summary?: string;
  image: string;
};

export interface ProjectData {
  id: string;
  title: string;
  contents: {
    id: string;
    title: string;
    name: string;
    content: ContentItem[];
  }[];
}
```

> ContentItem를 직접 작성하지 않고 Data의 key를 자체 타입으로 적용하고 싶었으나, 실패함. 추가 학습 필요.