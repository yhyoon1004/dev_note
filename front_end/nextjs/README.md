# NEXTJS의 CSR(클라이언트 사이드 렌더링) 방식

---

## 동작원리
- `초기 웹` : 서버에서 `완성된 html`과 `js`,`css`  전송
- `REACT / NEXTJS CSR` : 서버에서 `빈 html`과 `js`파일과 `css`를 전송,  
    브라우저가 `빈 html`에 `js파일로 html문서를 렌더링`하여 보여줌
---

## 페이지 이동시
- `초기 웹` : 해당하는 html 문서를 작성하여 `js`,`css`와 함께 전송
- `REACT / NEXTJS CSR` :  
  - 이동하려는 페이지가 `csr`일 경우 : 이동할 페이지의 `JS번들`이 있는 지 확인 후, 없으면 해당 번들을 요청후 로드
  - 이동하려는 페이지 `ssr/ssg`일 경우 : 마찬가지로 `JS번들` 확인 후 없으면 번들요청 +   
서버에서 데이터를 제공시 서버에 데이터 요청