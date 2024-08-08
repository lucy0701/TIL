# Markdown 문법 정리

## 제목

```markdown
# 제목 1

## 제목 2

### 제목 3
```

## 강조

- _이탤릭체_ 또는 _이탤릭체_
  **굵은 글씨** 또는 **굵은 글씨**
  ~~취소선~~

```markdown
_이탤릭체_ 또는 _이탤릭체_
**굵은 글씨** 또는 **굵은 글씨**
~~취소선~~
```

## 목록

### 순서가 있는 목록

```markdown
1. 항목 1
2. 항목 2
   1. 하위 항목 1
   2. 하위 항목 2
```

### 순서가 없는 목록

```markdown
- 항목 1
- 항목 2
  - 하위 항목 1
  - 하위 항목 2
```

## 링크

- [OpenAI](https://www.openai.com)

```markdown
[OpenAI](https://www.openai.com)
```

## 이미지

```markdown
![대체 텍스트](https://www.example.com/image.jpg)

<img src="https://www.example.com/image.jpg" alt="대체 텍스트" width="300" height="200" >
```

## 코드

### 인라인 코드

- `코드`

```markdown
`코드`
```

### 블록 코드

````markdown
```js
const hello = 'Hello, world!';
console.log('Hello, world!');
```
````

## 인용

> 인용문
>
> > 인용문2
> >
> > > 인용문3

```markdown
> 인용문
>
> > 인용문2
> >
> > > 인용문3
```

## 네비게이션

```markdown
[목차](#목차)
```

## 수평선

```markdown
---
```

## 표

| 헤더 1   | 헤더 2   |
| -------- | -------- |
| 데이터 1 | 데이터 2 |
| 데이터 3 | 데이터 4 |

```markdown
| 헤더 1   | 헤더 2   |
| -------- | -------- |
| 데이터 1 | 데이터 2 |
| 데이터 3 | 데이터 4 |
```

## 토글

<details>
  <summary>
    제목
  </summary>
    내용
</details>

```markdown
<details>
<summary>
  제목
</summary>
   내용
</details>
```
