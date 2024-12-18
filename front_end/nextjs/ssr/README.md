---
> [공식문서 링크](https://nextjs.org/docs/pages/building-your-application/rendering/server-side-rendering)
---

- 서버사이드 렌더링의 경우, 각 페이지 `요청이 올 때 HTML을 미리 생성`한다.
- 서버사이드 렌더링은 `"SSR"` 혹은 `다이나믹 렌더링(Dynamic Rendering)`이라고도 불린다.
- 서버사이드렌더링은 `자주 바뀌거나` `외부API`를 받아오는 경우에 사용하면 좋다.

--- 
### **서버사이드렌더링(Server-side-rendering) 사용법**
- 키워드 `export ` , `async `
- 함수명 `getServerSideProps`

서버사이드렌더링은  위의 2개의 JS 키워드와 함수명이 있으면 된다.
해당 `SSR메서드는 페이지 메서드 보다 먼저 실행`되고, 반환되는 값을 페이지 메서드의 인자값으로 넘겨준다.
> *call sequence*
> `getServerSideProps()` -> `Page(ssrData)` -> `렌더링` -> `요청응답 반환`

- 예제 코드

```
export default function Page({ data }) {
  // 렌더링할 페이지 로직
}
 
// 매 요청마다 불려진다
export async function getServerSideProps() {
  // 외부 API 로직
  const res = await fetch(`https://.../data`)
  const data = await res.json()
  ...
  //페이지 메서드로 넘겨줄 props 데이터 반환
  return { props: { data } }
}
```
