### 목록

- [SSR](#ssr)
- [SSG](#ssg)
- [ISR](#isr)
- [정리](#정리)
- [Pre-Rendering](#pre-rendering)
- [fallback](#fallback)
- [Next 13부터 달라진 점](#next-13부터-달라진-점)

## SSR

- 사용자가 요청할 때 마다 그 시점에 페이지를 새롭게 렌더링
- Fetching 데이터가 빈번하게 변경될 때 사용
- `fetchData` 함수 사용
  - `{cache : 'no-store'}`를 사용하여 매 요청마다 데이터를 호출

## SSG

- 빌드시에 HTML를 만들고 각 요청 시 캐싱된 데이터를 재사용 함
- SSG에서 HTML은 `next build` 명령어를 사용할 때 생성
- SSG는 빌드 시에 페이지를 생성하고, 데이터가 변경되면 다시 빌드를 해야함
- CDN으로 캐시가 되고 요청마다 HTML을 재사용
- 동적 라우팅 시에는 `generateStaticParams`를 사용
  - 페이지 라우터는 `getStaticPaths`와 `getStaticProps`를 사용했으나 앱라우터로 업데이트 된 후 `generateStaticParams`를 사용
  - 예시
    ```js
    export async function generateStaticParams() {
      return [{ id: '1' }, { id: '2' }];
    }
    ```

## ISR

- SSG에 포함되는 개념. SSG와 차이점은 설정한 시간마다 페이지를 새로 렌더링 함
- ISR은 일정 시간마다 특정 페이지만 다시 refetch 하여 페이지를 재생성 함
- revalidate 속성 사용 (시간 설정)

  ```js
  const res = await fetch(`https://example.kr/${params.id}`, {
    next: { revalidate: 15 },
  });
  ```

  > 페이지 라우트의 경우 getStaticProps 함수 내부에서 사용

## 정리

- SEO 적용이 중요하지 않거나 데이터 Pre-rendering이 필요없다면 **`CSR`**
- 정적 문서로 빠른 HTML 문서로드가 필요하다면 **`SSG`**
- 컨텐츠 데이터가 동적이지만 자주 변경되지 않을 경우 **`ISR`**
- 매 요청마다 데이터가 달라지면서 서버 사이드로 렌더링 해야할 땐 **`SSR`**

## Pre-Rendering

- Next.js는 렌더링을 할 때 기본적으로 Pre-rendering(사전 렌더링)을 수행
- 사전렌더링은 서버단에서 DOM요소를 Build하여 HTML 문서를 렌더링

## fallback

```js
export const dynamicParams = true;
```

- fallback는 `true` | `false` | `blocking` 값을 가짐
- `true`: 빌드시에 생성되지 않은 적적 페이지 요청하는 경우, fallback 페이지 제공. 서버에서 정적페이지를 생성, 생성되면 사용자에게 해당페이지 제공
- `false` : 빌드시에 생성되지 않은 정적 페이지 요청, 404 페이지 응답
- `blocking` : 빌드 시에 생성되지 않은 정적 페이지를 요청하는 경우, SSR방식으로 제작한 정적 페이지를 제공

## Next 13부터 달라진 점

- `getStaticPaths`, `getStaticProps`, `getServerSideProps`가 없어짐
- **`getStaticPaths`** 대신 **`generateStaticParams`** 를 사용하여 SSG를 생성
- getServerSideProps, getStaticProps 대신 getData() 로 데이터를 불러옴
- getStaticPaths내부에서 사용하던 fallback도 **`export const dynamicParams = true;`** 으로 사용 방법 병경
